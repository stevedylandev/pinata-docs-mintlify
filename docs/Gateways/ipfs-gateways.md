---
title: "IPFS Gateways"
slug: "ipfs-gateways"
excerpt: ""
hidden: false
metadata: 
  robots: "index"
createdAt: "Tue Jul 18 2023 11:22:06 GMT+0000 (Coordinated Universal Time)"
updatedAt: "Tue Sep 12 2023 16:56:56 GMT+0000 (Coordinated Universal Time)"
---
Once you have uploaded content to Pinata, naturally the first thing you want to do is view it on the IPFS network! But there's a problem: IPFS is a separate protocol, just like HTTP for regular websites is a protocol. To access that content, we need a gateway to bridge IPFS and HTTP. IPFS gateways help us do exactly that! 

An example of accessing content from this gateway can be seen with: `https://gateway.pinata.cloud/ipfs/bafkreih5aznjvttude6c3wbvqeebb6rlx5wkbzyppv7garjiubll2ceym4`

What's going on here exactly? Let's break it down. First you have the gateway domain. There are lots of different domains out there, like `ipfs.io`, `gateway.pinata.cloud`, and if you have a Pinata account you get a Dedicated Gateway which might look something like `aquamarine-casual-tarantula-177.mypinata.cloud`. Each of these can be used to bridge files from IPFS to HTTPs. Next you have the IPFS path which looks like `ipfs/` and this is necessary for a gateway to work. Finally we have the CID at the end, which is the actual IPFS address for our content. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a32c07b-gateway.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


## Folder Paths

Sometimes your CID might be a folder, in which case you might have difficulty loading it through the gateway. This happens because if you do not designate a complete folder path, then the gateway will try to load all of the files and index them into a sheet showing every file. This can be pretty intensive depending what kind of gateway you're using, and will likely stall out due to how long it can take. To remedy this, simply add on the file path of the content you're trying to get inside. For instance, if we have a folder with the CID of `QmWfHgs3nKiyFWx3tFEYvm8DiHTrCsxEHxvDdBh95ZQSLT` and the inside looks something like this:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/384ed79-folder-cid.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


Then we could access the files inside by adding on `/pinnie.png` or `clouds.json` to the end of our folder path. In the end we would have something like this. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c49c977-folder-path.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


## Public Gateways

The most common kind of IPFS Gateways are Public Gateways. These are usually run and maintained by IPFS Pinning Services, protocols, or even smaller groups that want to help build the IPFS ecosystem. They're referred to as "Public" because anyone can access them! You just have to add a CID to the end of one to start using it. You can try that now by adding this CID `bafkreih5aznjvttude6c3wbvqeebb6rlx5wkbzyppv7garjiubll2ceym4` to the end of Pinata's Public Gateway:

```text
https://gateway.pinata.cloud/ipfs/
```

There's something you might notice when you start using Public Gateways frequently, and that is the speed and rate limits. Since these are Public, they're used **heavily** by people all around the world. They are constantly getting hammered, and because of that they're just naturally slower. You can think of it like a highway or interstate: if you have a high flow of traffic, its likely going to bottleneck and cause a traffic jam. Same goes for IPFS Gateways! 

> 🚧 Public Gateways are not meant for production apps, be sure to only use them for testing!

## [Dedicated Gateways](doc:how-to-i-use-my-gateway)

Thankfully Pinata has you covered! When you sign up for a free Pinata account, you get your own Dedicated Gateway. Dedicated Gateways are like the toll roads on highways and interstates; it's your own private boulevard to get unmatched speeds. Our Dedicated Gateways are well known in the industry as being fast, reliable, and just plain simple to use, and they do that through a large network of IPFS nodes and a built in global CDN that helps cache content to be much faster on subsequent loads. Check on the next few pages to learn more about how they work!
