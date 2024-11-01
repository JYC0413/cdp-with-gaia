# Based Agent with gaia

## Overview

This project leverages a modified version of `gaia-swarm` forked from `openai/swarm` to handle multiple agents.
Helps LLM agents directly interact with the blockchain.

## Installation

To get started, you'll need to install the required packages. Run the following commands:

```bash
pip install openai
pip install python-dotenv
```

Configuration
You will need to set up your environment variables. Create a .env file in the root directory of your project and add the following configuration:
```dotenv
CDP_API_KEY_NAME=your_coinbase_key_name
CDP_PRIVATE_KEY=your_coinbase_private_key
OPENAI_ENDPOINT=your_toolcall_model_api
GAIA_CHAT_ENDPOINT=your_chat_model_api
SWARM_DEFAULT_MODEL=your_toolcall_model_name_for_Autonomous
GAIA_CHAT_MODEL=your_chat_model_name_for_two_agent_mode
OPENAI_API_KEY=your_openai_api_key
```

Replace the placeholders with your actual keys and endpoints.

## Running the Project
Once the environment variables are set, you can run the project by executing:

```bash
python run.py
```