## 第三章：第四章：Web3 前端 101

1. 完成课程的实操后，请思考如何监听合约事件；当有别人购买了像素格子的时候，如何及时通过监听事件更新 UI ? 请提交示例代码

### 合约端   
```
contract PixelGrid {
    address public owner;
    uint public PRICE = 0.01 ether;
    uint[100] public squares;

    // 事件定义：购买像素格子的事件
    event PixelPurchased(address indexed buyer, uint8 idx, uint color);

    constructor() {
        owner = msg.sender;
    }

    function buySquare(uint8 idx, uint color) public payable {
        // 检查索引有效性
        require(idx >= 0 && idx < 100, "Invalid index");
        // 检查支付是否足够
        require(msg.value >= PRICE, "Insufficient payment");

        // 将支付的以太币发送给合约拥有者
        (bool sent, ) = owner.call{value: msg.value}("");
        require(sent, "Failed to send Ether");

        // 更新格子颜色
        squares[idx] = color;

        // 触发 PixelPurchased 事件
        emit PixelPurchased(msg.sender, idx, color);
    }
}
```


### 前端代码   

```
// PixelGrid.jsx (React + Ethers.js)

import React, { useEffect, useState } from 'react';
import { ethers } from 'ethers';

// 合约地址和 ABI
const CONTRACT_ADDRESS = '0xYourContractAddress';
const CONTRACT_ABI = [
  "event PixelPurchased(address indexed buyer, uint8 idx, uint color)",
  "function squares(uint256 idx) external view returns (uint)"
];

export default function PixelGrid() {
  const [pixels, setPixels] = useState({}); // 用于存储每个格子的颜色
  const [provider, setProvider] = useState(null); // Ethers.js provider
  const [contract, setContract] = useState(null); // 合约实例

  useEffect(() => {
    // 连接到 Web3 提供者
    const web3Provider = new ethers.providers.WebSocketProvider("wss://mainnet.infura.io/ws/v3/YOUR_INFURA_PROJECT_ID");
    const contractInstance = new ethers.Contract(CONTRACT_ADDRESS, CONTRACT_ABI, web3Provider);

    // 初始化 provider 和 contract
    setProvider(web3Provider);
    setContract(contractInstance);

    // 监听 PixelPurchased 事件
    const handlePixelPurchased = (buyer, idx, color) => {
      console.log(`Pixel ${idx} purchased by ${buyer} with color: ${color}`);
      setPixels(prev => ({
        ...prev,
        [idx]: color
      }));
    };

    contractInstance.on("PixelPurchased", handlePixelPurchased);

    // 清理事件监听器
    return () => {
      contractInstance.off("PixelPurchased", handlePixelPurchased);
    };
  }, []);

  // 渲染像素网格
  const renderGrid = () => {
    const grid = [];
    for (let i = 0; i < 100; i++) {
      const color = pixels[i] || 0xffffff; // 默认颜色为白色
      grid.push(
        <div
          key={i}
          style={{
            width: 30,
            height: 30,
            backgroundColor: `#${color.toString(16).padStart(6, '0')}`,
            border: '1px solid #ccc',
            display: 'inline-block'
          }}
        />
      );
    }
    return grid;
  };

  return (
    <div>
      <h3>实时更新的像素格子</h3>
      <div style={{ display: 'flex', flexWrap: 'wrap', width: '300px' }}>
        {renderGrid()}
      </div>
    </div>
  );
}
```