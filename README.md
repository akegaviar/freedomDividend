# Universal Basic Income Opt-In on Hyperledger Fabric

[Andrew Yang](https://en.wikipedia.org/wiki/Andrew_Yang), a 2020 presidential candidate in the U.S., is arguably the only technologist to ever join the race.

Yang predicts that there will be massive worker displacement due to technological automation in the next five years. His solution to the worker displacement problem and the core proposal of his presidential campaign is enrolling every U.S. citizen in the Universal Basic Income program, which he calls the [Freedom Dividend](https://en.wikipedia.org/wiki/Andrew_Yang_2020_presidential_campaign#Freedom_Dividend_(UBI)).

Since there is money distribution involved and Andrew Yang's entire career being in technology, it makes for a fun thought experiment to imagine what the tech infrastructure could look like.

The the very basic framework proposed here is a [chaincode](https://hyperledger-fabric.readthedocs.io/en/release-2.0/chaincode.html#what-is-chaincode) on Hyperledger Fabric.

Each of the 50 states can run [organizations and peers](https://hyperledger-fabric.readthedocs.io/en/release-2.0/peers/peers.html#peers-and-organizations) that join a [channel](https://hyperledger-fabric.readthedocs.io/en/release-2.0/channels.html).

Each of the organizations and peers can have an app that lets the U.S. citizens submit their opt-in agreement to the Freedom Dividend based on their Social Security number as their identifier. Once opted in, Social Security number holders can also opt out of the Freedom Dividend program and have their Social Security number removed from the world state.

Those that have opted in, can be queried from the world state to have their monthly distribution of the Universal Basic Income.

## Prerequisites

This tutorial uses:

* [Visual Studio Code] with the [IBM Blockchain Platform extension](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform).
* The chaincode is done in JavaScript. You should have Node.js and npm installed.
* [Docker](https://www.docker.com/get-started) to support the local Hyperledger Fabric network initiated via the IBM Blockchain Platform extension.
* [fabric-shim](https://www.npmjs.com/package/fabric-shim) to package the chaincode via the IBM Blockchain Platform extension.

Note that for this tutorial, I am using Visual Studio Code version `1.38.1` as later versions [have issues](https://github.com/IBM-Blockchain/blockchain-vscode-extension/issues/1621) with the IBM Blockchain Platform extension.

## The chaincode, aka freedomDividendContract

### Create the chaincode

1. Start your Visual Studio Code the IBM Blockchain Platform extension installed.
1. On the left pane, click the square IBM Blockchain Platform icon.
1. Click **Smart Contracts** > **Create New Project**.
1. Select **Default Contract**. Select **JavaScript**. Type in a name for your chaincode. For this tutorial, it's `freedomDividendContract`.
1. Choose a directory to save your project to. In the directory, all the necessary chaincode files will be generated.
1. Your main file is the contract in the `./lib/` directory. For this tutorial, it's [freedomDividendContract.js]()TTK. Do check out the code, as it's commented.
1. Make sure your `index.js` file has the correct parameters relative to the contract. See [index.js]()TTK
1. Make sure you have the correct name and chaincode version in `package.json`. See [package.json]()TTk

### Package the chaincode

Make sure you have [fabric-shim](https://www.npmjs.com/package/fabric-shim) installed in your chaincode project directory:

`npm in fabric-shim`

Click **Smart Contracts** > **Package Open Project**.

### Start the local network

Under **Fabric Environments**, click **Local Fabric**.

Starting the network will take a few minutes.

Once the network is started, connect to the network:

1. Under **Fabric Gateways**, click **Local Fabric**. Select **admin**.

### Install and instantiate the chaincode

Install:

1. Under **Fabric Environments**, click **Smart Contracts** > **Installed** > **Install**.
1. Select your packaged chaincode.

Instantiate:

1. Click **Instantiated** > **Instantiate**.
1. Select your installed chaincode.
1. Leave the optional blank and hit Enter.
1. Click `No` for the private data collection file.
1. Select the default endorsement policy.

Instantiating the chaincode will take a few minutes.

### Interact with the chaincode

Under **Fabric Gateways**, click **Channels** > **mychannel** > **freedomDividendContract**.

This will expand the three options to interact with your chaincode:

* **optIn** — the arguments are `ssnId`, `optedIn`. For example, `"123-45-6789", "opt in"`. This will submit the transaction with the provided Social Security number and update the world state.
* **optOut** - the argument is `ssnId`. For example, `"123-45-6789"`. If you have previously opted in with a Social Security number, this will submit the transaction and remove the Social Security number from the world state, effectively opting this number out.
* **querySSN** — the argument is `ssnId`. If you have previously opted in with a Social Security number, this will query the world state and return a message that this number is opted in.

To interact with each of the options, right-click on it and select `Submit Transaction`.