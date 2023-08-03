# Gossip-Style Heartbeating Membership Protocol
This project implements a membership protocol that satisfies the following requirements:

1. Completeness all the time: Every non-faulty process must detect every node join, failure, and leave.
2. Accuracy of failure detection when there are no message losses and message delays are small. When there are message losses, completeness must be satisfied, and accuracy must be high. The protocol should handle simultaneous multiple failures. \n
   
The main logic for the membership protocol resides in the MP1Node files.

## The Three Layers
The implementation follows a three-layer protocol stack with the following layers (from top to bottom):

### Application
This layer drives the simulation and is responsible for starting up and crashing/stopping peers. It calls the nodeLoop() function in the P2P layer for each alive peer during each synchronous period (globaltime variable). The code for this layer can be found in the Application.cpp and Application.h files.

### P2P Layer
This layer is responsible for implementing the membership protocol. The main functionalities of this layer include:

- Introduction: Each new peer contacts a well-known peer (the introducer) to join the group. This is implemented using JOINREQ and JOINREP messages.
- Membership: Implementing the gossip-style heartbeating membership protocol that satisfies completeness all the time (for joins and failures) and accuracy when there are no message delays or losses (high accuracy when there are losses or delays).
The implementation for this layer is located in the MP1Node.cpp and MP1Node.h files.

### Emulated Network: EmulNet
The EmulNet layer emulates the network environment for the membership protocol. It provides functions that the membership protocol can use to send and receive messages. The provided functions include ENinit, ENsend, ENrecv, and ENcleanup.

The EmulNet implementation is available in the EmulNet.cpp and EmulNet.h files.

## Running the code
To run the code, follow these steps:

1. Run make to compile the code.
2. Execute ./Application testcases/<any_testcase> to start the simulation with the specified test case.
3. The run logs can be found in the dbg.log file.

## Grading the code
To grade the implementation, use the `./Grade.sh` script provided. It tests the membership protocol implementation in three scenarios and grades each of them based on three separate metrics. The scenarios are as follows:

1. Single node failure
2. Multiple node failure
3. Single node failure under a lossy network

The grader checks if all nodes joined the peer group correctly, if all nodes detected the failed node (completeness), and if the correct failed node was detected (accuracy). Configuration files representing each scenario can be found in the testcases folder.

## Future Optimization
To improve scalability, it is recommended to replace the all-to-all heartbeating with gossip heartbeating. This optimization can enhance the efficiency and performance of the membership protocol.

Note: The provided code structure is designed to facilitate easy debugging, performance measurement, and future conversion to run over a real network.