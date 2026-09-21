# Solana RPC Price Engine

A small n8n workflow for pulling Solana liquidity pool balances directly through RPC and calculating token prices from the pool reserves.

The goal is to get price data directly from on-chain pool state instead of relying only on third-party price APIs.

## Why I built it

Price APIs can be delayed, cached, or updated slower than the underlying pool.

This workflow reads token vault balances directly from Solana RPC, calculates the token's MEME/SOL price from the reserves, and then converts that price to USD using SOL/USD data from Dexscreener.

It also includes a paper trading system so price movement can be tested against take-profit and stop-loss conditions without executing real trades.

## Features

- Track multiple Solana pair addresses
- Automatically find pool token vault accounts
- Pull both token balances in a single RPC call
- Calculate token price from liquidity pool reserves
- Pull SOL/USD pricing from Dexscreener
- Convert MEME/SOL pricing into USD
- Simulate paper trades
- Take-profit and stop-loss logic
- Fee and slippage scenarios
- Shared bankroll across tracked pairs
- Track profit/loss and open positions

## How it works

The workflow first uses the pair address to find the relevant token vault accounts.

It then pulls both vault balances together using Solana RPC so both reserves come from the same snapshot.

The token price is calculated using:

```text
MEME/SOL price = SOL reserve / MEME reserve

## How to Use

1. Import the workflow JSON into n8n.
2. Add your Helius API key.
3. Add your wallet public key.
4. Add the Solana pair addresses you want to track to the watchlist.
5. Keep `activeTrading = false` and `mode = "paper"` while testing.
6. Run the workflow.

The bot will automatically find the pool vaults, calculate the token price from on-chain reserves, convert it to USD, and simulate the paper trade.
