# Omikuji dApp

A Web3 Omikuji (Japanese fortune slip) dApp that lets users connect their wallet and draw a fortune by interacting with a deployed Solidity smart contract.

## Run & Operate

- `pnpm --filter @workspace/omikuji run dev` — run the frontend (port 19044)
- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite, Tailwind CSS, wagmi v2, viem
- API: Express 5
- DB: PostgreSQL + Drizzle ORM (not yet used)

## Where things live

- `artifacts/omikuji/` — React/Vite frontend dApp
- `artifacts/omikuji/src/lib/contract.ts` — Contract ABI, address, fortune data
- `artifacts/omikuji/src/pages/OmikujiPage.tsx` — Main interactive page
- `artifacts/api-server/` — Shared Express API server

## Architecture decisions

- Frontend-only: all contract interaction happens client-side via wagmi/viem; no backend needed
- Contract address: `0x29edac435c9bb0a6ccbb11c038961d57df51c32c` (configurable in `contract.ts`)
- Fortune result is read from the `OmikujiDrawn` event in the transaction receipt (EVM doesn't return values from txs)
- Supports multiple chains: mainnet, Sepolia, Base, Polygon, Optimism, Arbitrum
- Uses `injected()` connector (MetaMask and compatible wallets)

## Product

Users can connect their MetaMask wallet, pay 0.00002 ETH, and draw a fortune from 6 possible results (大吉 to 凶). The fortune is revealed with an animated paper scroll unfurling animation.

## Gotchas

- The user must be on the correct network where the contract is deployed
- The fortune result is parsed from the `OmikujiDrawn` event, not the function return value
- Always run codegen after OpenAPI spec changes (not currently needed)

## Pointers

- See the `pnpm-workspace` skill for workspace structure and TypeScript setup
