# ZWallet Commandline Utility for 0Chain Blockchain
ZWallet Commandline Utility is useful to quickly demonstrate and understand the capabilities of 0Chain Blockchain. The utility is built using 0Chain's ClientSDK library written in Go V1.2
## Features
ZWallet Commandline Utility supports following features:
1. Create Wallets
2. Get test tokens from 0Chain Faucet
3. Send tokens between Wallets
4. Lock and Unlock tokens to earn interest
5. Recover wallet using passphrases

ZWallet Commandline Utility provides a self-explaining "help" option that lists out the commands it supports and the parameters each command needs to perform the intended action
## How to get it?
There are two ways of getting ZWallet.
1. If you would like to demo or get familiarize with 0Chain Blockchain on your Mac, download the executable from [here](https://0chain.net).
2. If you've extended or added a new feature to 0Chain's ClientSDK code, you can extend ZWallet Commandline utility to test/demo the new feauture. [Here](https://github.com/0chain/zwalletcmd) is the link to ZWallet Repository
## Pre-requisites
* ZWallet Commandline Utility application runs on Mac **OS X High Sierra** onwards.
* If you would like to build and play around the code, you need Go V1.2 or higher.
## How to Build the code?
1. Make sure you've Go SDK 1.2 or higher and Go configurations are set and working.
2. Clone [zwalletcmd](https://github.com/0chain/zwalletcmd)
3. Go to the root directory of the local repo
4. Run the following command:

        go build -tags bn256

5. zwalletcmd application is built in the local folder. 
## Getting started with ZWallet
### Before you start
Before you start playing with ZWallet, you need to know where the blockchain is running and what encryption scheme is it is using. Both of that information is stored in a configuration file called nodes.yaml. If you do not have that, please make a request to 0Chain support.
### Setup
ZWallet Commandline Utility needs to know the configuration at runtime. Either you can provide that with flag --config or save it under $Home/.zcn folder. For now let's create $Home/.zcn folder and store the nodes.yaml file there.
### Commands
To run the commands, cd to the folder where zwallet is located.

Let's go over all the available commands and play with it :
#### command with no arguments
When you run zwallet with no arguments, it will list all the supported commands.

Command

    ./zwallet
Response

    zwallet  is to store, send and execute smart contract on 0Chain platform.
    Complete documentation is available at https://0chain.net

    Usage:
    zwallet [command]

    Available Commands:
    faucet          Faucet smart contract
    getbalance      get balance from sharders
    getlockedtokens Get locked tokens
    help            Help about any command
    lock            Lock tokens
    lockconfig      Get lock configuration
    recoverwallet   Recover wallet
    send            Send ZCN token to another wallet
    unlock          Unlock tokens

    Flags:
        --config string   config file (default is $HOME/.zcn/nodes.yaml)
    -h, --help            help for zwallet
        --wallet string   wallet file (default is $HOME/.zcn/wallet.txt)

    Use "zwallet [command] --help" for more information about a command.

### help command
Run help command to get the list of required arguments 
Command

    ./zwallet getbalance --help

Response

    Faucet smart contract.
	        <methodName> <input>

    Usage:
        zwallet faucet [flags]

    Flags:
    -h, --help                help for faucet
        --input string        input
        --methodName string   methodName

    Global Flags:
        --config string   config file (default is $HOME/.zcn/nodes.yaml)
        --wallet string   wallet file (default is $HOME/.zcn/wallet.txt)

#### getbalance
getbalance helps in two ways 

1. to get balance of an existing wallet
2. to create a wallet if there is none

Command

    ./zwallet getbalance
Response

    No wallet in path  $Home/.zcn/wallet.txt found. Creating wallet...
    ZCN wallet created!!

    Get balance failed. 
If you open the wallet.txt file, you will see the wallet details.

    {"client_id":"44347b5640ef3f5313e5efe3c6ab0e0c83efd625ed2bf00e912479aa8813cb1d","client_key":"1e400854be8bc1a787f4528da60984f22aee9e1fa47d3aa3aef27c40e8b087077283596084a9329e82d9f4f1eaf4319415648dd47795c5ed1156c2363dbe1280","keys":[{"public_key":"1e400854be8bc1a787f4528da60984f22aee9e1fa47d3aa3aef27c40e8b087077283596084a9329e82d9f4f1eaf4319415648dd47795c5ed1156c2363dbe1280","private_key":"b4ec0f105417a833d38213f2c246bd5c37a242e251009088e1e8f7204f112f0a"}],"mnemonics":"portion hockey where day drama flame stadium daughter mad salute easily exact wood peanut actual draw ethics dwarf poverty flag ladder hockey quote awesome","version":"1.0","date_created":"2019-06-16 16:22:15.406946 -0700 PDT m=+0.007561539"}
Out of these, the client_id and menmonics we will be using later.

#### faucet
faucet command is useful to get test tokens into your wallet for transactional purposes.

Command

     ./zwallet faucet --methodName pour --input "{Pay day}"

Response

    Execute faucet smart contract success

#### getbalance
Let's use getbalance again to check the balance.
Command

    ./zwallet getbalance

Response

    Balance: 1 

There is 1 token deposited in the wallet as specified in wallet.txt. Same way you can use faucet any number of time whenever you need additional tokens.

#### getbalance
You can use getbalance command to create a named wallet too.

Command

     ./zwallet getbalance --wallet from

Response

    No wallet in path  /Users/jay_at_0chain/.zcn/from found. Creating wallet...
    ZCN wallet created!!

    Get balance failed. 

Check the new wallet file "from" created under your $Home/.zcn

#### send
Use send command to send a transaction from one wallet to the other. Send commands take four parameters. 
1. From wallet -- default is the account in wallet.txt
2. to_client_id -- address of the wallet receiving the funds
3. desc -- description for the transaction
4. tokens -- tokens in decimals to be transferred.

Command

     ./zwallet send --wallet from --desc "testing" --toclientID "7fe5e58d94684e3ec0b7fe076c4bc2aa56c455bfc7a476155c142d42eaf0d416" --token 0.5

Response

    Send token success

When you run a getbalance on both the wallets you see the difference

#### lockconfig
0Chain has a great way of earning additional tokens by locking tokens. When you lock tokens for a period of time, you will earn interest. The terms of lock can be obtained by lockconfig command.
Command

    ./zwallet lockconfig

Response

    Configuration:
    {"ID":"6dba10422e368813802877a85039d3985d96760ed844092319743fb3a76712d9","max_lock_period":31536000000000000,"min_lock_period":60000000000,"simple_global_node":{"interest_rate":0.5,"min_lock":10}}


#### lock
Command

./zwallet lock --wallet from --durationHr 0 --durationMin 5 --token 10.0

Response

    Tokens (10.000000) locked successfully

#### getlockedtokens
Use getlockedtokens command to get informatiion about locked tokens

Command
    ./zwallet getlockedtokens --wallet from

Response

    Locked tokens:
    {"stats":[{"pool_id":"41fd52bbc848553365ae7b1319a3732764ea699964c3c97f1d85fb45fb46572e","start_time":"2019-06-17 05:48:54 +0000 UTC","duration":"5m0s","time_left":"3m57.17069839s","locked":true,"interest_rate":0.000004756468797564688,"interest_earned":475646,"balance":100000000000}]}

#### unlock

Use this command to unlock the locked tokens. Unless you unlock, the tokens are not released.

Command

    ./zwallet unlock --poolid 41fd52bbc848553365ae7b1319a3732764ea699964c3c97f1d85fb45fb46572e

#### recoverWallet

use this command to recover wallet from a different computer. You need to provide mnemonics mentioned in the wallet as an argument to prove that you own the wallet.

Command

    ./zwallet recoverwallet --mnemonic  "portion hockey where day drama flame stadium daughter mad salute easily exact wood peanut actual draw ethics dwarf poverty flag ladder hockey quote awesome"

### Tips
1. Sometimes when a transaction is sent, it may fail with a message "verify transaction failed". In such cases you need to resend the transactions
2.  Use cmdlog.log to check possible reasons for failure of transactions.
3. zwalletcmd also comes with a Makefile which simplifies a lot of these zwallet commands.  