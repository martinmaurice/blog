# Setup Hardhat locally

## Prerequisites
- npm version >= 7
- visual studio code with `hardhat extension`

## Installation
- create empty folder and cd in
- run `npm init`
- run `npm install --save-dev hardhat`
- run `npx hardhat` (select typescript and keep default setup)

## run
- open project using vscode
- open vscode terminal and enter `npx hardhat compile` (typechain-types folder is created)
- test your code running `npx hardhat test`
- `npx hardhat run script/deploy.ts` to r run it

## Connecting a metamask wallet
### install metamask
- go to [metamask](https://metamask.io/)
- click on download
- follow instruction to download and install chrome extension
- if your are new create a new account
- enter a password
- save your pass phrases and that it

### configure metamask local test network
- Click on the icon at top right
- select settings
- hit advanced params
- scroll down and enable `show test networks` and `customization of transaction nonce`

### hardhat network
- run `npx hardhat node`
- copy private key associated to account #0

### connect metamask to hardhat network
- go to settings
- select network params
- select localhost 8545
- fill field `New rpc url` with hardhat local network server url
- save
- click on icon top right
- hit import account
- paste #0 private key in dedicated field
- make sure to select the local network in the select list at the left of user icon
- you now have 10000 ETH fake money
