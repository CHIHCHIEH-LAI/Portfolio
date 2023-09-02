# Fault-Tolerant Key-Value Store

Welcome to the Fault-Tolerant Key-Value Store project! In this project, I've implemented a resilient key-value store with support for CRUD operations, load-balancing, and fault tolerance up to 2 failures.

## 🌟 Features

- **CRUD Operations**: The key-value store supports Create, Read, Update, and Delete operations.
- **Load-balancing**: Utilizes a consistent hashing ring to balance the load both for servers and keys.
- **Fault-tolerance**: To accommodate up to 2 failures, each key is replicated 3 times across 3 successive nodes in the ring.
- **Quorum Consistency**: Requires at least 2 replicas for both read and write operations to ensure data integrity.
- **Stabilization**: After a node failure, it ensures that 3 replicas are recreated to maintain the fault-tolerant nature.

## 📐 Architecture

The project is structured into three layers:

1. **EmulNet (network)**: The base layer simulating the network.
2. **P2P Layer**: Comprising of the `MP1Node` (membership protocol) and the `MP2Node` (key-value store). The key-value store gets its membership list from the membership protocol and uses it to maintain its virtual ring view.
3. **Application Layer**: Interacts with the P2P layer.

![image](https://github.com/CHIHCHIEH-LAI/Fault-Tolerant-Key-Value-Store/blob/main/imgs/Architecture.jpg)

Each node in the P2P layer is logically split into `MP1Node` and `MP2Node`. The key-value store communicates with the membership protocol to update its membership list and virtual ring.

## 🛠 Classes and Methods

- **HashTable**: A wrapper around C++11's `std::map`. It supports keys and values which are `std::string`.
- **Message**: Facilitates message passing between nodes.
- **Entry**: Used to store the value in the key-value store.
- **Node**: Wraps each node's Address and hash code.
- **MP2Node**: The heart of the project that implements the key-value store functionalities. The significant methods include:
  - `MP2Node::updateRing`
  - Client-side CRUD operations: `clientCreate`, `clientRead`, `clientUpdate`, `clientDelete`.
  - Server-side CRUD operations: `createKeyValue`, `readKey`, `updateKeyValue`, `deletekey`.
  - `MP2Node::stabilizationProtocol`

## 📝 Logging

Logging is an essential aspect of this project. For successful CRUD operations, use:
- `Log::logCreateSuccess`
- `Log::logReadSuccess`
- `Log::logUpdateSuccess`
- `Log::logDeleteSuccess`

For failed CRUD operations, use:
- `Log::logCreateFail`
- `Log::logReadFail`
- `Log::logUpdateFail`
- `Log::logDeleteFail`

Remember, both replicas and the coordinator should log messages, depending on the situation.

## 🚀 Testing

Test cases cover various scenarios from basic CRUD tests to handling multiple failures and system stabilization. To run tests, use:

```bash
bash ./KVStoreTester.sh
