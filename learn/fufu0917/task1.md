## 第一章：走进 Web3 世界

1. 简单描述一下本地开发、部署合约的流程

- 本地开发一般可以在云上 remix 平台，或者自己本地 vscode/cursor 这种 IDE 上开发，这些平台能解决代码静态编译 sol 代码过程的问题，会有 ABI 文件生成
- 部署的话可以通过 remix 或者其他 js 包直接部署当前 sol 代码，然后 deploy 到指定的 EVM，才能在链上运行，生出当前部署的合约地址

2. 简单描述一下用户在使用一个 DApp 时与合约交互的流程

- 1.用户访问浏览器，通过钱包比如 MetaMask 和 DApp 建立连接
- 2.DApp 会在操作过程中，去读写链上的合约 ABI 方法，然后消费 Gas 费，或者链上数据

3. 通读[「区块链黑暗森林自救手册」](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README_CN.md)，列出你觉得最重要的三个安全技巧

- 不要相信任何链接和地址，保持怀疑态度，多查看再确认下一步
- 所见即所签，保存好自己的私钥/钱包/助记词
- 使用去中心化交易所，中心化交易随时会被盗窃和因为合规等被封
