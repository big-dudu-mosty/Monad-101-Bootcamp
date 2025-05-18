## 第四章：Web3 前端 101

1. 完成课程的实操后，请思考如何监听合约事件；当有别人购买了像素格子的时候，如何及时通过监听事件更新 UI ? 请提交示例代码

contract BuyEarth is Ownable {
    
    event SquareBought(uint256 index, string value);//新增事件
    //原代码内容
    function buySquare(uint256 _index, string memory _value) public payable {
    //原代码内容
        emit SquareBought(_index, _value);//触发事件
    }
}

import { useState, useEffect } from 'react';
import { ethers, BrowserProvider, Contract } from 'ethers';

useEffect(() => {
      let contract;
      let eventListener;

      const init = async () => {
        try {
          contract = await getContract();

          // 创建事件过滤器（示例事件：SquareBought）
          eventListener = contract.on('SquareBought', (newValue, event) => {
            console.log('事件触发:', newValue.toString(), event);

            getSquare();  //更新 UI

          });
        } catch (error) {
          console.error('事件监听初始化失败:', error);
        }
      };

      init();
      console.log(events)
      return () => {
        if (eventListener) {
          contract.removeAllListeners('SquareBought'); // 清理监听器
        }
      };
    }, []);