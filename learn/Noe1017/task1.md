## 第一章：走进 Web3 世界

1. 简单描述一下本地开发、部署合约的流程                                                              
    a. 环境准备
        * -安装Node.js (v16+)
        * -配置开发工具：
            - Hardhat：`npm install --save-dev hardhat`
            - Foundry
    b. 项目初始化
        *- npx hardhat init 
        *- 生成核心目录：
            contracts/ - 存放Solodity合约代码
            scripts/   - 部署脚本
            test/      - 测试用例
    c. 编写智能合约
        在contracts/中创建.sol文件，示例子：
        {
            ^pragma solidity ^0.8.0;
            contract SimpleStorage{
                    unit storeData;

                    function set(unit x) public{
                        storedData = x;
                    }

                    function get() public view returns (unit) {
                        return storedData;
                    }
            }
        }
    d. 本地测试
        ``` bash
        npx hardhat node #生成10个测试账户
        * 运行测试脚本
        npx hardhat test --network loclahost
        * 使用Chai/Mocha编写单元测试（在test/目录）
    e. 部署合约
        创建部署脚本 (`scripts/deploy.js`):
        ```javascript
        async function main() {
            const Contract = await ethers.getContractFactory("SimpleStorage");
            const contract = await Contract.deploy();
            await contract.deployed();
            console.log("合约地址", contract.address);
        }
        main();
        执行部署：
        npx hardhat run scripts/deploy.js --network localhost
    f. 生产环境部署
        配置网络 (`hardhat.config.js`):
        ``` javascript
        networks: {
            sepolia:{
                url: "https://rpc.sepolia.org",
                accounts: [privateKey] //私钥
            }
        }

        获取测试代币:
            example: faucets.chain.link
        部署到测试网络：
            npx hardhat run scripts/deploy.js --network sepolia
2. 简单描述一下用户在使用一个 DApp 时与合约交互的流程                                                
 用户界面 -> 钱包签名✍️ -> 区块链网络 -> 合约执行 -> 状态更新
3. 通读[「区块链黑暗森林自救手册」](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README_CN.md)，列出你觉得最重要的三个安全技巧 
    a.私钥与助记词的「物理隔离管理」
    b.合约交互的「最小授权原则」
    c.交易预览的「数据层验证」
    d.区块链的不可逆性意味着：一次失误=永久损失，安全必须前置到每笔交互的原子层面