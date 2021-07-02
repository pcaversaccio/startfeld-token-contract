# Startfeld's ERC-20 Smart Contract
This is Startfelds's [ERC-20](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20) smart contract, whose tokens are used as vouchers in their ecosystem.

## Changelog
See the created [`CHANGELOG`](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/blob/main/CHANGELOG.md) file in this repository.

## Installation
### 1. Install the Truffle Framework
We use the Truffle framework for the compilation, testing, and deployment. Please follow their [guide](https://www.trufflesuite.com/truffle) to install the framework on your computer.

### 2. Getting Started
Run `npm i` in order to install the necessary [OpenZeppelin node modules](https://www.npmjs.com/package/@openzeppelin/contracts) as well as further required dependencies.

## Compilation
To compile the contract, it is important that you have installed the project correctly, as we use external dependencies and contracts. Use the following command to compile the contracts: 
```
truffle compile
```

## Unit Tests
Since we build the [ERC-20](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20) smart contract on top of the audited [OpenZeppelin node modules](https://www.npmjs.com/package/@openzeppelin/contracts), there is no further requirement to write dedicated tests for these modules. Nonetheless, due to the fact that we integrate the non-standard [`permit`](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Permit-permit-address-address-uint256-uint256-uint8-bytes32-bytes32-) method, unit tests have been written for this specific extension.

You can run the tests with 
```
npx hardhat test
```

## Deployment
### Local Deployment With Ganache
To deploy the contract on your local Ganache blockchain, you must first install the software on your computer. Follow the installation [guide](https://www.trufflesuite.com/ganache).

Once you installed the local blockchain, you can create a workspce. This is described [here](https://www.trufflesuite.com/docs/ganache/workspaces/creating-workspaces).
> **Note:** We have observed that Truffle and Ganache do not use the same default RPC configuration. The easiest way to align is to adjust Ganache's server hostname, port, and network ID with Truffle's configurations (check the file [`truffle-config.js`](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/blob/main/truffle-config.js)).

Once you are setup, just run: 
```
truffle migrate --network development
```

### Rinkeby
To deploy the smart contract to [Rinkeby](https://rinkeby.etherscan.io), you need to preconfigure first some things:
1. Create a `secrets.json` file.
2. Create a [MetaMask Wallet](https://metamask.io) and paste the respective seedphrase into `secrets.json`. Make sure you got some ETH. You can get some [here](https://faucet.rinkeby.io).
3. Create a new [Infura project](https://infura.io) and copy the project key into `secrets.json`.
4. Create a [Etherscan](https://etherscan.io) account and copy the API key to `secrets.json`.
The file will look like the following (make sure to always [`.gitignore`](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/blob/main/.gitignore) it!):
```
{
    "seedPhrase": "drip voice crush ...",
    "projectId": "a657e3934de84d64b27a256079a3b8a2",
    "etherscanKey": "RQFAFV4DE1H75PT5KXVUFFYBWS82AXI59E"
}
```

Now run the following command:
```
truffle migrate --network rinkeby
```

If the deployment was successful, you will get the final deployment result:

![Deployment Result](/assets/RinkebyDeploymentResult.png)

Copy the contract address and verify the contract right away so that you can interact with it. Run the following command:
```
truffle run verify SaentisGulden@CONTRACT-ADDRESS --network rinkeby
```

If the verification was successful, you will see a similar result as follows:
```bash
Verifying SantisGulden@0x10c35227901F2D1D19067E7c5798CF745e360dBc
Pass - Verified: https://rinkeby.etherscan.io/address/0x10c35227901F2D1D19067E7c5798CF745e360dBc#contracts
Successfully verified 1 contract(s).
```

For more information, see [here](https://github.com/rkalis/truffle-plugin-verify).
> **Note 1:** The smart contract [`SaentisGulden.sol`](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/blob/main/contracts/SaentisGulden.sol) does include a `premint` functionality in the constructor that creates an initial amount of 100'000 tokens for the deployer (no need anymore to separately call `mint` after the deployment).

> **Note 2:** The smart contract [`SaentisGulden.sol`](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/blob/main/contracts/SaentisGulden.sol) does include the [`permit`](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Permit-permit-address-address-uint256-uint256-uint8-bytes32-bytes32-) method, which can be used to change an account's ERC20 allowance (see [`IERC20.allowance`](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#IERC20-allowance-address-address-)) by presenting a message signed by the account. By not relying on [`IERC20.approve`](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#IERC20-approve-address-uint256-), the token holder account doesn't need to send a transaction, and thus is not required to hold Ether at all.

## Interaction
If you deployed the smart contract succefully, you are now able to interact with it.

### Local Interaction With the Truffle CLI
To start the Truffle JavaScript console, please run:
```
truffle develop
```

In the console, you can create an instance of the provided contract by typing:
```javascript
let i = await SantisGulden.deployed()
```

You can use the instance variable to call functions like symbol:
```javascript
i.symbol()
```

### Rinkeby
Go to the corresponding Etherscan link, e.g. https://rinkeby.etherscan.io/address/CONTRACT-ADDRESS#code. You are able to invoke READ and WRITE functions on the contract.

## Test Deployments
The smart contract [`SaentisGulden.sol`](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/blob/main/contracts/SaentisGulden.sol) has been deployed across all the major test networks:
### Version 1 (Without `permit` Functionality)
- **Rinkeby:** TBD
- **Ropsten:** TBD
- **Kovan:** TBD
- **Goerli:** TBD

### Version 2 (With `permit` Functionality)
- **Rinkeby:** [0xF89BDCC76C4757C8C036bdC49cF4acE6977D6e41](https://rinkeby.etherscan.io/address/0xF89BDCC76C4757C8C036bdC49cF4acE6977D6e41)
- **Ropsten:** TBD
- **Kovan:** TBD
- **Goerli:** TBD

## Production Deployments on SwissDLT
The smart contract [`SaentisGulden.sol`](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/blob/main/contracts/SaentisGulden.sol) has been deployed to the SwissDLT network with [Remix<sup>*</sup> ](http://remix.ethereum.org) and signed with the Säntis Gulden hardware wallet (Ledger Nano S):
### Version 1 (Without `permit` Functionality)
- [SwissDLT Block Explorer](https://swissdlt.appswithlove.net)
- Contract creation transaction hash: [0x857f538e8476cebc7d5b22863c30494e713bf05cb09aac005d69fdff79e606cd](https://swissdlt.appswithlove.net/tx/0x857f538e8476cebc7d5b22863c30494e713bf05cb09aac005d69fdff79e606cd)
- **Contract address:** [0x263DC7587aBE19595f7d7db378EE7aC2A773CF69](https://swissdlt.appswithlove.net/address/0x263DC7587aBE19595f7d7db378EE7aC2A773CF69)
- **Contract admin:** [0x7de729bc151084C3F455Dbc03E7842919565D23e](https://swissdlt.appswithlove.net/address/0x7de729bc151084C3F455Dbc03E7842919565D23e)
- Contract Application Binary Interface (ABI): Can be downloaded from the [snippet](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/snippets/12). This file was copied from Remix after compilation.

### Version 2 (With `permit` Functionality)
- [SwissDLT Block Explorer](https://swissdlt.appswithlove.net)
- Contract creation transaction hash: TBD
- **Contract address:** TBD
- **Contract admin:** TBD
- Contract Application Binary Interface (ABI): Can be downloaded from the [snippet](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/snippets/14). This file was copied from Remix after compilation.
> **Note 1:** Make sure that you always copy the full smart contract ABI and not just one of the inherited interfaces!

> **Note 2:** Remix uses checksummed addresses for the `At Address` button and if it's invalid the button is disabled. Always use checksummed addresses with Remix! One way to handle this is by using [EthSum](https://ethsum.netlify.app). The checksum algorithm is laid out in full detail [here](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-55.md). 

<sup>*</sup> Remix deployment configuration:
- **Version 1 (Without `permit` Functionality)**
  - Compiler: `0.8.0+commit.c7dfd78e`;
  - Language: `Solidity`;
  - EVM Version: `compiler default`;
  - Enable optimization: `200`;
  - Only the smart contract [`SaentisGulden.sol`](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/blob/main/contracts/SaentisGulden.sol) was used for compilation and deployment. Remix imported the dependencies successfully (see [here](https://remix-ide.readthedocs.io/en/latest/import.html) how this works in the background with the `.deps` folder);
- **Version 2 (With `permit` Functionality)**
  - Compiler: `0.8.6+commit.11564f7e`;
  - Language: `Solidity`;
  - EVM Version: `compiler default`;
  - Enable optimization: `200`;
  - Only the smart contract [`SaentisGulden.sol`](https://gitlab.appswithlove.net/saentis-gulden/saentis-gulden-token-contract/-/blob/main/contracts/SaentisGulden.sol) was used for compilation and deployment. Remix imported the dependencies successfully (see [here](https://remix-ide.readthedocs.io/en/latest/import.html) how this works in the background with the `.deps` folder);

## Further References
[1] https://docs.openzeppelin.com/contracts/4.x/erc20

[2] https://github.com/rkalis/truffle-plugin-verify

[3] https://www.trufflesuite.com/ganache

[4] https://www.trufflesuite.com/truffle
