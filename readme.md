To extract token pair prices from different Decentralized Exchanges (DEXs) such as Uniswap, SushiSwap, PancakeSwap, etc., you'll typically want to interact with the smart contracts of these platforms via their Application Programming Interface (API) or through direct blockchain queries. One common method is to use Web3.py, a Python library for interacting with Ethereum-compatible blockchains.

Here is a step-by-step guide on how you might go about doing this:

### 1. Install Required Packages

You'll need Web3.py and requests, which you can install using `pip`:

```bash
pip install web3 requests
```

### 2. Set Up Your Web3 Provider

To interact with the blockchain, you need access to a node. You can set up your own, but the most straightforward way is to use a service like Infura or Alchemy that provides an API endpoint.

```python
from web3 import Web3

# Replace this with your own node provider URL
provider_url = 'https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID'
web3 = Web3(Web3.HTTPProvider(provider_url))
print(web3.isConnected())  # Should print True if connected successfully
```

### 3. Get ABI for DEX Contracts

The ABI (Application Binary Interface) is essential for interacting with the smart contracts. You can find ABIs for popular contracts on resources like Etherscan or through the DEX's developer documents.

### 4. Create Contract Instances

Using the contract ABIs and their addresses, you can create instances of the contracts.

```python
# Sample for Uniswap V2; replace with real ABI and address
uniswap_v2_abi = '[{"constant":true,"inputs":[...],"name":"...","outputs":[...]}]'
uniswap_contract_address = '0xUniswapV2ContractAddress'

uniswap_v2_contract = web3.eth.contract(address=uniswap_contract_address, abi=uniswap_v2_abi)
```

### 5. Query The Contract For Token Prices

Different DEXs have different methods of getting the price. For Uniswap, you would typically get the reserves for a pair and calculate the price from that. Each DEX will have its own approach.

```python
# Example function call to get reserves for a Uniswap pair, not complete nor functional
reserves = uniswap_v2_contract.functions.getReserves().call()
reserve0 = reserves[0]
reserve1 = reserves[1]

# Calculate price based on the reserves
# This assumes token0 and token1 are in equal decimals, which may not always be the case.
price_of_token0_in_terms_of_token1 = reserve1 / reserve0
```

### 6. Repeat for Other DEXs

Repeat steps 3-5 for each DEX you're interested in. Remember, each one may require slightly different handling depending on how they've implemented their smart contracts.

### 7. Error Handling

Make sure to add error handling, especially considering that interactions with the blockchain can fail due to network issues, or the data may not be in the expected format.

```python
try:
    # Your code to fetch and compute prices here
except Exception as e:
    print(f"An error occurred: {e}")
```

This is a high-level overview. Each DEX has specific details, so you'll need to read the documentation for the particular DEXes you're interested in to handle the nuances correctly.

**Note**: When you're dealing with DeFi projects, remember to treat them with caution. Interacting with smart contracts carries risks, and you should never expose private keys or sensitive information in your scripts. Always test your code using testnets before executing any calls on the mainnet.