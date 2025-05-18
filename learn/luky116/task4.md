## 第四章：Web3 前端 101

1. 完成课程的实操后，请思考如何监听合约事件；当有别人购买了像素格子的时候，如何及时通过监听事件更新 UI ? 请提交示例代码

我们可以在 packages/hardhat/contracts/YourContract.sol 中添加相关代码来监听合约事件，代码如下：

```solidity

// 定义用户状态更新事件
event UserStatusUpdate(
    address indexed user,
    UserStatus status,
    uint256 timestamp
);

receive() external payable {
    // 触发事件
    emit UserStatusUpdate(msg.sender, userStatus[msg.sender], block.timestamp);
}
```

然后在 packages/nextjs/hooks 下新建一个 ts 文件，在里面监听事件，代码如下：
```typescript
publicClient.watchContractEvent({
  address: deployedContractData.address as Address,
  abi: deployedContractData.abi,
  eventName: "UserStatusUpdate",
  onLogs: logs => {
    logs.forEach(log => {
      const { args } = log as Log & {
        args: {
          user: Address;
          status: number;
          timestamp: bigint;
        };
      };
      console.log("New UserStatusUpdate event:", {
        user: args.user,
        status: UserStatus[args.status],
        timestamp: new Date(Number(args.timestamp) * 1000).toLocaleString()
      });
    });
}
```