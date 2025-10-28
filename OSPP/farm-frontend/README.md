# 🌾 Farm 3.0 Frontend

基于 Monad 测试网的去中心化农场游戏前端应用。

![Farm 3.0 Banner](https://via.placeholder.com/800x200/22c55e/ffffff?text=Farm+3.0+-+Blockchain+Farming+Game)

## 🎮 游戏概览

Farm 3.0 是一款完全在链上运行的农场游戏，玩家可以：

- 🌱 **种植收获**: 购买NFT种子，在100块土地上种植和收获作物
- 🤝 **互助合作**: 帮助其他农民获得KIND代币奖励
- 🏆 **竞争排名**: 在收获数量和善良值双排行榜上争取名次
- ⚡ **道具加速**: 使用浇水和施肥道具加速作物成长
- 🌦️ **天气系统**: 独立的天气影响每块土地的作物成长

## 🚀 快速开始

### 前置要求

- Node.js 18+
- npm 或 yarn
- MetaMask 或其他Web3钱包
- Monad 测试网络配置

### 安装依赖

```bash
npm install
```

### 开发环境运行

```bash
npm run dev
```

应用将在 `http://localhost:3000` 启动。

### 生产构建

```bash
npm run build
npm run preview
```

## 🏗️ 项目架构

### 技术栈

- **前端框架**: React 18 + TypeScript
- **状态管理**: Zustand (轻量级)
- **Web3集成**: Viem + Wagmi
- **样式方案**: TailwindCSS + 自定义组件
- **UI组件**: Headless UI + 自建组件库
- **构建工具**: Vite
- **代码规范**: ESLint + Prettier

### 目录结构

```
src/
├── components/          # 组件库
│   ├── ui/             # 基础UI组件
│   ├── layout/         # 布局组件
│   ├── game/           # 游戏特定组件
│   └── web3/           # Web3相关组件
├── pages/              # 页面组件
│   ├── FarmPage/       # 农场主页
│   ├── ShopPage/       # 种子商店
│   ├── LeaderboardPage/ # 排行榜
│   └── ProfilePage/    # 个人中心
├── hooks/              # 自定义Hook
│   ├── contracts/      # 合约交互Hook
│   ├── web3/           # Web3相关Hook
│   └── game/           # 游戏逻辑Hook
├── contracts/          # 合约ABI和配置
├── types/              # TypeScript类型定义
├── utils/              # 工具函数
└── constants/          # 常量配置
```

## 🎯 核心功能

### 1. 农场管理 (FarmPage)
- 10x10土地网格显示
- 实时土地状态和天气效果
- 种植、收获、偷菜操作
- 道具使用和互助功能

### 2. 种子商店 (ShopPage)
- 普通种子 (MON代币购买)
- 稀有种子 (KIND代币购买)
- 价格查询和余额检查
- 购买确认和交易状态

### 3. 排行榜系统 (LeaderboardPage)
- 收获数量排行榜
- KIND代币余额排行榜
- 实时排名更新
- 个人排名显示

### 4. 个人中心 (ProfilePage)
- NFT作物收藏展示
- 游戏统计和成就
- 钱包余额管理
- 活动历史记录

## 🔗 合约集成

### 部署地址 (Monad Testnet)

```typescript
const CONTRACTS = {
  FarmGame: "0xF6121A319b094c44f1B1D8A24BAd116D37C66E33",
  SeedNFT: "0x574a7B2b86d2957F1266A3F7F6eD586885512a05",
  LandNFT: "0x6D2145b588aD0ED722077C54Fa04c0fceEEf6643",
  KindnessToken: "0x8411b1120a1ADBd0f7270d70eCb55cfEa01984c1",
  Shop: "0xC7433bA91a619E7F028d1514bf1Acd3B709ea450"
}
```

### 网络配置

```typescript
const MONAD_TESTNET = {
  chainId: 10143,
  name: 'Monad Testnet',
  rpcUrl: 'https://testnet-rpc.monad.xyz',
  blockExplorer: 'https://testnet-explorer.monad.xyz'
}
```

## 🎨 设计系统

### 色彩方案
- **主色调**: 自然绿色系 (#22c55e)
- **状态颜色**:
  - 空闲: #6b7280 (灰)
  - 成长: #22c55e (绿)
  - 成熟: #f59e0b (金)
  - 冷却: #ef4444 (红)
- **稀有度颜色**:
  - 普通: #6b7280 (灰)
  - 稀有: #3b82f6 (蓝)
  - 传说: #a855f7 (紫)

### 组件规范
- 统一的卡片阴影和圆角
- 响应式网格布局
- 流畅的动画过渡
- 直观的状态指示

## 📱 响应式设计

- **桌面端**: 完整功能展示
- **平板端**: 适配布局调整
- **移动端**: 核心功能优化

## 🔧 开发工具

### 可用脚本

```bash
npm run dev          # 开发服务器
npm run build        # 生产构建
npm run preview      # 预览构建结果
npm run lint         # 代码检查
npm run lint:fix     # 自动修复代码问题
```

### 环境变量

创建 `.env.local` 文件：

```env
VITE_WALLETCONNECT_PROJECT_ID=your_project_id
VITE_APP_NAME=Farm 3.0
VITE_APP_DESCRIPTION=Blockchain Farming Game
```

## 🧪 测试

### 单元测试
```bash
npm run test
```

### E2E测试
```bash
npm run test:e2e
```

## 📦 部署

### Vercel部署
1. 推送代码到GitHub
2. 连接Vercel账户
3. 配置环境变量
4. 自动部署完成

### Netlify部署
1. 运行 `npm run build`
2. 上传 `dist` 文件夹
3. 配置重定向规则

## 🤝 贡献指南

1. Fork项目仓库
2. 创建功能分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add amazing feature'`)
4. 推送分支 (`git push origin feature/amazing-feature`)
5. 创建Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 🔗 相关链接

- [Monad官网](https://monad.xyz)
- [Monad测试网区块浏览器](https://testnet-explorer.monad.xyz)
- [项目文档](https://docs.farm3.xyz)
- [GitHub仓库](https://github.com/farm3-xyz)

## 📞 支持

如有问题或建议，请：
- 创建Issue: [GitHub Issues](https://github.com/farm3-xyz/issues)
- 加入Discord: [Farm 3.0社区](https://discord.gg/farm3xyz)
- 邮件联系: support@farm3.xyz

---

**🚀 Built with ❤️ on Monad Blockchain**