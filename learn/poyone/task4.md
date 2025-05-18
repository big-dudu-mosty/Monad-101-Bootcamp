购买逻辑完成之后`emit`打下交易日志
```solidity
event ItemPurchased(address indexed buyer, uint256 tokenId, uint256 price);

function purchaseItem(uint256 tokenId) public payable {
    // 购买逻辑...
    emit ItemPurchased(msg.sender, tokenId, msg.value);
}
```

前端可以通过`contract.on("ItemPurchased", ...)`来监听这个变化
```js
const contract = new ethers.Contract(contractAddress, abi, provider);

// 监听事件
contract.on("ItemPurchased", (buyer, tokenId, price, event) => {
    if(tokenId === yourTokenId) {
        console.log(`Item ${tokenId} purchased by ${buyer} for ${price}`);
        // 更新UI
    }
});
```