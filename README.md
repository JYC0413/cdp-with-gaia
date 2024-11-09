# Coinbase Wallet agent with gaia

## Video demo

[![Video demo](https://img.youtube.com/vi/x9lde3Vfwpg/0.jpg)](https://www.youtube.com/watch?v=x9lde3Vfwpg)

## Installation

To get started, you'll need to install the required packages. Run the following commands:

```bash
pip install openai
pip install python-dotenv
pip install cdp-sdk
```

## Configure the agent

First, log into the [Coinbase developer portal](https://portal.cdp.coinbase.com/) and [create an API key](https://portal.cdp.coinbase.com/projects/api-keys).

Create a local file called `.env` set the following parameters.

```
CDP_API_KEY_NAME=<API KEY NAME FROM COINBASE DEV PORTAL>
CDP_PRIVATE_KEY=<PRIVATE KEY ASSOCIATED WITH THE API KEY>
CDP_WALLET_ID=<LEAVE EMPTY FOR NOW>
CDP_WALLET_SEED_FILE=wallet_seed.json
OPENAI_ENDPOINT=https://llamatool.us.gaianet.network/v1
OPENAI_API_KEY=GAIA
SWARM_DEFAULT_MODEL=llama
GAIA_CHAT_ENDPOINT=https://llama8b.gaia.domains/v1
GAIA_CHAT_MODEL=llama
```

* Here the `OPENAI_ENDPOINT`, `OPENAI_API_KEY`, and `SWARM_DEFAULT_MODEL` parameters point to a Gaia node running
the open-source Llama-3-Groq LLM, which is fine-tuned to support tool use.
* The `GAIA_CHAT_ENDPOINT` and `GAIA_CHAT_MODEL` parameters point to a Gaia node running
the open-source Llama 3.1 8B Instruct model for the agent to converse with the user.

## Set up an agent wallet

Create a new wallet and get 0.01 ETH from the base testnet.

```
python3 setup.py
```

Here is the result. You can click open the `basescan.org` link to see the newly created wallet address with a `0.01 ETH` balance.

```
Seed for wallet 688f2d8c-da3d-4ca0-a60d-6d7abd047242 saved to wallet_seed.json
Faucet transaction: FaucetTransaction: (transaction_hash: 0x2a3de94c4c7e12e2f13d66c3657a4ff19dbf8f9a9e272a68de0c53f018af3a20, transaction_link: https://sepolia.basescan.org/tx/0x2a3de94c4c7e12e2f13d66c3657a4ff19dbf8f9a9e272a68de0c53f018af3a20, status: pending, network_id: base-sepolia)
Agent wallet address: 0xb8AD0a228a4d51Fd13E6587c8815B208B509cEe2
```

Update the `.env` file to set the newly created Wallet ID.

```
CDP_WALLET_ID=688f2d8c-da3d-4ca0-a60d-6d7abd047242
```

## Use the agent to perform on-chain tasks!

Start the agent app.

```
python3 run.py
```

Select the `chat` mode to interact with the agent on the CLI.

```
Starting Based Agent...

Available modes:
1. chat    - Interactive chat mode
2. auto    - Autonomous action mode
3. two-agent - AI-to-agent conversation mode

Choose a mode (enter number or name): 1

Starting chat mode...
Starting Swarm CLI 🐝
```

Below is the conversation I had with the agent.

```
User: I want to know the balance
Based Agent: Sure, what asset's balance would you like to check?
User: ETH balance
Based Agent: <tool_call>
{"id": 0, "name": "get_balance", "arguments": {"asset_id": "eth"}}
</tool_call>
get_balance("asset_id"= "eth")
Based Agent: Your current ETH balance is 0.01 ETH.
User: What about USDC?
Based Agent: <tool_call>
{"id": 0, "name": "get_balance", "arguments": {"asset_id": "usdc"}}
</tool_call>
get_balance("asset_id"= "usdc")
Based Agent: Your current USDC balance is 0.
User: Let's create an ERC20 contract with the name MJY and initial supply of 1000 tokens
Based Agent: <tool_call>
{"id": 0, "name": "create_token", "arguments": {"name": "MJY", "symbol": "MJY", "initial_supply": "1000"}}
</tool_call>
create_token("initial_supply"= "1000", "name"= "MJY", "symbol"= "MJY")
Based Agent: The token MJY has been successfully created with an initial supply of 1000 tokens and its contract address is 0xa24C886a0a9caC9dad5E397C5B821Eb2c32F7045.
User: What's my balance now?
Based Agent: <tool_call>
{"id": 0, "name": "get_balance", "arguments": {"asset_id": "eth"}}
</tool_call>
get_balance("asset_id"= "eth")
Based Agent: Your current ETH balance is 0.009998896893340157 ETH.
User: exit
Based Agent: You're exiting the session. Is there anything else I can assist you with before you go?
User: no
Based Agent: Goodbye, have a great day!
```

Go to `basescan` to see the wallet account. It now has 0.00999 ETH and 1000 MJY tokens.

https://sepolia.basescan.org/address/0xb8ad0a228a4d51fd13e6587c8815b208b509cee2

You can also see the MJY contract address it created.

https://sepolia.basescan.org/address/0xa24c886a0a9cac9dad5e397c5b821eb2c32f7045

That's it. Happy coding!
