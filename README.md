# Project Portfolio Collection

## Description
Welcome to my project showcase repository! This repository is a curated collection of my previous projects, where I aim to provide an overview of the projects without disclosing any code due to academic integrity guidelines or non-disclosure agreements.

As you browse through this showcase, I hope you find inspiration and insight into my problem-solving approach and how I leverage technology to create meaningful solutions. If you have any questions or would like to connect, feel free to reach out to me.

Thank you for visiting my project showcase, and I hope you enjoy exploring these projects as much as I enjoyed creating them!

For recruiters and interviewers interested in delving deeper into the technical aspects of my projects, I am more than happy to share the code and project details during interviews. 

## Project List
- Quantitative Research
  - Automated Alpha Research Process (Alpha Research, Python, API Calling)
- Distributed Systems
  - Gossip-Style Heartbeating Membership Protocol (C++, distributed systems, membership protocol)
  - All To All Heartbeating Membership Protocol (C++, distributed systems, membership protocol)
  - Fault-Tolerant Key-Value Store (C++, distributed systems, key-value stores, replication control, load-balancing mechanisms, consistent hashing ring)
- Web Development
  - Trip Plan Management App (Python, FastAPI, Redis Caching)
  - Publication Explorer App (Python, MySQL, MongoDB, Neo4j, Dash)
  - Employee Data Management API (Elixir, PostgreSQL)
  - NBA Player Narrative Visualization (D3js, HTML, JavaScript, CSS, front-end web)
- AWS Applications
  - US City Distance Calculator Chatbot (PyThon, AWS Lambda, AWS DynamoDB, AWS Lex, serverless computing)
  - AWS Storage Service with RDS, ElastiCache, and S3 (Python, AWS RDS, AWS ElasticCache for Redis, AWS S3)
  - Deep Learning Web Service (Python, Flask, Docker, DockerHub, AWS EKS)
- ML/DL Applications
  - CPU Performance Analysis and Prediction
  - Face-Aging Image Generation Model (Python, PyTorch, Numpy, generative adversarial network (GAN))
  - Logo Generation Model (Python, PyTorch, Numpy, generative adversarial network (GAN))
  - Mango Tier Classifier (Python, scikit-learn)

## Quantitative Research
### Automated Alpha Research Process
As a WorldQuant Research Consultant, I leverage WorldQuant simluation platform to build alphas. By leveraging WorldQuant simulation platform APIs, I develop automated alpha research process including alpha signal discovery, optimization and backtesting.

Key Skills: Alpha Research, Python, API Calling

## Distributed Systems
### Gossip-Style Heartbeating Membership Protocol
In distributed systems, nodes or members might join, fail and leave systems, and membership protocol is a mechanism used to keep track of the members that are currently part of the system or operational. And heartbeating is used for detecting whether a member is operational. Gossip heartbeating is highly scalable and fault-tolerant, making it suitable for large distributed systems. The "Gossip Heartbeating Membership Protocol" project project implements a membership protocol that ensures complete and accurate failure detection in a distributed system. The protocol achieves the following requirements:

1. Completeness all the time: Every non-faulty process detects every node join, failure, and leave without missing any updates.
2. Accuracy of failure detection: The protocol maintains high accuracy in failure detection even in the presence of message losses and small delays. It effectively handles simultaneous multiple failures.

The protocol implementation follows a three-layer protocol stack, containing application layer, P2P layer and emulated network layer. The application layer drives the simulation and is responsible for launching and crashing nodes. The emulated network layer emulates the network environment for the membership protocol, which provides functions that the membership protocol can use to send and receives messages. The P2P layer is responsible for implementing the main logic of the membership protocol. I implemented the gossip heartbeating membership protocol for the P2P layer in C++.

Key Skills: C++, distributed systems, membership protocol

### All To All Heartbeating Membership Protocol
In distributed systems, nodes or members might join, fail and leave systems, and membership protocol is a mechanism used to keep track of the members that are currently part of the system or operational. And heartbeating is used for detecting whether a member is operational. The "All-To-All Heartbeating Membership Protocol" project project implements a membership protocol that ensures complete and accurate failure detection in a distributed system. The protocol achieves the following requirements:

1. Completeness all the time: Every non-faulty process detects every node join, failure, and leave without missing any updates.
2. Accuracy of failure detection: The protocol maintains high accuracy in failure detection even in the presence of message losses and small delays. It effectively handles simultaneous multiple failures.

The protocol implementation follows a three-layer protocol stack, containing application layer, P2P layer and emulated network layer. The application layer drives the simulation and is responsible for launching and crashing nodes. The emulated network layer emulates the network environment for the membership protocol, which provides functions that the membership protocol can use to send and receives messages. The P2P layer is responsible for implementing the main logic of the membership protocol. I implemented the all-to-all heartbeating membership protocol for the P2P layer in C++.

Key Skills: C++, distributed systems, membership protocol

### Fault-Tolerant Key-Value Store 
This project is built upon the membership protocol project. Each node in the P2P layer is logically split into membership protocol part and key-value store part. The key-value store communicates with the membership protocol to update its membership list and virtual consistent hashing ring. I've implemented a resilient key-value store with support for CRUD operations (Create, Read, Update, Delete). The key-value store will also provide load-balancing using a consistent hashing ring to hash both servers and keys, and it will be fault-tolerant up to two failures.

Features
1. CRUD Operations: The key-value store will support Create, Read, Update, and Delete operations, allowing users to store and manage data efficiently.
2. Load-Balancing: Load-balancing will be achieved through a consistent hashing ring that efficiently distributes keys and servers, ensuring a balanced distribution of data across the system.
3. Fault-Tolerance: The system will be able to tolerate up to two failures. To achieve fault-tolerance, each key will be replicated three times and stored on three successive nodes in the ring, starting from the first node at or to the clockwise of the hashed key.
4. Stabilization After Failure: The system will be capable of stabilizing after a failure by recreating the necessary replicas to maintain fault-tolerance.

Key Skills: C++, distributed systems, key-value stores, replication control, load-balancing mechanisms, consistent hashing ring

## Web Development
### Trip Plan Management App
- I’m working on a trip plan management that will allow users to manage, download and share their travel plans.
- Until now, the application supports creating, deleting, updating, and retrieving travel plans and it has the ability to download trip details in ics file format. 
- I am working on the logic of allowing users to exchange travel plans.

Key Skills: Python, FastAPI, Redis Caching

### Publication Explorer App
The "Research Topic Explorer" is an interactive web application designed to assist users in discovering research directions and relevant publications. It provides valuable guidance to users, whether they are exploring new research areas or searching for specific papers. The app also offers the functionality to find faculty members with similar research interests, promoting collaboration within the academic community.

Features:

* Explore relevant publications based on specific keywords of interest.
* Determine research direction and identify potential areas for further exploration.
* Find faculty members with similar research interests.
* Interactive drop-down story structure for easy navigation.

Implementation:

* Utilizes Dash and other libraries to create declarative widgets and layouts.
* Follows a three-layer architecture: database, data access, and application layers.
* MySQL, MongoDB, and Neo4j servers store the data.
* Data access layer uses utils classes as interfaces to query and update databases.
* Application layer comprises app_widget, app_layout, and app, using Dash components like dcc.Input, dcc.Graph, dash_table.DataTable, and dcc.Dropdown.
* Callback functions update widgets with data from databases based on user inputs.

[DEMO](https://www.youtube.com/watch?v=dz1DilhuRcw)

Key Skills: Python, MySQL, MongoDB, Neo4j, Dash

### Employee Data Management API
The Payroll API is an Elixir Phoenix application that simplifies employee data management through a RESTful API. Key features include PostgreSQL integration, a user-friendly JSON-based interface, and the ability to create, retrieve, and delete employee records. 

[REPO](https://github.com/CHIHCHIEH-LAI/payroll_api)

Key skills: Elixir, PostgreSQL

### NBA Player Narrative Visualization
The "NBA Players Visualization" project aims to create an interactive narrative visualization for NBA player contracts, offering valuable insights into player salaries and ratings. The visualization empowers users to explore the average salaries of NBA first-round drafted players based on their positions and analyze the relationship between salary and 2K rating for different positions and countries.

[DEMO](https://chihchieh-lai.github.io/)

[REPO](https://github.com/CHIHCHIEH-LAI/NBA-Player-Narrative-Visualization-Drilldown-D3js)

Key Skills: D3js, HTML, JavaScript, CSS, front-end web

## Amazon Web Service (AWS)
### US City Distance Calculator Chatbot
![image](https://github.com/CHIHCHIEH-LAI/Portfolio/blob/main/images/AWS_Chatbot.jpg)

This project builds a chatbot that provides users with the shortest distance between two US cities in a directed graph with unit-weight edges.

Key Skills: PyThon, AWS Lambda, AWS DynamoDB, AWS Lex, serverless computing

### AWS Storage Service
![image](https://github.com/CHIHCHIEH-LAI/Portfolio/blob/main/images/AWS_storage.jpg)

The project builds a storage service with read and write APIs for a relational database. The architecture includes Amazon Aurora as the database, Amazon S3 for data storage, and a Redis cluster for caching. Lambda functions are used to implement the APIs.

Key Skills: Python, AWS RDS, AWS ElasticCache for Redis, AWS S3

### Deep Learning Web Service

This project sets up and manages infrastructure using Kubernetes and Docker containers to provide an online image classification service for the given project. The service will be exposed through a web server interface that receives HTTP requests and launches corresponding machine learning (ML) jobs using Kubernetes.

Key Skills: Python, Flask, Docker, DockerHub, AWS EKS

## ML/DL Applications
### Face-Aging Image Generation Model
The "Aging GAN" project trains a Generative Adversarial Network (GAN) to generate realistic aging faces. The GAN takes two inputs: a face image and the desired age (ranging from 20 to 70 years) and produces an output image of the face aged to the specified age.

[REPO](https://github.com/CHIHCHIEH-LAI/Aging-GAN-PyTorch)

Key Skills: Python, PyTorch, Numpy, generative adversarial network (GAN)

### Logo Generation Model
Trained a deep learning model to generate new logos

Key Skills: Python, PyTorch, Numpy, generative adversarial network (GAN)

### CPU Performance Analysis and Prediction
During my internship at Qualcomm, with the team designing the latest Snapdragon CPUs, I was assigned to create machine learning models to analyze and predict CPU performance.

### Mango Tier Classifier
Developed achine learning application to classify the quality of mangos

Key Skills: Python, scikit-learn
