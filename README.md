# Dapp Funds X

Dapp Funds X is a full-stack Web3 crowdfunding application where users can create charity campaigns and receive on-chain donations.

The project combines:

- Next.js + TypeScript frontend
- Solidity smart contract (Hardhat)
- Wallet connection (RainbowKit + Wagmi)
- SIWE authentication with NextAuth
- Redux state management

## Project Insight

This app is built around a smart contract-first donation model:

- Campaigns are created on-chain and stored in the `DappFund` contract.
- Donations are made directly on-chain.
- A platform tax is automatically split to the contract owner per donation.
- Campaign visibility is controlled by status (`deleted`, `banned`, funding progress).

The frontend reads and writes campaign data through ethers.js contract calls and presents those actions through a wallet-connected UX.

## Core Features

### Smart Contract Features

- Create a campaign with title, owner details, media, and goal amount
- Update campaign details (owner only)
- Delete campaign (owner only)
- Ban/unban campaigns (platform owner only)
- Donate to active campaigns
- Track campaign supporters and donation history
- Configurable platform tax (`changeTax`)

### Frontend Features

- Wallet connection (MetaMask, Trust Wallet, Coinbase Wallet, Rainbow)
- SIWE-based authentication using NextAuth
- Landing page for discoverable active campaigns
- Campaign detail page with donation and supporter history
- Create and edit campaign pages
- User-specific campaigns page
- Transaction feedback via toast notifications

## Tech Stack

- Next.js 13
- React 18
- TypeScript
- Tailwind CSS
- Hardhat + ethers v6
- Solidity 0.8.17
- RainbowKit + Wagmi + Viem
- NextAuth + SIWE
- Redux Toolkit

## Prerequisites

- Node.js 18+
- Yarn 1.x
- MetaMask (or any supported EVM wallet)

## Local Setup

1. Clone and install dependencies.

```bash
git clone <your-repo-url>
cd Web3-HackOn
yarn install
```

2. Create your environment file (`.env`) in project root.

```env
NEXT_PUBLIC_RPC_URL=http://127.0.0.1:8545
NEXT_PUBLIC_ALCHEMY_ID=your_alchemy_api_key
NEXT_PUBLIC_PROJECT_ID=your_walletconnect_project_id
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=replace_with_a_strong_random_secret
```

3. Start a local blockchain node.

```bash
yarn blockchain
```

4. Deploy the contract (new terminal).

```bash
yarn deploy
```

This updates `contracts/contractAddress.json` with the deployed address.

5. (Optional) Seed sample campaigns and donations.

```bash
yarn seed
```

6. Run the Next.js app.

```bash
yarn dev
```

Open http://localhost:3000.

## Useful Commands

```bash
yarn dev          # Run app in development
yarn build        # Production build
yarn start        # Run production server
yarn lint         # Lint code
yarn blockchain   # Start hardhat local node
yarn deploy       # Deploy DappFund contract
yarn seed         # Seed demo data
npx hardhat test  # Run smart contract tests
```

## Render Deployment Guide

Render should host the Next.js frontend as a Web Service.

Important: a Render-hosted app cannot use your local Hardhat node (`127.0.0.1:8545`). For production, deploy your contract to a public network (for example Bitfinity testnet as configured in `hardhat.config.js`) and point your RPC env var to that network.

### 1) Deploy contract to a public network

1. Configure deployer account in `hardhat.config.js` for your target network.
2. Deploy:

```bash
npx hardhat run scripts/deploy.js --network bitfinity
```

3. Commit the updated `contracts/contractAddress.json` so Render uses the correct contract address.

### 2) Create Render Web Service

1. Push your repository to GitHub.
2. In Render dashboard, click New + -> Web Service.
3. Select your repository and configure:
   - Runtime: Node
   - Build Command: `yarn install && yarn build`
   - Start Command: `yarn start`

### 3) Add Environment Variables in Render

Set these values in Render (Environment tab):

- `NEXT_PUBLIC_RPC_URL` = Public RPC URL of deployed chain
- `NEXT_PUBLIC_ALCHEMY_ID` = Alchemy API key (if used)
- `NEXT_PUBLIC_PROJECT_ID` = WalletConnect Project ID
- `NEXTAUTH_URL` = Your Render app URL (for example `https://your-app.onrender.com`)
- `NEXTAUTH_SECRET` = Long random secret string

### 4) Deploy

1. Trigger deployment from Render.
2. After deployment, open the app URL and test wallet connection + campaign flows.

## Testing

Run contract tests:

```bash
npx hardhat test
```

## Notes

- Keep private keys and secrets out of source control.
- If you redeploy the contract, remember to update `contracts/contractAddress.json`.
- Ensure wallet network matches the chain used by `NEXT_PUBLIC_RPC_URL`.

## License

This project is available for use. Please check with the repository owner for specific licensing information.