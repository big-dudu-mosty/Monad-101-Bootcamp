## 第四章：Web3 前端 101

1. 完成课程的实操后，请思考如何监听合约事件；当有别人购买了像素格子的时候，如何及时通过监听事件更新 UI ? 请提交示例代码

答：合约在完成购买后，触发事件，前端监听事件，然后进行更新
```
.sol
event SquareBougth(uint8 indexed idx, uint color, address indexed buyer);
...
function buysquare(uint8 idx, uint color) public payable {
  ...
  emit SquareBougth(idx, color, msg.sender);
}

.js

contract.events.SquareBougth({
    fromBlock: 'latest'
}, function(error, event) {
    if (error) {
        console.error(error);
    } else {
        console.log(event);
        const index = event.returnValues.idx;
        const color = event.returnValues.color;
        const cell = document.querySelector(`.cell:nth-child(${parseInt(index) + 1})`);
        cell.style.backgroundColor = '#' + parseInt(color).toString(16).padStart(6, '0');
    }
})
```