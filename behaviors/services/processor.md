# Processor

Stateless execution engine for smart contract methods in an isolated environment (multiple languages supported). The processor retains no knowledge of the system around it. Every API it provides to smart contract developers is routed to the `VirtualMachine` controlling the execution process.

Multiple processors exist is various languages and runtimes like Ethereum Solidity, Go, Python and JavaScript. Every smart contract execution is always running in the memory context of a [deployed service](../../terminology.md).

Currently a single instance per virtual chain per node.

#### Interacts with services

* `VirtualMachine` - Uses it to perform smart contract SDK calls.

&nbsp;
## `Processors`

#### `Native`
* Smart contracts that are hard coded in code base and updated through node version updates.
* Implemented in native language of the node for efficiency and performance.
* Name space code is stored in a registry which is loaded on initialization and cached by the processor.

#### `Ethereum Solidity`
* Coming soon.

&nbsp;
## `Permissions`

#### Per contract
* Is it a system service:
  * `Service` - Standard permissions, eg. can only read and write state variables in its own address space.
  * `System` - A system contracts that runs under special permissions that give it more abilities.

#### Per method
* Who can call this method:
  * `Internal` - Caller must be the same service.
  * `External` - Caller can be anyone.
* Does it change state:
  * `ReadWrite` - Potentially changes state (thus needs to run under consensus).
  * `ReadOnly` - Does not change state (thus can run not under consensus).

&nbsp;
## `Smart Contract SDK`

* Every processor provides an SDK for [smart contract](../../terminology.md) developers.
* API that integrates with the system (like state access) is routed through `VirtualMachine`.
* API that doesn't integrate with the system (like a standard crypto operation) is implemented by the processor.
* See API [specification](../smart-contracts/sdk/api.md).

&nbsp;
## `ProcessCall` (method)

> Executes a method call under the context of a service and generates output or side effects (like transient state).

* Identify whether we're running under a system contract (original contract permission is `Service` or `System`).
* Identify the smart contract (service or library) whose method we need to call.
* Retrieve the method code and permissions, fail if not found.
* Verify that the method has appropriate permissions for execution:
  * `Internal` method can only be called within the same smart contract unless `System` permissions.
  * `ReadWrite` method can only be called with appropriate write context.
  * Fail if found a permission problem.
* Execute the code on the processor.
  * External SDK calls are performed by blocking execution and calling `VirtualMachine.SdkCall`.

&nbsp;
## `DeployNativeService` (method)

> Deploys a native service (smart contract). Methods of a new service cannot be called until it is deployed to the virtual chain. This method only exists on the native processor. Other processors deploy via transaction, not RPC.

#### Native smart contracts
* Make sure a native smart contract exists with the service name.
* Run system smart contract `_Deployments.Deploy` by calling the `Native` processor's `Processor.ProcessCall`.
  * See `_Deployments` contract [specification](../smart-contracts/system/_Deployments.md).


<!-- TODO: oded add the diagrams again

&nbsp;
![alt text][processor_interface] <br/><br/>

[processor_interface]: ../_img/processor_interface.png "VM - Processor Interface"

-->
