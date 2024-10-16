# PumpFun Monitor

PumpFun Monitor is a TypeScript project that monitors the PumpFun program on the Solana blockchain. It tracks the creation of new tokens and their initial buy transactions.

## Features

- Connects to the Solana mainnet
- Listens for logs from the PumpFun program
- Parses transaction data to extract token creation details
- Monitors initial token buys
- Provides real-time console output for new token creations

## Prerequisites

- Node.js (v14 or later recommended)
- npm (comes with Node.js)

## Installation

1. Clone the repository:
   ```
   git clone https://github.com/tarik0/pumpfun-monitor.git
   cd pumpfun-monitor
   ```

2. Install dependencies:
   ```
   npm install
   ```

## Configuration

The project uses default constants for monitoring. If you need to adjust these, you can modify the following values in the `src/index.ts` file:

```typescript
const PUMPFUN_PROGRAM_ID = new PublicKey("PFv6nYW5Zcn2P9rHpxnDWfHkf6LsxfD6Uy6ew8qLNVi");
const COMMITMENT: Commitment = "confirmed";
const RPC_ENDPOINT = "https://api.mainnet-beta.solana.com";
```

## Running the Monitor

To start the PumpFun Monitor, run the following command:

```
npm start
```

This will compile the TypeScript code and start the monitor. The program will continuously run and output information about new token creations to the console.

## How it Works

1. The monitor establishes a connection to the Solana mainnet.
2. It subscribes to logs from the PumpFun program.
3. When relevant logs are detected, it fetches the full transaction data.
4. The transaction data is parsed to extract details about the newly created token.
5. Information about the new token, including initial SOL and token balances, is logged to the console.

## Important Code Sections

- Program ID and Constants:
  ```typescript
  const PUMPFUN_PROGRAM_ID = new PublicKey("PFv6nYW5Zcn2P9rHpxnDWfHkf6LsxfD6Uy6ew8qLNVi");
  const COMMITMENT: Commitment = "confirmed";
  const RPC_ENDPOINT = "https://api.mainnet-beta.solana.com";
  ```

- Solana Connection Setup:
  ```typescript
  const connection = new Connection(RPC_ENDPOINT, {
      commitment: COMMITMENT,
      wsEndpoint: RPC_ENDPOINT.replace("https", "wss"),
  });
  ```

- Main Monitoring Loop:
  ```typescript
  const main = async () => {
      const subscription = await connection.onLogs(
          PUMPFUN_PROGRAM_ID,
          (logs, ctx) => handleLogs(logs, ctx),
          COMMITMENT
      );
      console.log("Subscribed to pumpfun program");
      console.log(subscription);
      while (true) {
          await new Promise((resolve) => setTimeout(resolve, 1000));
      }
  };
  ```