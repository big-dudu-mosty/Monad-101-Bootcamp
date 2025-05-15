1. 完成课程的实操后，请思考如何监听合约事件；当有别人购买了像素格子的时候，如何及时通过监听事件更新 UI ? 请提交示例代码

## 解决方案

### 1. 智能合约部分

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract PixelGrid {
    uint256 constant GRID_SIZE = 100;
    uint256 constant PRICE = 0.01 ether;

    struct Pixel {
        string color;
        address owner;
    }

    mapping(uint256 => mapping(uint256 => Pixel)) public pixels;

    // 定义购买像素格子的事件
    // 使用过去时态 Purchased 表示动作已完成
    event PixelPurchased(
        uint256 indexed x,
        uint256 indexed y,
        address indexed buyer,
        string color,
        uint256 price
    );

    // 购买格子函数
    function mint(uint256 x, uint256 y, string memory color) public payable {
        require(x < GRID_SIZE && y < GRID_SIZE, "Coordinates out of bounds");
        require(msg.value >= PRICE, "Insufficient payment");
        require(pixels[x][y].owner == address(0), "Pixel already minted");

        pixels[x][y] = Pixel(color, msg.sender);
        emit PixelPurchased(x, y, msg.sender, color, msg.value);
    }
}
```

### 2. 前端实现

#### 2.1 初始化Web3连接

```javascript
import { ethers } from 'ethers';

// 合约ABI（仅包含必要的事件部分）
const contractABI = [
    "event PixelPurchased(uint256 indexed x, uint256 indexed y, address indexed buyer, string color, uint256 price)",
    "function pixels(uint256 x, uint256 y) view returns (string color, address owner)"
];

// 初始化provider和contract
const provider = new ethers.providers.Web3Provider(window.ethereum);
const pixelContract = new ethers.Contract(contractAddress, contractABI, provider);
```

#### 2.2 设置事件监听

```javascript
async function setupEventListeners() {
    try {
        // 创建事件过滤器
        const filter = pixelContract.filters.PixelPurchased();
        
        // 监听PixelPurchased事件
        pixelContract.on(filter, (x, y, buyer, color, price, event) => {
            console.log(`像素在坐标(${x}, ${y})被${buyer}购买`);
            updatePixelOnUI(x.toNumber(), y.toNumber(), color, buyer);
        });

        // 错误处理
        pixelContract.on('error', (error) => {
            console.error('事件监听器错误:', error);
            // 5秒后重试
            setTimeout(setupEventListeners, 5000);
        });
    } catch (error) {
        console.error('设置事件监听器失败:', error);
        setTimeout(setupEventListeners, 5000);
    }
}
```

#### 2.3 UI更新函数

```javascript
function updatePixelOnUI(x, y, color, buyer) {
    const pixelElement = document.getElementById(`pixel-${x}-${y}`);
    if (pixelElement) {
        pixelElement.style.backgroundColor = color;
        pixelElement.title = `所有者: ${buyer}`;
    } else {
        // 如果元素不存在，动态创建
        const gridContainer = document.getElementById('pixel-grid');
        const newPixel = document.createElement('div');
        newPixel.id = `pixel-${x}-${y}`;
        newPixel.className = 'pixel';
        newPixel.style.backgroundColor = color;
        newPixel.title = `所有者: ${buyer}`;
        gridContainer.appendChild(newPixel);
    }
}
```

#### 2.4 页面初始化

```javascript
window.addEventListener('load', async () => {
    if (window.ethereum) {
        try {
            // 请求用户连接钱包
            await window.ethereum.request({ method: 'eth_requestAccounts' });
            await setupEventListeners();
        } catch (error) {
            console.error('初始化失败:', error);
        }
    } else {
        console.error('请安装MetaMask!');
    }
});
```