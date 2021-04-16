# dSocial Design
The design surrounding dSocial's distributed social network is not only groundbreaking but will also change how we look at social networks moving forward. dSocial's design involves the use of multiple blockchains, multiple cryptocurrencies, an identity management system, a multi-currency wallet and an off-chain data mesh that is revolutionizing distributed applications. The architectural design of dSocial is different from any other social network ever created, putting users in control of their data and bringing to life an application that has zero infrastructure costs while giving the community collective control over the network's content. This insures that dSocial can be monetized like no other app on the web, while also allowing the community to maintain their content moderation powers in order to prevent the distribution of illegal content, illegal activities or violence. Before we dive into the technical details, I felt it was important to do a quick introduction, so that the new concepts used within dSocial could be explained.

## INTRODUCTION
A traditional social network built completely on a blockchain would typically store all of its user's data on-chain, which in-turn would prevent the network from being globally scalable, since it would face the inherent bottlenecks brought on by blockchain technology. While dSocial still utilizes a blockchain, it doesn't use a blockchain for storing user data, rather, it uses a blockchain to create a "social data mesh" of off-chain, distributed databases and file systems that are entirely controlled by the users who create them. In other words, the blockchain is used to store the dWeb network addresses of each user's off-chain database, along with a user's following, which allows each user to create a custom social mesh based on their followers.

**You could organize the data found on dSocial into five categories:**

- Action-related data (posts, comments, interactions, etc.)
- Post media (videos, photos, audio, etc.)
- Off-chain data addressing (the address of distributed databases)
- Following/follower data (a user's following/follower list)
- Payment data (payments related to upvotes)

=== **ACTION-RELATED DATA**
Action-related data is stored within a distributed database known as a [dTree](https://npmjs.com/package/dtree).

=== **POST MEDIA**
Post media is stored within a distributed file system known as a [dDrive](https://npmjs.com/package/ddrive), which is referenced alongside the post data its related to, within a user's dTree.

=== **OFF-CHAIN DATA ADDRESSING**
The address of a user's dTree is stored on the ARISEN blockchain alongside the user's ARISEN account (used as a dSocial account). Only the ARISEN user can store this data on the ARISEN blockchain.

=== **FOLLOWING/FOLLOWER DATA**
A user's following and follower data (the users they are following and the users who follow them) is stored on the ARISEN blockchain, alongside the user's ARISEN account. A list of a user's following can be compiled by simply searching ARISEN using the "by_follower" index (searching the on-chain follower table for a specific user, where the users who are returned, are the users whom are following that specific user).

===  **PAYMENT DATA**
dSocial is integrated with several blockchains including Bitcoin, Ethereum, TRON and ARISEN, which allow for users to upvote another user's post(s), using various cryptocurrencies (BTC, ETH, TRON and RIX). Upvote related data is stored on the ARISEN blockchain. This way, ARISEN can be searched for payments associated with a specific user and that user's specific posts. Since financial transactions require heavy consensus, it was best to leave this data on-chain, rather than in a dTree. ARISEN can easily be traversed for a user and the upvotes associated with a postID. Post ID's are unique to a user (64-character hexadecimal keys) and the user's dTree key is also stored on-chain, which means the ARISEN smart contract on dSocial can be used to verify that an actual postID exists within a user's individual dTree. 

### THE PATHWAY TO DISTRIBUTED MACHINES
Its minimal use of blockchains and the distributed nature of its underlying data ultimately allows dSocial to globally scale, without having to face the many bottlenecks that blockchains introduce to applications like a social network. This also allows us to move the minimal amount of data that dSocial stores on ARISEN, to a dWeb-based distributed machine (dVM) in the very near future, if we choose to do so, without having to worry about migrating anything outside of dTree keys and a user's following/follower data. This sort of migration can easily be handled by the users themselves using a simple migration tool that can be built into dSocial's application. There should be no issues with data migration, considering the fact that time stamping isn't involved. It's important to note that the way we're currently using ARISEN is really the perfect use-case for a blockchain (data linking, financial transactions, etc.) and we could technically use ARISEN for the foreseeable future without any issues. Although, if we do somehow face issues, we at least have a plan and the option of migrating to a more scalable solution moving forward, namely dVM, that will not affect the uptime or security of the network, nor will it introduce any sort of centralization into the mix. In fact, it would further decentralize the network.  

### DISTRIBUTED DATABASES
Every single user on dSocial stores their data (i.e. posts, comments, interactions, etc.) within a dTree. The dTree can only be written to by its creator and is only used to store the creator's data.  Since a dTree is built on top of a [dDatabase](https://npmjs.org/package/ddatabase), it brings with it all of dDatabase's powerful and distributed features, from being publicly auditable, to sparse and live replication over any network connection. Every dTree is announced on dWeb's DHT and made available to peers around the world, just as a dDrive is. A dTree is an off-chain key-value store that can be distributed like any other dWeb-based data structure, where anyone with an Internet connection can perform key lookups on any dTree and can receive the results, without downloading the entire dTree. In fact, they would only download the data associated with the lookup. dTrees are associated with a public/private keypair that is stored on the user's device and used to write to the dTree itself. dTrees, upon creation are announced on dWeb's DHT and sponsored/peered by dSocial's public peering service known as Orion, so that a user's data can always be replicated by anyone using the dSocial application, regardless if the creator goes offline and their dTree has no other peers seeding their data.

### DISTRIBUTED FILE SYSTEMS
When a user creates a post that contains some sort of large media file(s), those media files are stored within a dDrive and the dDrive's dWeb network address is stored alongside the post data within a user's dTree. A pseudo-representation of how this dDrive address would be stored within a user's dTree, alongside the post data its related to, can be found below:

**EXAMPLE OF A POST**
```
KEY            VALUE
======       ==============
/posts/1     {
                    postData: "This is a post",
                    postDrive: "dweb://ddrive-url",
                    postTimestamp: epoch,
                    inReplyTo: null
                  }
```

**EXAMPLE OF A COMMENT**
```
KEY            VALUE
======      ===============
/posts/2    {
                   postData: "This is a comment.",
                   postDrive: "dweb://ddrive-url",
                   postTimestamp: epoch,
                   inReplyTo: "dweb://dtree-url/posts/1"                 
                 }
``` 

dSocial's application, when viewing posts, will automatically download a dDrive that is associated with the post they're viewing, as long as `postDrive` isn't `null`. dDrives upon creation of a post, are announced on dWeb's DHT and sponsored/peered by Orion as well, so that they're always available to others on the network.

### USER ACCOUNTS + THE SOCIAL DATA MESH
Once a dTree is announced on dWeb's DHT, it is also announced by its creator on the ARISEN blockchain under dSocial's ARISEN-based on-chain database which is structured and defined by the `dsocialmesh` smart contract. Any user's data can be located by simply performing a lookup on ARISEN for a specific username, which will return the user's dTree key. That dTree can then be downloaded from its various seeds and traversed for specific records. A user's following and followers are also stored in the on-chain database, so that the dSocial application (client) can easily perform a lookup for a user's following and build out a "meshed" group of users and their dTree keys. This allows dSocial to live replicate data for a specific set of users and allows users to build their own networks around the users they follow, since they're only replicating data from the users they follow to begin with.

"Users" are simply [ARISEN](https://arisen.network) blockchain accounts (human readable names, i.e. @jared) which can include many different permission levels. In order for a user to update the dTree key associated with a specific ARISEN username, or a user's followers/following, it must be able to sign for these smart contract-based actions with the private key associated with the account-specific permission level that's required by the smart contract. Obviously, dSocial abstracts away a user's knowledge of ARISEN and private key-based authentication, which means users simply view their accounts as dSocial accounts and thanks to asymmetric cryptography, they're able to benefit from a completely password-less experience.

**NOTE:** A dTree's private key, is completely different than the private key associated with an ARISEN account. More on how dTree and ARISEN-based authentication is found below.

As mentioned above, dSocial utilizes a smart contract called "dsocialmesh" that you can learn more about in the [ARISEN Smart Contract](#arisen-smart-contract) section. This contract brings to life the user mesh described in this document. In this way, the dWeb network address related to a user's data can easily be retrieved, along with their follower data and the data associated with their upvotes.

### DWALLET AND USER AUTHENTICATION
dSocial deals with both "Local Authentication",  when writing data to their personal dTree, and "Remote Authentication", when writing data to the ARISEN blockchain or sending payments via another integrated blockchain. Local Authentication is quite easy, considering the private keys for a dTree are stored on a user's device and this authentication process is abstracted away from the user. Remote Authentication is an entirely different story. It's not a secret that blockchain-based authentication destroys the user experience for most blockchain-based applications and if dSocial were to have integrated directly with ARISEN, it would be no different than these other applications. For the past few months, we have been working on a solution to this. At first, we were going to call this app "PeepsID" but have instead chose to build all of this functionality into an app we're calling "dWallet." Currently the dWallet application is simply a RIX wallet, but we will soon release the second iteration of the dWallet application, which will allow users to do much more than simply send RIX to friends. dWallet will also allow users to sign for ARISEN-based smart contract actions and manage an unlimited number of Bitcoin, Ethereum, TRON and ARISEN accounts, while also allowing users to easily manage their earnings within those accounts. The plan is to integrate dWallet into all future apps like dRide, etc., so that all identity management, transaction signing and account management can take place from within the same application. 

dWallet from a user's perspective feels like a banking system and users really don't realize that it's being used to sign transactions or to manage blockchain-based identities thanks to dWallet's new "Simple" mode. At the very same time, thanks to the MoonPay API (https://moonpay.io), users of dWallet can easily purchase Bitcoin and Ethereum with a debit/credit card (without realizing they're using MoonPay), which will allow these users to participate in dWeb-based apps that require the use of cryptocurrency. This is the sort of onboarding that most cryptocurrency apps lack, and allows regular everyday Internet users to begin using decentralized applications. In the case of dSocial, this allows users to buy cryptocurrency, so that they can begin upvoting the posts of others. In other cases, users may not need to purchase cryptocurrency to use dSocial, since anyone can earn cryptocurrency for their posts and comments. In any event, dWallet gives users the option. dWallet's ability to handle transaction signing, without the user knowing about it, will simplify dSocial's overall user experience as well and will bring to life a familiar experience for users to that of the applications that they're already using on the traditional web.

## THE DESIGN
Below is the official design schematics for the dSocial application. The web, desktop and mobile versions of dSocial will all utilize the same design. The only requirement for any of these platforms to work would be that they have access to the social data mesh (dWeb's protocol suite) and the ARISEN blockchain, which are both distributed widely amongst the peers who use any of dSocial's platform-based distributions. The only other requirement, would be that users would have to have a local version of dWallet installed, due to blockchain-based transaction signing, in order to use dSocial.

**NOTE:** This will always be the case, anytime blockchain-based currencies are involved within an application, although, blockchain-less currencies like DWEB, would eliminate the overall need for an external wallet application, since transaction signing would happen at the dDatabase level. To add to that, we do use a blockchain for authenticating smart contract actions, but with the upcoming release of dVM, we will soon be able to utilize auditable off-chain smart contracts, that are powered using dDatabases as well. 

The breakdown of dSocial's design will be explained in the following sections:

- [Components, Frameworks & Libraries](#components-frameworks-and-libraries)
-- [Frontend](#frontend)
-- [ARISEN Smart Contract](#arisen-smart-contract)
-- [Tree Aggregator (Orion)](#tree-aggregator)
-- [Index Aggregator (Lunar)](#index-aggregator)
- [Accounts](#accounts)
-- [Logging In](#logging-in)
-- [Signing Up](#signing-up)
- [Distributed Data](#distributed-data)
-- [Distributed Data Schemas](#distributed-data-schemas)
- [Authority Model](#authority-model)
- [Social Data Mesh](#social-data-mesh)
-- [Announcing dTrie Keys](#announcing-dtree-keys)
-- [Following Users](#following-users)
-- [Distributed Data Aggregation](#distributed-data-aggregation)
-- [Replanting dTries)(#replanting-dtrees)
- [Search Indexing](#search-indexing)
- [Cryptocurrency Payments](#cryptocurrency-payments)
- [Governance & Moderation](#governance-and-moderation)
-- [dWeb Constitution](#dweb-constitution)
-- [Decentralized Reporting System](#decentralized-reporting-system)

### COMPONENTS, FRAMEWORKS AND LIBRARIES
dSocial's frontend and backend components are built with a variety of different libraries and frameworks, all of which are JavaScript-based. Below is a breakdown of dSocial's different components and the frameworks and libraries that are being used to develop each of them.

#### FRONTEND
dSocial's frontend is designed to be available across all web, desktop and mobile platforms. While the goal is to utilize a single codebase for all platforms, at first, we're developing a separate frontend for each platform. The tools used to develop these frontend platforms are listed below:

##### WEB FRONTEND
The web frontend is built using the following libraries and frameworks:
- React.JS
- Webpack
- BabelJS
- Nodemon (Development)
- [dWeb SDK](https://npmjs.com/package/dweb-sdk)
- [Basestore](https://npmjs.com/package/basestorex)
- [Basestore Networker](https://npmjs.com/package/@basestore/networker)
- [DemuxJS](https://npmjs.com/package/demuxjs)
- [ARISEN SDK](https://npmjs.com/package/arisen-sdk)
- [dTree](https://npmjs.com/package/dwebtree)
- [dIndex](https://npmjs.com/package/dwebindex)
- [dWalletJS](#) // Launching soon
- [ARISEN UAL ReactJS Renderer](https://github.com/arisenual/react-renderer)
- [dDatabase Multi-Key](https://npmjs.com/package/@ddatabase/multi-key)
- [dDatabase Crypto](https://npmjs.com/package/@ddatabase/crypto)

##### DESKTOP FRONTEND (Node.JS)
React.JS
Electron
Webpack
BabelJS
Nodemon (Development)
Basestore
Basestore Networker
ARISEN SDK
dTree
dIndex
dWalletJS
[dWeb Electron Runtime](https://github.com/dwebprotocol/dweb-electron-runtime)
ARISEN UAL ReactJS Renderer
dDatabase Crypto

##### Mobile Frontend (React Native)
React Native
dWeb SDK
Basestore
Basestore Networker
dTree
dIndex
Random Access RN File 
dWalletJS
ARISEN UAL ReactJS Renderer
dDatabase Crypto

#### ARISEN SMART CONTRACT
dSocial uses an ARISEN-powered smart contract for announcing keys for different distributed data structures, maintaining the state of a user's following and the consensus and storage of upvote-related data. The ARISEN smart contract is written in C++ and contains the following actions:

- postskey - Used for announcing the network address of a user's dTrie related to post-related data
- loveskey - Used for announcing the network address of a user's dTrie related to love-related data
- replieskey - Used for announcing the network address of a user's dTrie related to reply-related data
- repostskey - Used for announcing the network address of a user's dTrie related to repost-related data
- profilekey - Used for announcing the network address of a user's dTrie related to profile-related data
- follow - Used for following a user
- unfollow - Used for unfollowing a user
- upvote - Used for upvoting a post

More information on how dSocial's smart contract works, can be found in [Announcing dTrie Keys](#announcing-dtrie-keys), [Following Users](#following-users)

#### TREE AGGREGATOR
dSocial interfaces with a globally distributed database, that could be made up of millions or even billions of smaller databases, all maintained by the individual users who created them. In order to merge these databases into a single database, specific database schemas are used, and a new mounting feature, that allows one distributed database to be mounted into another. When the mounted database is updated, the database it was mounted within is also updated. This allows the users of individual databases to control their own data, while also allowing dSocial to interface with a single database. This also insures that dSocial's developers would be unable to edit the data, since the append-only database at any given mountpoint, can only be edited by the database's creator (a specific dSocial user). 

dSocial users each maintain 5 distributed databases on their computers, where their data and interactions within dSocial are ultimately stored. Each database is used to store data for specific content types (posts, loves, replies, reposts and profile data) and each of these databases have a network key. These network keys are announced on ARISEN, using various smart contract actions that are ultimately used to announce the dWeb network addresses of a user's different databases.  ARISEN gives us a way to know about every single user's database addresses, although, an aggregator was needed to retrieve these addresses from ARISEN and mount them into a single database. This aggregator is known as "Orion" and simply listens on ARISEN for newly added keys and mounts them into a database that Orion manages and then distributes on the dWeb. 

There is more information about Orion, in [Distributed Data](#distributed-data) and [Distributed Data Aggregation](#distributed-data-aggregation).

Orion is built using the following libraries and tools:

[Subcommand](https://npmjs.com/package/subcommand)
dTree
[dHub](https://npmjs.com/package/dhub)
[Mountable dWebTrie](https://npmjs.com/package/mountable-dwebtrie)

##### INDEX AGGREGATOR
The Index Aggregator, also known as Lunar, works just like Orion, the only difference between that Lunar is an aggregator for dIndexes rather than dTries. Each dSocial user keeps an index of all their posts, along with the keywords (hashtags and most popular words) that are associated with each post. These indexes are announced on ARISEN and Lunar listens for these announcements, and then mounts them into a globally distributed dIndex that's under its management. dSocial's search engine searches this globally distributed index for posts that match a specific keyword and returns all matching posts to dSocial's frontend for sorting.

There is more information about Lunar, in [Search Indexing](#search-indexing).

Lunar is built using the following libraries and tools:

Subcommand
[dWebTrie](https://npmjs.com/package/dwebtrie)
dWebIndex
dHub
Mountable dWebTrie

## AUTHORITY MODEL
As mentioned early on in the design document, dSocial uses two different forms of authentication, albeit Local Authentication and Remote Authentication. Both are explained below:

### Local Authentication
When a user creates data on dSocial, whether it be creating a post, loving a post, replying to a post or reposting a post, this data is written to a specific distributed database used for each of these data types. These distributed databases (dTries), are located on the user's computer, along with the private keys required to append data to each of them. Since the database is local to the user's computer, authentication happens locally while the entire process is hidden from the user.

### Remote Authentication
When a user announces the network address of a distributed database, follows another user or upvotes a post, these actions occur on a remote blockchain and therefore require Remote Authentication. Remote Authentication is used every time one of these actions take place and is the chosen form of authentication used within dSocial's frontend. Frontend authentication is described in the sections that follow.

### Signing Up
Account creation for dSocial happens within dWallet, where a user's keys are securely generated, encrypted and securely stored on a user's device. The following onboarding process happens via the Signup page:

== DETECTION:
dSocial detects whether or not dWallet has been installed on the user's device

== IF NOT INSTALLED:
- The user is shown a screen that tells them that they need to download dWallet to continue using dSocial. A button is provided with a direct link to download dWallet for their operating system. Once clicked, the dWallet installer is immediately downloaded on the user's device.

== ONCE DOWNLOADED:
- The screen updates to show a user a quick graphic on how easy it is to install dWallet using the downloaded installer.

== ONCE INSTALLED:
- The screen changes and lets the user know that dWallet has been detected and lets them know that they have to create an account within dWallet and provides a button to the user that when clicked, will open up dWallet's account creation screen.

== ONCE OPENED:
- The user is asked to choose a username. If available, they click "Signup" and their private keys are generated, the account is registered on ARISEN and the keys are securely saved on their device. It's important to note that the user never sees keys in the process.

### Logging In
If dWallet is downloaded, the login screen will automatically pull up a dropdown menu that contains all of the usernames that exist within dWallet.  When accessing dSocial's frontend, dSocial interfaces with dWallet in the background to ensure that a user possesses the private key related to a given username and that the specified account does in-fact exist within dWallet. If so, the user is able to login. Really, authentication is not required at this step, since a user's interactions are saved locally on their device. Although, when remote interactions (following a user) do indeed occur, this actual authentication will occur, so it's important for a user's experience, to keep the "Login" step, to ensure that a user would never get to this point, without possessing the keys required to authenticate themselves. By having the login step, we can ensure that this particular user can easily authenticate themselves both locally and remote, and therefore will be able to use the application in its entireity. 

## DISTRIBUTED DATA
dSocial utilizes various distributed data structures within its design, namely Mountable dTries and dIndexes. These data structures are built on top of the dDatabase data structure and can easily be exchanged between peers using the dWeb protocol suite. Each individual user owns, controls, manages and broadcasts their own set of distributed databases that ultimately store the data related to their interactions within the dSocial application. By separating our data types out into separate databases, we're able to easily add new features and data types to our application, without disturbing our legacy features and data types down the road. It also further distributes data across dSocial's users. While data is managed at the local level by each individual user, this data is aggregated into a single distributed database at a remote level, by a distributed data aggregator known as "Orion". In order to achieve this, dSocial uses strict data schemas, at the local level, which makes all of these individual databases easily mountable at the remote level. Below is a description of dSocial's distributed database schemas:

### LOCAL SCHEMAS
When writing data to local dTries, dSocial forces users to abide by the following schemas:

#### POSTS
This schema is used when adding data to a user's "post" trie:

- **Key:** 
`/{id}`

- **Value:**
```
{
  postingUser: @username,
  postTimestamp: epoch,
  postData: <string>,
  postDrive: dDrive network address,
  inReplyTo: <postID> or null,
  postIndexKey: <postIndexKey>
}
```

#### REPLIES
This schema is used when adding data to a user's "replies" trie:

- **Key:** 
`/{postID}/{replyingUser}/i++`

NOTE: `i++` defines the incrementation related to a user's replies, in related to a specific postID. For example, if you reply to the same postID twice, for the second reply, `i++` would be equal to `2`.

- **Value:**
```
{
  postPointer: {id},
  replyTimestamp: epoch
}
```

#### LOVES
This schema is used when adding data to a user's "loves" trie:

- **Key:**
`/{postID}/{lovingUser}`

- **Value:**
``
{
  lovingUser: @username,
  loveTimestamp: epoch
}
```

#### REPOSTS
This schema is used when adding data to a user's "reposts" trie:

- **Key:**
`/{postID}/user`

- **Value:**
```
{
  repostingUser: @username,
  repostTimestamp: epoch
}
```

#### PROFILE
This schema is used when adding data to a user's "profile" trie:

##### PROFILE DATA
- **Key:**
`/profile`

- **Value:**
```
{
  profileUser: @username,
  profileLocation: <string>,
  profileBio: <string>,
  displayName: <string>,
  profileUrl: <string>
}
```

##### PROFILE IMAGE
- **Key:**
`/pimage`

- **Value:**
<ddrive-key>/image.jpg 
(converted image)

##### PROFILE HEADER IMAGE
- **Key:**
`/himage`

- **Value:**
<ddrive-key>/image.jpg
(converted image)

### REMOTE SCHEMAS
When mounting data within a global dTrie, Orion uses specific mountpoints within the global dTrie, to mount the dTries of individual users. 

#### POSTS
- **Mountpoint:**
`/posts/`

When mounting a dTrie, at the /posts/ mountpoint, you could access the posts of users, using the following format:
- `/posts/{id}`

This is possible because the key within each user's individual dTrie, is simply the {id} of their posts and by mounting at `/posts/` we can search posts by ID, easily.

#### LOVES
- **Mountpoint**:
`/loves/`

When mounting a dTrie, at the /loves/ mountpoint, you could access the loves of users, using the following format:
- `/loves/{postID}/{user}`

This is possible because the key within each user's individual `love` dTrie, is `/{postID}/{username}` and by mounting at `/loves/` we can search all loves from every single mounted user, by postID.

#### REPLIES
- **Mountpoint:**
`/replies/`

When mounting a dTrie, at the /replies/ mountpoint, you can access the replies for a particular post, using the following format:
- `/replies/{postID}/{replyingUser}/i++`

This is possible because the key within each user's individual "replies" dTrie, is `/{postID}/{replyingUser}/i++` and by mounting at `/replies/` we can search all replies for every single mounted user, by postID.

#### PROFILE
- **Mountpoint:**
`/{username}/`

When mounting a dTrie at the /{username}/ mountpoint, you can access the profile data for a particular user, using the following formats:
- `/{username}/profile`
- `/{username}/pimage`
- `/{username}/himage`

## SOCIAL DATA MESH
By keeping a list of every user's dTrie keys and their follower list on ARISEN, we end up forming a "social data mesh" that we can use to create globally distributed database for the network. Above, you saw how individual dTries could be mounted into a single globally distributed dTrie at specific mountpoints, which would allow dSocial to interface with a single database, rather than having to interface with potentially billions of distributed databases at once. As was described earlier in this design document, an aggregator is needed to accomplish this and handle the mounting of each individual user's dTries, into this globally distributed dTrie. In the sections that follow, the methods surrounding dTrie key announcement, the following of users, distributed data aggregation and how dTries are shared amongst a user's devices, is explained, in-depth.

### ANNOUNCING DTRIE KEYS
dTrie keys are announced and stored on the ARISEN blockchain, using dSocial's publicly available smart contract. Each key-type related to each database type (posts, reposts, replies, loves, profile) has their own action (postskey, repostskey, replieskey, loveskey, profilekey) that's used to announce dTrie keys on-chain. These keys can be updated anytime by the ARISEN user who created the original record.

### FOLLOWING USERS
One user can easily follow or unfollow another user, using the `follow` and `unfollow` actions via dSocial's smart contract. Follower data can easily be fetched from ARISEN, either by user, or by follower.

### DISTRIBUTED DATA AGGREGATION
Orion is a Node.JS-based application that runs on a single server. It's job is to listen to ARISEN for on-chain updates to the dTrie keys of dSocial users and to mount those keys into the global dTrie that it's responsible for managing and distributing. Orion uses different listeners, along with DemuxJS, to keep tabs on the state changes surrounding dSocial's smart contract and is able to quickly mount newly added or updated dTrie keys to the globally distributed database. The dTrie managed by Orion is then shared on dWeb's DHT and directly utilized by dSocial's application. This means, users of dSocial's application end up peering this data as a result, and ultimately end up exchanging this data amongst each other. 

Orion's job is to simply mount individual dTries into the global dTrie it manages, and is able to do this by getting the keys associated with dTries and mounting them into its global "mountable" dTrie (https://npmjs.com/package/mountable-dwebtrie). The data from these individual dTries is not actually saved in the global dTrie, instead, a pointer to the individual dTrie is mounted (stored) within the global dTrie. In this way, the server powering Orion uses minimal resources (2GB RAM/20GB DISK/2 vCPU) and is the only infrastructure used by the dSocial application. Being that dSocial is downloading data from the seeds of the global dTrie, rather than the Orion server itself, resources are only needed to handle dTrie mounting within the global dTrie, hence the minimal resource requirements mentioned above.

#### Data Creation At The Local Layer
A pseudo representation of how data is created at the local layer is found below:
```
User Data => dTrie => DHT/ARISEN
```

- The dTree is announced on dWeb's DHT
- It's peered by dSocial's peering service
- The `key` action is executed within dSocial's ARISEN contract and the user's dTree key is stored on ARISEN, alongside the user's ARISEN account

#### Data Aggregation/Peering At The Remote Layer
A pseudo representation of how data is created at the remote layer is found below:
```
Post dTrie => Post Mount (/posts/) => Global dTrie
```

- The data aggregator is constantly listening for `key` action execution on ARISEN
- The data aggregator keeps a local DB (levelDB) of the global dTrie key so that the aggregator can be ran persistently across restarts.
- The data aggregator uses DemuxJS to maintain the local state surrounding the latest updated list of dTrie keys on ARISEN
- Using dHub and the dHub Mirroring Service (https://npmjs.com/package/dhub-mirroring-service), dSocial's public peering service becomes a persistent peer for each user's individual dTries.
- The data aggregator begins mounting each user's individual dTries to a specific mount point (for example, the "posts" dTrie for each user, is mounted at the "/posts/" mountpoint within the global dTrie managed by the aggregator).
- The data aggregator announces the global dTrie on dWeb's DHT and the dSocial application's users begin retrieving data from it, based on those they follow and live-sync changes so that their dSocial experience is essentially "live streaming."

### REPLANTING DTRIES AND A USER'S DINDEX
A dTrie and a dIndex are the two most popular data structures used by dSocial and both can only be written to by the holder of each structure's private key. When a user first starts using dSocial, an initial dTrie (for each data type) and a dIndex are created on their device, along with a keypair that's used for writing to both data structures. The problems arise when a dSocial user attempts to use dSocial on a different device. Since moving private keys from one device to another brings about security concerns, we ended up creating a process called "replanting" that allows dWeb data structures to be migrated and rekeyed on a new device, so that as users begin using a new device, they can continue manipulating their dTrie and dIndex. 

Replanting works as follows:
1. Bob starts using dSocial on his desktop and over time, adds various data to his dTries and dIndex.
2. His dTries and dIndex are announced on ARISEN
3. Bob downloads dSocial on his iPhone and upon opening dSocial, he logs in as @bob
4. dSocial looks for the dTrie keys and dIndex key related to @bob's profile on ARISEN, and checks to see whether Bob has write access to these data structures on his mobile device. dSocial realizes that Bob does not have either and therefore doesn't have write access.
5. Using [dDatabase Multi-Key](https://npmjs.com/package/@ddatabase/multi-key), dSocial forks (clones) Bob's dTries and dIndex into newly created and rekeyed dTries and does the same with Bob's dIndex, on Bob's iPhone. It then announces the new network addresses for all dTries and Bob's dIndex on ARISEN.

NOTE: When forking into a new structure, all of the data from the old structure is moved to the new structure and Bob continues writing to the new structure. This allows Bob to continue adding data to a database, that contains the same state that existed in the database on Bob's desktop device. When Bob switches back to his desktop, this same replanting process occurs; where a lookup on ARISEN occurs, dSocial ends up forking the data structures peered by Bob's mobile device and again, announces those keys on ARISEN. This replaces the old keys on ARISEN, which means Orion would remove the old mountpoints and would add Bob's new mountpoints. These updates are live across the network, for ALL dSocial users. As Bob switches between devices, the most recently announced data structures are forked on the current device and re-announced so that this process can continue.

## SEARCH INDEXING
Like data types on dSocial, data indexing happens at the local layer and is aggregated at a remote layer, using a similar system to Orion, known as "Lunar." Below, both the local and remote layers for search indexing is explained:

### Data Indexing At The Local Level
- When a user is writing data to a dTrie, they are also linking that data within a dIndex (an abstraction on top of a dWebTrie key-value store). 
- A user's index only stores post data and the keywords associated with their post, where hashtags take prominence in the index's keyword array. Hashtags are stored as actual hashtags in the keyword array so that dSocial's frontend can distinguish between regular words and hashtags.
- Each entry in the index carries the following schema:

```
{
  postID: <postID>,
  postTimestamp: epoch,
} keywords: [#hashtag, #hashtag2, keyword1, keyword2, etc.]
```

The user's index is announced on dWeb's DHT, it's peered by dSocial's peering service and the `dindex` action is executed within dSocial's ARISEN contract and the user's dIndex key is finally stored on ARISEN, alongside the user's ARISEN account.

### Index Aggregation At The Remote Level
A pseudo representation of how index aggregation would work at the remote level is found below:
```
User dIndex => Global dIndex Mountpoint (/lunar/) => Global dIndex
```

- Index aggregator is constantly listening for `dindex` action execution on ARISEN to find newly announced dIndexes or updated dIndex addresses.
- Index aggregator maintains a local DB that contains the keys associated with the global dIndex, so that the system can run persistently across restarts.
- Using dHub, the index aggregator mounts each dIndex to a global dIndex, that is under its management, just like the data aggregator does.
- The global dIndex that's managed by the data aggregator is announced on dWeb's DHT and directly used by dSocial's distributed search engine. Each dSocial user ends up seeding portions of it, through their usage of the network.

It's important to note that each index only stores a pointer to the PostID and not the actual post data itself. This way, dSocial's frontend can query the global dIndex distributed by Lunar for the postID's related to a specific keyword search and then pull the post data related to these postID's from the global dTrie distributed by Orion.

## CRYPTOCURRENCY PAYMENTS
dSocial allows users to upvote the posts of other users, using various cryptocurrencies that are integrated with the platform. Those cryptocurrencies will initially include Bitcoin, Ethereum, TRON and RIX. dSocial will interface with the dWallet application to display a user's earnings, and to handle transaction signing when one user sends money to another user. This process is seemless and what most dApp user are already familiar with. Upvote data is then stored on ARISEN, using the `upvote` action found within dSocial's smart contract. The dSocial platform charges a 3% fee for every single post upvote, to support development and marketing costs of the platform.

## GOVERNANCE & CONTENT MODERATION
Being that dSocial is a gay-owned platform, we understand the importance of equality. With that said, we believe there is a fine line between censorship and protecting the community from clear hate speech, violence and illegal activity. A lawless community would never survive, and platforms like Parler who are lacking in fair content moderation policies are being removed from the world's most popular app stores like Apple and Google's. Content moderation should never be handled by a platform's developers. Instead, we believe it should be handled by the community itself. dSocial follows dWeb's constitution and dWeb's elected governance (via the ARISEN blockchain) has the ability to remove data on-chain that is related to violations of its on-chain constitution. dSocial also uses a decentralized reporting system known as dReport that allows community members to report violations to the governance. Since dSocial stores a user's dTrie keys and each of their dIndex keys on ARISEN, the governance, in the face of a violation, has the ability to remove these keys from dSocial's on-chain database. In this particular case, this would cause Orion to unmount those keys from dSocial's globally distributed dTrie, which would cause the data to disappear from the network. This is an approach that requires a 15/21 vote from elected governance members.

You can learn more about ARISEN's governance [here](https://arisen.network/governance.html)

## CONCLUSION
Each individual user of dSocial controls their data via their own individual dTries and then distribute that data across the dWeb. Orion is able to aggregate this data (as a peer of all of these databases) into a single globally distributed database that is seeded in small portions by each of dSocial's users. ARISEN helps form this data mesh by storing the dTrie and dIndex keys of users within its global state and making this data publicly available. This mesh is then manipulated by aggregators like Orion and Lunar, to bring to life a live, distributed view of dSocial data and a truly distributed search engine, without the need for storing all of this data on-chain or using expensive centralized infrastructure in the process. This allows dSocial to operate with hardly any infrastructure costs, where users have incentives to create content and the platform can monetize itself through the actions of its users. This allows the platform to focus its earnings on marketing and development, rather than infrastructure costs that most platforms on the web today are unable to afford, even with hundreds of millions of users. Even though the platform itself utilizes decentralized and distributed technologies, dSocial is still able to utilize the community itself for content moderation, through an elected blockchain-based governance that can remove data in the face of illegal activity, violence or hate speech, according to a constitution that is ratified and constantly updated by the community itself.
