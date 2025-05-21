## 第四章：Web3 前端 101

### 监听合约事件并更新 UI 的示例代码


```javascript
const provider = new ethers.providers.Web3Provider(window.ethereum);
const contract = new ethers.Contract(contractAddress, abi, provider);

// 监听购买事件
contract.on("PixelPurchased", (buyer, x, y, color, event) => {
    // 解析事件参数
    const pixelX = x.toNumber();
    const pixelY = y.toNumber();
    const pixelColor = `#${color.toString(16).padStart(6, '0')}`;

    // 更新 UI
    updatePixelGrid(pixelX, pixelY, pixelColor);
});

// 更新像素格子的 UI 函数
function updatePixelGrid(x, y, color) {
    const pixelElement = document.querySelector(`.pixel[data-x="${x}"][data-y="${y}"]`);
    if (pixelElement) {
        pixelElement.style.backgroundColor = color;
    }
}
```
