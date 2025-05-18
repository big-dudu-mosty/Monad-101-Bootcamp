## 第四章：Web3 前端 101

1. 完成课程的实操后，请思考如何监听合约事件；当有别人购买了像素格子的时候，如何及时通过监听事件更新 UI ? 请提交示例代码

完善合约

event SquarePurchased(address indexed buyer, uint indexed idx, uint newColor);

声明SquarePurchased事件，该事件包含三个参数：
buyer：购买者地址
indexed：购买格子的索引
newColor：格子的新颜色

在 buySquare 函数的末尾，当所有条件都满足并且格子颜色更新后，添加了：

emit SquarePurchased(msg.sender, idx, color);

这会记录一个事件到区块链上，包含了调用者（购买者）、格子索引和新颜色。

前端监听合约
```js
contract.events.SquarePurchased()
  .on('data', (event) => {
    console.log('新格子被购买:', event.returnValues);
    const { idx, color } = event.returnValues;
    // 直接更新被購買的格子
    const pixel = grid.children[idx];
    if (pixel) {
      pixel.style.backgroundColor = `#${parseInt(color).toString(16)}`;
    }
  })
```