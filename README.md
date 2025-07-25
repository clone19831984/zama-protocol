# Zama Protocol Deploy Contract
## Install Dependecies
#### Install Hardhat
```
sudo npm install -g hardhat
```
## Deploy FHECounter contract
### Clone Repo
```
git clone https://github.com/zama-ai/fhevm-hardhat-template
cd fhevm-hardhat-template
```
### Install:
```
npm install
```
### Replace hardhat.config.ts file:
```
rm hardhat.config.ts && nano hardhat.config.ts
```
```
import "@fhevm/hardhat-plugin";
import "@nomicfoundation/hardhat-chai-matchers";
import "@nomicfoundation/hardhat-ethers";
import "@nomicfoundation/hardhat-verify";
import "@typechain/hardhat";
import "hardhat-deploy";
import "hardhat-gas-reporter";
import type { HardhatUserConfig } from "hardhat/config";
import "solidity-coverage";
import "dotenv/config";

import "./tasks/accounts";
import "./tasks/FHECounter";

// Load environment variables
const SEPOLIA_RPC_URL: string = "https://ethereum-sepolia-rpc.publicnode.com";
const PRIVATE_KEY: string = process.env.PRIVATE_KEY || "";
const ETHERSCAN_API_KEY: string = process.env.ETHERSCAN_API_KEY || "";

if (!SEPOLIA_RPC_URL && process.env.HARDHAT_TARGET === "deploy") {
  throw new Error("SEPOLIA_RPC_URL is not set.");
}
if (!PRIVATE_KEY && process.env.HARDHAT_TARGET === "deploy") {
  throw new Error("PRIVATE_KEY is not set.");
}

const config: HardhatUserConfig = {
  defaultNetwork: "hardhat",
  namedAccounts: {
    deployer: {
      default: 0,
    },
  },
  etherscan: {
    apiKey: {
      sepolia: ETHERSCAN_API_KEY,
    },
  },
  gasReporter: {
    currency: "USD",
    enabled: !!process.env.REPORT_GAS,
  },
  networks: {
    hardhat: {
      chainId: 31337,
    },
    anvil: {
      url: "http://localhost:8545",
      chainId: 31337,
      accounts: PRIVATE_KEY ? [`0x${PRIVATE_KEY}`] : [],
    },
    sepolia: {
      url: SEPOLIA_RPC_URL,
      chainId: 11155111,
      accounts: PRIVATE_KEY ? [`0x${PRIVATE_KEY}`] : [],
    },
  },
  paths: {
    artifacts: "./artifacts",
    cache: "./cache",
    sources: "./contracts",
    tests: "./test",
  },
  solidity: {
    version: "0.8.24",
    settings: {
      metadata: {
        bytecodeHash: "none",
      },
      optimizer: {
        enabled: true,
        runs: 800,
      },
      evmVersion: "cancun",
    },
  },
  typechain: {
    outDir: "types",
    target: "ethers-v6",
  },
};

export default config;
```
### Compile
```
npx hardhat compile
```
### Deploy
```
read -p "🔑 Enter your private key: " PK && echo && PRIVATE_KEY=$PK npx hardhat deploy --network sepolia
```
### -------- Done!---------
Check your contract: https://sepolia.etherscan.io/

Fill form for: ✔️ Access to "Submit your project to participate to the monthly competition"

Here: https://docs.google.com/forms/d/e/1FAIpQLSfQ6_ZWv30cxfS3s6WqptwFBtvjms8k9HVK6OB9X_Ga4yHvZQ/viewform


