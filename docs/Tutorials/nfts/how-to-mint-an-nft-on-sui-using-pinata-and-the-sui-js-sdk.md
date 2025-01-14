---
title: "How to Mint an NFT on Sui using Pinata and the Sui JS SDK"
slug: "how-to-mint-an-nft-on-sui-using-pinata-and-the-sui-js-sdk"
excerpt: ""
hidden: false
createdAt: "Mon Sep 18 2023 19:15:30 GMT+0000 (Coordinated Universal Time)"
updatedAt: "Wed Nov 29 2023 16:31:58 GMT+0000 (Coordinated Universal Time)"
---
![](https://files.readme.io/7259836-image.png)

One of my favorite pastimes is playing around with new blockchains and seeing what they’re like, what they offer, and how easy they are to build on. Recently I stumbled upon Sui and some of its unique features, such as dynamic metadata and goals to help onboard everyday people. After playing with it myself I was impressed with its speed and smoothness, which is saying something as it’s still in development!

The only thing I found lacking was some basic explanations of how to mint NFTs on Sui and some of the things you might have to tackle, so I thought I would share that experience and demonstrate this!

To get started, you should only need the basics such as NodeJS and a text editor like VSCode, as well as some fundamental knowledge about Javascript.

## Setting up your NFT with Pinata

While Sui features dynamic metadata, you will still want to use a service to store images, video, or any other kind of content you want to turn into an NFT! Pinata is ideal as it uses IPFS which prevents tampering with content which you can read more about here.

Getting started with Pinata is easy! Just visit the signup page here and start out with a free account. Now all you have to do is upload the image you want to use. Do that by visiting the main files page and clicking “Upload” and “Select File.”

[block:html]
{
  "html": "<video muted autoplay style=\"width: 100%; height: auto; position: relative;\">\n    <source src=\"https://mktg.mypinata.cloud/ipfs/Qmer2ReDzussRC2CZCZRK4TPp3jxT191bz4WwfwwsVX7FD?filename=video.mp4\" type=\"video/mp4\">\n</video>"
}
[/block]


After that just follow the steps of selecting your file, give it a name, then upload! Once done it should show up in your files page as seen below, and we will want to copy the CID for later.

## Code Setup with the Sui JS SDK

First let’s make a new directory where the project will live using the following terminal command:

`mkdir sui-nft && cd sui-nft`

Now let’s install the Sui Javascript SDK:

`npm init -y && npm install @mysten/sui.js`

One small change we need to make to the package.json file is adding the module type, so make sure your package.json looks like this

```json
{  
  "name": "sui-nft",  
  "type": "module",  
  "version": "1.0.0",  
  "description": "",  
  "main": "index.js",  
  "scripts": {  
    "test": "echo \"Error: no test specified\" && exit 1"  
  },  
  "keywords": \[],  
  "author": "",  
  "license": "ISC",  
  "dependencies": {  
    "@mysten/sui.js": "^0.26.1"  
  }  
}
```

Lastly, we will want to make a file to run our code which we’ll call mint-nft.js

`touch mint-nft.js`

Now we can get into good stuff — writing the code to mint our NFT!

Minting the NFT  
Go ahead and open up your `mint-nft.js` file in your text editor of choice, and the first thing we’re gonna do is import the following methods at the top of the page.

`import { Ed25519Keypair, JsonRpcProvider, Network, RawSigner } from '@mysten/sui.js';`

All we really need are four things, and you’ll see how they play a part as we start building. Next thing we need to do is create a wallet and get the public address for that wallet, which we can do like so.

```javascript
import { Ed25519Keypair, JsonRpcProvider, Network, RawSigner } from '@mysten/sui.js';

const keypair = new Ed25519Keypair()  
const address = "0x" + keypair.getPublicKey().toSuiAddress().toString()  
console.log(address)
```

Accessing the keypair object makes it super simple to either get the public key or private key, and then turn it into usable data. The Sui address from the public key doesn’t include the leading “0x” so I’m adding that manually here. If you go into the terminal now and run `node mint-nft.js` then you should see an address like this!

`0x8f7671eedff42d6dde8c365a6b641bb9769ea02e`

Now that we have a wallet address, it’s time to connect to the Sui network and get some test Sui coin! First we’ll declare a new provider and use the JsonRpcProvider, and pass in the Network “DEVNET.”

```javascript
import { Ed25519Keypair, JsonRpcProvider, Network, RawSigner } from '@mysten/sui.js';

//Create keypair  
const keypair = new Ed25519Keypair()  
const address = "0x" + keypair.getPublicKey().toSuiAddress().toString()  
console.log(address)

//Create network connection  
const provider = new JsonRpcProvider(Network.DEVNET);
```

Then we’ll use the provider to request some test Sui from the Devnet faucet and use our new address as the receiver like so.

```javascript
import { Ed25519Keypair, JsonRpcProvider, Network, RawSigner } from '@mysten/sui.js';

//Create keypair  
const keypair = new Ed25519Keypair()  
const address = "0x" + keypair.getPublicKey().toSuiAddress().toString()  
console.log(address)

//Create network connection  
const provider = new JsonRpcProvider(Network.DEVNET);

// Get Sui from faucet  
const fund = await provider.requestSuiFromFaucet(address)  
console.log(fund)
```

Let’s run the `node mint-nft.js` command now and see what we get!

```text
0xf64640227ff94ba762252c15f2adbcedb6d3aaab  
{  
  transferred_gas_objects: [  
    {  
      amount: 10000000,  
      id: '0x39c25c3885c2cccea957c26219de9c7e58a33a21',  
      transfer_tx_digest: '4ETS2rGNzRYZ95SsLrUsQf8ckfZWQSSqTEpGi32RqKbk'  
    },  
    {  
      amount: 10000000,  
      id: '0x3bdad5c729495d9d152cfd03b0e44e8549972d53',  
      transfer_tx_digest: '4ETS2rGNzRYZ95SsLrUsQf8ckfZWQSSqTEpGi32RqKbk'  
    },  
    {  
      amount: 10000000,  
      id: '0x4a748f4e928b974dd46913e2cc069a21fecaad86',  
      transfer_tx_digest: '4ETS2rGNzRYZ95SsLrUsQf8ckfZWQSSqTEpGi32RqKbk'  
    },  
    {  
      amount: 10000000,  
      id: '0x66d601ef1811cbdea82d2eb97a0994afdbbc888a',  
      transfer_tx_digest: '4ETS2rGNzRYZ95SsLrUsQf8ckfZWQSSqTEpGi32RqKbk'  
    },  
    {  
      amount: 10000000,  
      id: '0xee8668e7c2fcd60047992f170da075dafd955f48',  
      transfer_tx_digest: '4ETS2rGNzRYZ95SsLrUsQf8ckfZWQSSqTEpGi32RqKbk'  
    }  
  ],  
  error: null  
}
```

Nice! Through this we can see the Sui we received which equals out to 0.05 Sui. Something you might be wondering is why we have 5 different items here with different ids, and that’s actually a very important piece of the Sui model that isn’t natural. Every coin in Sui has its own unique object id. These are all of the same type of currency which is the default Sui type, but our five pieces are in their own camp. This can cause a problem when we try to use our Sui to mint an NFT because its gonna try to pull from just one of these objects when ideally we want to pull from all of them like a normal blockchain (huge shoutout to Paul Fidika for helping me understand this bit!).

Think of it like this. Let’s say you have a farm with five cow fields, and each field is separated by the type of cow: short haired, long haired, etc. Each field has 10 cows so you technically have 50 cows, but in order to trade for some goats you need 20 cows. You would have to empty two of those cow fields and combine them to trade them. That’s essentially what we’re doing in Sui with coins.

With that said, in order for us to mint, we need to combine some of our recently acquired Sui drop. We’re gonna do that with the following code.

```javascript
// Merge two of the Sui coin objects  
const coin1 = fund.transferred_gas_objects[0].id  
const coin2 = fund.transferred_gas_objects[1].id  
const signer = new RawSigner(keypair, provider);  
const mergeTxn = await signer.mergeCoin({  
  primaryCoin: coin1,  
  coinToMerge: coin2,  
  gasBudget: 1000,  
});  
console.log('MergeCoin txn', mergeTxn);
```

There’s quite a bit going on here so let’s break it down.

First we declare coin1 and coin2 from the airdrop we just received by accessing those object ids from our fund result. Then we need to declare our signer! This is what lets us use our private key from our keypair to transfer or mint on the chain, and we do that by declaring a new RawSigner and passing in our previously made keypair and provider to connect. Finally we use the mergeCoin method and pass in our two coins, along with a gas budget.

Before we run this, there is one small thing we’re missing. If you try to run this code now, you will likely get an error that it could not find the object id for the coin. That’s because its was just freshly created and we’re trying to access an object id that isn’t quite discoverable yet. To fix that we’re gonna make a small little helper function called “wait” like so.

```javascript
// Pause function  
const wait = async (time) => {  
  return new Promise((resolve, reject) => {  
    setTimeout(() => {  
      resolve();  
    }, time)  
  });  
}
```

This is really simple and just lets us pass in how many milliseconds we want to wait before continuing our function! Let’s use it after getting our airdrop and passing in 3 seconds. You code should look something like this now.

```javascript
import { Ed25519Keypair, JsonRpcProvider, Network, RawSigner } from '@mysten/sui.js';

// Generate a new Keypair  
const keypair = new Ed25519Keypair();  
const address = "0x" + keypair.getPublicKey().toSuiAddress().toString()  
console.log(address)

// Create Network Connection and receive airdrop  
const provider = new JsonRpcProvider(Network.DEVNET);

// Get Sui from faucet  
const fund = await provider.requestSuiFromFaucet(address)  
console.log(fund)

// Pause function  
const wait = async (time) => {  
  return new Promise((resolve, reject) => {  
    setTimeout(() => {  
      resolve();  
    }, time)  
  });  
}

await wait(3000)

// Merge two of the Sui coin objects  
const coin1 = fund.transferred_gas_objects[1].id  
const coin2 = fund.transferred_gas_objects[2].id  
const signer = new RawSigner(keypair, provider);  
const mergeTxn = await signer.mergeCoin({  
  primaryCoin: coin1,  
  coinToMerge: coin2,  
  gasBudget: 1000,  
});  
console.log('MergeCoin txn', mergeTxn);
```

If you run this code you should see the result of the airdrop, a pause before merging the coins, and the successful coin merge! Now that we have all our cows in one heard, we can mint an NFT successfully. Let’s take a look!

```javascript
// Call to Mint NFT  
const mintTxn = await signer.executeMoveCall({  
  packageObjectId: '0x2',  
  module: 'devnet_nft',  
  function: 'mint',  
  typeArguments: \[],  
  arguments: [  
    'gm',  
    'A nice gm brought to you by Pinata and Sui',  
    'ipfs://QmZhnkimthxvL32vin2mrQvnhN8ZbWFMvKMxRqHEq7dPz3',  
  ],  
  gasBudget: 10000  
});  
console.log('mint transaction:', mintTxn);
```

Our minting function is simply accessing a pre-built smart contract called `devnet_nft` and we’re using the `mint` function. All we have to pass into the arguments is the name of the NFT, the description, and then the asset link!

Before we mint, I’m gonna do one last console log that will let us take a look at the NFT in the browser. To do that , we need to access the results of the NFT mint and get the object id of the NFT, and then just pass that into a Sui explorer link like so.

```javascript
// View NFT  
const nftId = mintTxn.effects.effects.created[0].reference.objectId.toString()  
console.log(`View NFT: https://explorer.sui.io/object/${nftId}?network=devnet`)  
Now let’s look at our full code to make sure everything is good, then run it!

import { Ed25519Keypair, JsonRpcProvider, Network, RawSigner } from '@mysten/sui.js';

// Generate a new Keypair  
const keypair = new Ed25519Keypair();  
const address = "0x" + keypair.getPublicKey().toSuiAddress().toString()  
console.log(address)

// Create Network Connection and receive airdrop  
const provider = new JsonRpcProvider(Network.DEVNET);

// Get Sui from faucet  
const fund = await provider.requestSuiFromFaucet(address)  
console.log(fund)

// Pause function  
const wait = async (time) => {  
  return new Promise((resolve, reject) => {  
    setTimeout(() => {  
      resolve();  
    }, time)  
  });  
}

await wait(3000)

// Merge two of the Sui coin objects  
const coin1 = fund.transferred_gas_objects[1].id  
const coin2 = fund.transferred_gas_objects[2].id  
const signer = new RawSigner(keypair, provider);  
const mergeTxn = await signer.mergeCoin({  
  primaryCoin: coin1,  
  coinToMerge: coin2,  
  gasBudget: 1000,  
});  
console.log('MergeCoin txn', mergeTxn);

// Call to Mint NFT  
const mintTxn = await signer.executeMoveCall({  
  packageObjectId: '0x2',  
  module: 'devnet_nft',  
  function: 'mint',  
  typeArguments: \[],  
  arguments: [  
    'gm',  
    'A nice gm brought to you by Pinata and Sui',  
    'ipfs://QmZhnkimthxvL32vin2mrQvnhN8ZbWFMvKMxRqHEq7dPz3',  
  ],  
  gasBudget: 10000  
});  
console.log('mint transaction:', mintTxn);

// View NFT  
const nftId = mintTxn.effects.effects.created[0].reference.objectId.toString()  
console.log(`View NFT: https://explorer.sui.io/object/${nftId}?network=devnet`)
```

If all works as it should you’ll get a link and then you should see your final NFT!

You did it!! 🎉  
You successfully minted an NFT on Sui!! Of course this is only the beginning of what’s possible. Maybe a nice next step is transferring the NFT to another wallet, or maybe even trying to build a Sui marketplace 👀

With whatever you’re trying to do, Pinata is here to help with tools like our API, Dedicated Gateways to quickly stream images or video content for a marketplace, or token gated solutions using Submarine!

If you have any questions or want to learn more, feel free to join our [Discord](https://discord.gg/pinata) and say hi! :)
