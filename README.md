# Orbs Core Specification

V1 release

&nbsp;
## Terminology

Definitions of terms like `node`, `service`, `committee`. Available [here](terminology.md).

&nbsp;
## Services

* [Public Api](behaviors/services/public-api.md)

  Provides external public gateway interface for clients

* [Transaction Pool](behaviors/services/transaction-pool.md)

  Holds pending and committed transactions that are propagated between nodes

* [Gossip](behaviors/services/gossip.md)

  Connects different nodes over the network with efficient message broadcast and unicast

* [Consensus Algo](behaviors/services/consensus-algo.md)

  Pluggable consensus algorithm (multiple algorithms supported side by side)

* [Consensus Builder](behaviors/services/consensus-builder.md)

  Builds and validates the actual content of blocks for the consensus process

* [Virtual Machine](behaviors/services/virtual-machine.md)

  Executes service methods (smart contracts) using various processors

* [Processor](behaviors/services/processor.md)

  Executes a stream of transactions in an isolated environment (in a specific language)

* [Block Storage](behaviors/services/block-storage.md)

  Holds the long term journal of all confirmed blocks (the actual chain of blocks)

* [State Storage](behaviors/services/state-storage.md)

  Holds the latest state under consensus meaning all of the state variables for all deployed services in a virtual chain.

* [Sidechain Connector](behaviors/services/sidechain-connector.md)

&nbsp;
## High level flows

* [System Init](behaviors/flows/system-init.md)

  Node and all services init (fresh start or after a restart)

* [Call Method](behaviors/flows/call-method.md)

  Client calls a read-only method of a service (not under consensus)

* [Send Transaction](behaviors/flows/send-transaction.md)

  Client sends a transaction that may write to state of a service (under consensus)

* [Transaction Status](behaviors/flows/transaction-status.md)

  Client queries regarding the status and receipt of an old transaction

* [Block Creation](behaviors/flows/block-creation.md)

  The main continuous flow of consensus where blocks are created from pooled transactions

* [Inter Node Sync](behaviors/flows/inter-node-sync.md)

  Block synchronization between nodes

* [Intra Node Sync](behaviors/flows/intra-node-sync.md)

  Block synchronization between services inside a node
