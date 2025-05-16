## 第一章：走进 Web3 世界

1. 简单描述一下本地开发、部署合约的流程

   以使用 foundry 为例：

   - a. 初始化项目：forge init newProject；
   - b. 编写合约：src 文件夹，编写 .sol 合约文件；
   - c. 编译并测试合约：
     - test 文件夹，编写 .sol 测试文件；
     - forge test
   - d. 部署到测试网络：
     - script 文件夹，编写 .sol 脚本文件；
     - forge script ...

2. 简单描述一下用户在使用一个 DApp 时与合约交互的流程

   - i. 连接钱包
   - ii. 触发交互
   - iii. 签名交易
   - iv. 等待合约执行
   - v. 确认结果

3. 通读[「区块链黑暗森林自救手册」](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README_CN.md)，列出你觉得最重要的三个安全技巧

   - A. 备份助记词，“乱序规律”，甚至把某个把单词替换为其他的单词；
   - B. 备份时考虑灾备，灾备人 + 多处备份，使用 GPG；
   - C. 当目标页面出现 HTTPS 错误证书提醒时，就立即停止继续访问、关闭页面。
