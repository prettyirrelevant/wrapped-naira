# Wrapped Naira

This project provides a seamless fiat-to-crypto ramp, allowing users to easily convert Nigerian Naira (NGN) into a stablecoin ERC20 token. It also features a peer-to-peer exchange platform where merchants can list offers, and interested buyers can participate. The process is straightforward - users deposit NGN via a payment processor like Paystack, and the equivalent token amount is minted. The tokens are burned for withdrawals, and the corresponding fiat is sent back to the user's linked bank account. Crucially, the project ensures 1:1 backing of all funds, providing transparency and security.

## Folder Structure

```sh
.
├── README.md
├── afang (Subgraph files and configurations)
├── miyan (Solidity smart contracts)
├── ukwa (Node.js backend API)
└── wara (TypeScript React frontend)
```

## Architecture

```mermaid
sequenceDiagram
    participant User
    participant Paystack
    participant WNGNContract
    participant Server

    User->>Paystack: Sends money
    Paystack->>Server: Sends webhook
    Server-->>WNGNContract: Mints WNGN tokens
    Server-->>User: Allocates minted WNGN tokens to user's blockchain address

    User->>WNGNContract: Burns WNGN tokens
    WNGNContract->>Server: Emits Burn event
    Server-->>User: Sends equivalent fiat asset to user's linked bank account
```

## Utilities

- **Peer-to-peer stablecoin exchange**: This platform allows users to trade the WNGN token directly with each other, facilitating the growth of the token's liquidity. As the project gains more traction, liquidity pools will be created on popular decentralized exchanges (DEXes) to further improve the token's liquidity.

```mermaid
sequenceDiagram
    participant Merchant
    participant User
    participant P2PExchange
    participant WNGN
    participant USDC

    alt Merchant Buy Ad Flow
        Merchant->>P2PExchange: Create a BUY advertisement with price, WNGN quantity, and validity period
        P2PExchange->>WNGN: Transfer required WNGN tokens from Merchant to the contract
        P2PExchange-->>Merchant: Notify the Merchant that the BUY advertisement was created

        User->>P2PExchange: Participate in the BUY advertisement by providing the ad ID and the amount of USDC to sell
        P2PExchange->>USDC: Transfer the provided USDC amount from User to the Merchant
        P2PExchange->>WNGN: Transfer the calculated WNGN tokens from the contract to the User
        P2PExchange-->>User: Notify the User that the trade was executed

        alt Buy advertisement has expired and there are remaining WNGN tokens
            Merchant->>P2PExchange: Withdraw the remaining funds for the expired BUY advertisement
            P2PExchange->>WNGN: Transfer the remaining WNGN tokens from the contract to the Merchant
            P2PExchange-->>Merchant: Notify the Merchant that the funds were withdrawn
        end
    else Merchant Sell Ad Flow
        Merchant->>P2PExchange: Create a SELL advertisement with price, USDC quantity, and validity period
        P2PExchange->>USDC: Transfer required USDC tokens from Merchant to the contract
        P2PExchange-->>Merchant: Notify the Merchant that the SELL advertisement was created

        User->>P2PExchange: Participate in the SELL advertisement by providing the ad ID and the amount of WNGN to buy
        P2PExchange->>WNGN: Transfer the provided WNGN amount from User to the Merchant
        P2PExchange->>USDC: Transfer the calculated USDC tokens from the contract to the User
        P2PExchange-->>User: Notify the User that the trade was executed

        alt Sell advertisement has expired and there are remaining USDC tokens
            Merchant->>P2PExchange: Withdraw the remaining funds for the expired SELL advertisement
            P2PExchange->>USDC: Transfer the remaining USDC tokens from the contract to the Merchant
            P2PExchange-->>Merchant: Notify the Merchant that the funds were withdrawn
        end
    end
```

## Deployment Addresses

### 1. WNGN Token

- Scroll Sepolia - [0xaf97c3478abf6eeac933d3383b71668f314400aa](https://sepolia.scrollscan.com/address/0xaf97c3478abf6eeac933d3383b71668f314400aa)

- Scroll Mainnet - Coming soon 🚀

### 2. P2P Contract

- Scroll Sepolia - [0x3e3ff6bd166aca5837e3df8419991b3905c28fa2](https://sepolia.scrollscan.com/address/0x3e3ff6bd166aca5837e3df8419991b3905c28fa2)

- Scroll Mainnet - Coming soon 🚀

## API

### Technologies Used

- [Viem](https://viem.sh/)
- [Honojs](https://hono.dev/)
- [Supabase](https://supabase.com/)
- [Trigger.dev](https://trigger.dev/)
- [Subgraph](https://thegraph.com/en/)
- [Drizzle ORM](https://orm.drizzle.team/)
- [Cloudflare for deployment](https://www.cloudflare.com)

### URL

Backend is live @ [https://ukwa.ienioladewumi.workers.dev/](https://ukwa.ienioladewumi.workers.dev/) ✨


## Challenges Faced

During the development of this project, we encountered several obstacles related to the deployment of the subgraph. Despite multiple attempts, the subgraph deployment consistently failed, hindering our progress.

The first issue we faced was related to IPFS deployment. To overcome this hurdle, we utilized the Satsuma IPFS URL as an alternative solution. However, this workaround did not completely resolve the deployment problems.

Subsequently, we encountered difficulties when attempting to deploy the subgraph to the node. As a potential solution, we explored the possibility of switching to Alchemy's node, as they offer support for subgraphs. Unfortunately, as at the time of the hackathon, Alchemy did not provide support for the Scroll zkEVM blockchain, which was the platform we were targeting.
****
