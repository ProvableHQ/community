
# snarkOS Standup Notes (7/29/21)

Welcome to the notes page for the most recent snarkOS standup. Please see below for topics covered during the public discussion:

## Agenda

* Weekly standups from the snarkOS team (30 min.)
* Current sprint epic conversation (10 min.)
* Community questions (15 min.)

## Standups


### Lukasz:

* fixed the snarkOS issue where the `getnodestats` RPC would not provide stats when running with the `prometheus` feature
* proposed a private RPC call to connect to the given addresses
* extended RPC tests
* fixed a network crawler peering bug
* fixed the queued outbound message count metric (appears to be solved for good this time)
* improved the performance of the network crawler
* did multiple PR reviews for snarkVM
* filed a PR with improved (randomized) test data for some snarkVM tests

### Max:

* Storage refactor stable in testing -- finishing up rustdocs and unit tests

### Niklas:

OOO

### Akis:

### Jules:

### Fabiano:

OOO

## Orphan Rate

Tracking Epic:  

Max:
* TODO: aggressively restart miners when new canon block is found (https://github.com/AleoHQ/snarkOS/issues/970)

## Multi-Network Support

Tracking Epic:  

Max:
* TODO: introduce new serialization format: https://github.com/AleoHQ/snarkOS/issues/971

## Take-aways:

## Questions (Community welcome!)



-------------


# snarkOS Standup Notes (7/22/21)

Welcome to the notes page for the most recent snarkOS standup. Please see below for topics covered during the public discussion:

## Agenda

* Weekly standups from the snarkOS team (30 min.)
* Current sprint epic conversation (10 min.)
* Community questions (15 min.)

## Standups


### Lukasz:

* sped up the storage validator tool by ~40%, shared detected some small performance wins with snarkVM
* added the storage validator tweaks requested by Fabiano
* reduced the CI runtime by disabling duplicate tests
* reduced the incidence of CI job failures by tweaking flaky network tests
* done some snarkVM reviews (the engineering/code side)
* handled a few snarkOS contributions (1 issue, 2 PRs) from community members

Blocker:  Pull request with snarkVM update.  testnet2 related refactoring.  Transaction kernel doesn't work. See:  PR:  937


### Max:
* Written, currently testing storage refactor
* We will need a complete rewrite of db schema to be multi-net compatible (properly), future PR though
* Currently is backwards compatible with old schema

Will put the prelim PR up on a branch so we can review.

Blockers: None.

### Niklas:

* Implemented improvements to peer lists by only propagating routable addresses (details in [#927](https://github.com/AleoHQ/snarkOS/pull/927)).
* Proposed the separation of crawlers from bootnodes (details and discussion in [#957](https://github.com/AleoHQ/snarkOS/pull/957)), opened [#960](https://github.com/AleoHQ/snarkOS/issues/960) to track the discussion regarding the removal of the sync layer from bootnodes and crawlers.
* Refactored unnecessary async functions to be synchronous where possible. Improved the peering tests and related setup code following the peering changes.
* Multiple PR reviews.

Let's add an ARC to document the definition of a bootnode, mining node, client node.

### Akis:

* Orphan Rate:  No progress this week.

PR went in.  It's in staging.  Do we need cut a release? Master is two weeks behind.  Metrics are most accurate on staging right now.  Let's cut a release before we merge the snarkVM update.  Howard to cut a release today or tomorrow.

The next snarkVM merge will be the last snarkVM to support testnet1.  We will move from 0.7.x to 1.x.  

Lukasz will check the nodes to make sure that we're able to gather the data for orphan rates.

### Jules:

Working on Setup.

### Fabiano:

OOO

## Orphan Rate

Tracking Epic:  

## Multi-Network Support

Tracking Epic:  

## Take-aways:

* Need an ARC for node definitions:  [#957](https://github.com/AleoHQ/snarkOS/pull/957)), opened [#960](https://github.com/AleoHQ/snarkOS/issues/960) 
* The next snarkVM merge will be the last snarkVM to support testnet1.  We will move from 0.7.x to 1.x.  
* Max:  Will put the prelim PR up on a branch so we can review.

## Questions (Community welcome!)



-------------


# snarkOS Standup Notes (7/15/21)

Welcome to the notes page for the most recent snarkOS standup. Please see below for topics covered during the public discussion:

## Agenda

* Weekly standups from the snarkOS team (30 min.)
* Current sprint epic conversation (10 min.)
* Community questions (15 min.)

## Standups

### Lukasz:

* Detected, investigated and reported some oddities related to the commitment tree merkle tree root calculation.
* Done multiple PR reviews.
* Further improvements to the storage validator tool.
* Improved and added some test cases.
* Slightly blocked on https://github.com/AleoHQ/snarkOS/pull/937 being merged.

Fabiano:  Can we add an option so that it immediately exits after validation?   Answer:  Yes.  

### Max:

* SnarkVM IR still waiting for review (presumably deprioritized by testnet2 work) -- need to bug howard more. Howard:  We can look at the IR next week.  We need to do the setup ceremony first.  Howard is working on that first.
* Trait refactors in snarkVM in preparation for storage refactor cancelled by request from howard.  (Howard wants to do them himself).
* For storage refactor, storage model is done, working on a separate ledger model during integration of storage model in consensus

What can we do to stop the miner stalls?  Max:  Process blocks as a critical section could help.  There could be a race condition w/ the status checks.  Rewrite of storage implicitly will include a fix for this.  

### Niklas:

* Implemented a fork detection tool, the output of which is exposed via the `getnetworkgraph` rpc endpoint. It maps potential fork heights to member nodes of the fork. This was merged into `staging` yesterday.
* PR reviews and testnet crawler db investigations (26k fork).
* Currently continuing work on peering improvements and bugfixes.
* No blockers.

### Akis:

* Currently working on PoSW docs.  
* PR for the orphan blocks has been approved.  Need to merge into staging.
* Taking a snapshot of the orphan rate every `x` blocks should give us easy to read stats for the orphan rate.
* Initially we want to get the numbers for the last 24-48 hours.  I can put up a script and get some numbers on some real-time graph.  Neet to watch rate changes after changes are pushed.  Fabiano to help with creating a visualization of this rate.

### Jules:

* Just gotten the tie-breaker patch merged into staging
* Working on Setup and away from this project.

### Fabiano:

* Running all the nodes w/ min 100 and max of 5000.  All fine.
* One node which started downloading the whole change.
* Refactored the deployment automation due to a bug.  Added automatic backup of the database so we can roll-back if we need to.
* Ran us1 w/ the optimized TCP stack settings.  This week:  Roll out to the other nodes.
* Want to replace the IPs of the bootnodes w/ floating IPs so that we can hot-patch. Both old and new IPs will still work.

## Orphan Rate

Tracking Epic:  https://github.com/aleohq/snarkos/issues/826

## Multi-Network Support

Tracking Epic:  https://github.com/AleoHQ/snarkOS/issues/928

* Storage refactor is the blocker/enabler for multi-network support.  
* Most important thing to start:  Make sure what's represented on disk is modular per network.
* Single network per snarkOS node.

## Questions (Community welcome!)

* Fabiano:  Should we run storage validator on every run for auto-deploy?  Answer:  Makes sense.  For auto-deploy, we can run the validation as a test before starting a node.
* How to expedite storage fix problems.
Decouple ledger from storage, decouple storage from network context, stricter enforcement of constraints (i.e. indexes)

 

-------------


# snarkOS Standup Notes (7/8/21)

Welcome to the notes page for the most recent snarkOS standup. Please see below for topics covered during the public discussion:

## Agenda

* Weekly standups from the snarkOS team (30 min.)
* Current sprint epic conversation (10 min.)
* Community questions (15 min.)

## Standups

### Lukasz:

* Squashing bugs related to storage and metrics.  Working on final bits in storage:  Coherence of indexes--contiguous.
* When we have the spec for different network implementation, will work on it.
* No blockers.

### Max:

Update on [snarkVM IR](https://github.com/AleoHQ/snarkVM/issues/218):  Everything is implemented in snarkVM.  Needs to be rebased.  Waiting on testnet2 and the design doc sent out to the snarkVM team.  Leo impl is done and ready to go.  No relevance to snarkOS right now.  When we start doing the program registry it will be more relevant.

Started to work on better abstraction layer for application level storage.  Allows us to maintain coherency of our database while maintaining performance.  Also allows other database impls.  Did most of the refactors that are highly required in snarkVM to facilitate this. Mostly done.  Now blocked on testnet2 to avoid conflicts.  This will help snarkVM code quality.  Should be straightforward to move to our new storage system.  For now, just moving into the model and will use rocksdb and be backwards compatible.  Implementing sqllite later will be easier later.  Not now though.

Next:  In snarkOS:  rebasing snarkVM storage changes, then doing the actual impl for new storage model.

CUDA support for mining has been merged into snarkVM.  Once testnet2 is merged, will have a cargo for CUDA.  We haven't tested Windows support.  Help wanted from community here!  :)

### Niklas:

* Crawler that was merged last friday is now running on testnet1.  Exposed some tracking measurements via RPC.  Improves how we're able to analyze the network topology.  Pairing changes.  Improves tracking of network forks.  Experimenting with another solution.
* Next:  Improving cluster detection, island detection in the network.
* No blockers.

### Akis:

* Currently:  Pushed the PR for the orphan rate which adds counters for each node which keeps track of orphan blocks.  Justed added an extra counter in the snarkOS metrics, imported that into the consensus code.  Tried to keep the same structures as the other RPC structures.
* Went through the impl of PoSW.  Need Pratyush and Howard.  Need to do an extra check while doing the PoSW.  Will file an issue.
* Now have a miner running and it's winning blocks.  Some things came up:  Couldn't connect with a parent node.  Getting blocks not really connected to the chain.  Max:  One thing can happen at the sync layer:  If one peer doesn't respond for the same set of blocks then we have to re-request blocks from a different peer, and that can delay the processing of the blocks, but we spend more time fast-forwarding.
* No blockers.


### Jules:
* Pushed the PR for the impl for the orphan tie breaker:  Goes back to the fork point and tallies up the difficulties.  Been testing on nodes to make sure there's no perf issues or bugs.  Still running into stalls.  Need to see if this is a regression.
* Speaking with Fabiano to get setup with devnet.
* No blockers.
* Will be moving to work on Setup, too.

### Fabiano:

* Completed the pipeline to deploy master and staging to testnet.  Going to deploy half the nodes to master and half to staging.  Will increase the max peers.
* Next:  Work on the pipeline.  Fine tune the TCP stack to see if we can get more perf.
* No blockers.

## Orphan Rate

Tracking Epic:  https://github.com/aleohq/snarkos/issues/826

## Multi-Network Support

Tracking Epic:  https://github.com/AleoHQ/snarkOS/issues/928

High-level Goals:
* Most important:  We need to have a standard convention for network ids and ports.  Currently we use 4130 plus the network ID.  
* For each request, do we want to encode the network ID for each RPC request?  There's explicit vs implicit (i.e. 1 is `testnet1`).
* We are updating the generators for the curves.  This is a breaking change between `testnet` and `testnet2`.  This affects address generation and signing operations.
* Do we want to run multiple networks in a single instance of snarkOS?  Under the hood, we'd want to modularize the databases for each network?
* There may be different curves per network, so nodes should be modular
* Multi-dbs will be described in the new abstraction impl.
* Decoupling storage from ledger
* We want to support an abstraction on the ledger from storage so if you want to load from disc your ledger you should be able to do that, but also, it should be easy to load ledger in Wasm.  Third option:  Make a ledger that's only for remote calls--RPC only.  A ledger:  A tracker of three merkle trees + a block store:  Serial, Commitment, Memo trees.  For transactions:  Merkle trees are more relevant than the block source.  Block store:  Valuable if you want to verify the validity of a block.  If you are live client, receive a block, you can verify the latest block is reasonably correct from the previous block--for verification by lite clients.  To minimize state space for block headers, we could have a merkle tree.  
* Removing encryption from the entire Aleo network P2P.  Doesn't prevent MITM and all the content should be public anyway.  See: https://github.com/AleoHQ/snarkOS/issues/819


* [Need to write an ARC](https://github.com/aleohq/snarkos/issues/929) (Aleo Request for Comments):  https://github.com/AleoHQ/ARCs
* Community network migration plan towards `testnet2` : https://github.com/aleohq/snarkos/issues/935
** Let's put in a transition plan over the coming quarter:  September or October we will make the switch.  Will give us time to confirm that the generators are working as expected and it gives us time to implement modular networks.
** Gives us time to seperate storage from ledger
** Deprecate testnet1
** Because of the changes we're making in snarkVM, it will break the underlying assuptions of testnet1.  snarkVM 0.7.x will likely be the last compatible VM for testnet1.  1.0.x will not be compatible with testnet1.

## Potential Future Work

Covered already.  

## Community Questions

Please write any questions that you would like to have answered during the snarkOS standup below (Please use bullet point format):

- (i.e.) What are the hardware requirements to run a mining node on the Aleo testnet? Any modern processor, 8gig RAM necessary minimum requirement (just to compile), seperately:  Compilation times of snarkVM:  Threadripper:  1min30s to compile snarkVM.  Maxed out mac pro:  3min30s.  M1. 6min30s compile from clean;  Intel-based MBP:  8min for clean compile.  If you want to run a client node, it doesn't really matter; however, if you want to run a mining node to win blocks, you will need a beefier machine or use a cloud provider.  Also, use the CUDA impl to use MSM to double perf (or roughly 30-40% perf improvemnt as we've seen so far).  Cloud providers can provide CUDA, too.
- Does the blockchain contain info about the keys that each particular block is mined? If yes, why isn't the block explorer showing that?  The coinbase tx in each block, it's the only transaction with the negative balance value.  View keys are currently not on the blockchain, so transactions are private for each miner node.  To see blocks that are mined, use private endpoints, you can get your record commitments, then you can use decode and decrypt functions to turn them into human readable format.  If you are running a miner, those private endpoints will get you access to the transactions/blocks that you mined.  
- Our community members running mining nodes but sometimes their nodes stopping. When that happens they are restarting their mining nodes and their numbers starting from  0. Are their mining scores registering in your system or are their mining scores staying in their wallet?  Answer:  Yes.  The miners will be still credited when the database is reset as long as one uses the same miner address as when the node was started previously.  In a coinbase transactions, two-in two-out.  Because all the transactions are public, the decode record works--Need to verify this.  The decrypt record requires a view key:  https://developer.aleo.org/aleo/getting_started/overview/#chapter-12-private-rpc-endpoints
- I have a wallet address on the miner, but there must also be a file where data about mined blocks is written. So, nothing is written to this file although I see how many blocks have been mined at the moment. Do I need to correct the situation so that records are made to this file, or can I just know the wallet address? I also have a node, but now I'm talking about miner. Answer:  To see the balance you need the view key.
- How is the progress on the CUDA (GPU based) implementation?  See:  https://github.com/AleoHQ/snarkOS/issues/936. Could be available as early as today.  Will roll it out to staging directly.  :)
- Do we have a way to pull block timestamp, difficulty, from Leo:  At the moment, no.  Long-term answer:  We'd like to.  There's a way to add these things in as data commitments that could be passed in.  We'd like to do this for testnet2.




