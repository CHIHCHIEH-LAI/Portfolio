# Project Portfolio Collection

## Description
Welcome to my project showcase repository! This repository is a curated collection of my previous projects, where I aim to provide an overview of the projects without disclosing any code due to academic integrity guidelines.

As a passionate and motivated CS graduate student with a keen interest in backend development and cloud computing, I have carefully selected and developed these projects to highlight my skills and experiences.

In this portfolio, you will find a variety of projects, each with its unique challenges and solutions. Each project has its own dedicated folder, complete with a detailed README.md that introduces the project, outlines the technologies used, and explains its key features and functionalities. Additionally, I have provided links to the live demos (where applicable) and the original project repositories (if public) for further exploration.

As you browse through this showcase, I hope you find inspiration and insight into my problem-solving approach and how I leverage technology to create meaningful solutions. If you have any questions or would like to connect, feel free to reach out to me.

Thank you for visiting my project showcase, and I hope you enjoy exploring these projects as much as I enjoyed creating them!

For recruiters and interviewees interested in delving deeper into the technical aspects of my projects, I am more than happy to share the code and project details during interviews. 

## Project List
- Distributed Systems Protocol Implementation
  - Fault-Tolerant Key-Value Store in C++
  - Gossip-Style Heartbeating Membership Protocol in C++
  - All To All Heartbeating Membership Protocol in C++
- Web Development
  - Publication Explorer App with Dash, MySQL, MongoDB and Neo4j
  - Employee Data Management RESTFul API with Elixir, PostgreSQL, and Phoenix
  - NBA Player Narrative Visualization with D3js, html
- AWS Applications
  - US City Distance Calculator Chatbot with AWS Lambda, Lex and DynamoDB
  - AWS Storage Service with RDS, ElastiCache, and S3
  - Deep Learning Web Service using AWS EKS and Docker
- AI Applications
  - Face-Aging Image Generation with PyTorch and generative adversarial network

## Distributed Systems
### Fault-Tolerant Key-Value Store
In this project, I've built a fault-tolerant key-value store that supports CRUD operations (Create, Read, Update, Delete). The key-value store will also provide load-balancing using a consistent hashing ring to hash both servers and keys, and it will be fault-tolerant up to two failures.

The project is built upon the foundation of a Membership Protocol, which I have implemented. The project handles failure detection and maintains a dynamic view of the membership in the distributed system.

Features
1. CRUD Operations: The key-value store will support Create, Read, Update, and Delete operations, allowing users to store and manage data efficiently.
2. Load-Balancing: Load-balancing will be achieved through a consistent hashing ring that efficiently distributes keys and servers, ensuring a balanced distribution of data across the system.
3. Fault-Tolerance: The system will be able to tolerate up to two failures. To achieve fault-tolerance, each key will be replicated three times and stored on three successive nodes in the ring, starting from the first node at or to the clockwise of the hashed key.
4. Quorum Consistency Level: Both read and write operations will follow a quorum consistency level, where at least two replicas are involved to ensure data consistency.
5. Stabilization After Failure: The system will be capable of stabilizing after a failure by recreating the necessary replicas to maintain fault-tolerance.

Key Skills: C++, key-value stores, replication control, load-balancing mechanisms, consistent hashing ring, CRUD operations

---

### Gossip-Style Heartbeating Membership Protocol
The "Gossip-Style Membership Protocol" project implements a membership protocol that ensures complete and accurate failure detection in a distributed system. The protocol achieves the following requirements:

1. Completeness all the time: Every non-faulty process detects every node join, failure, and leave without missing any updates.
2. Accuracy of failure detection: The protocol maintains high accuracy in failure detection even in the presence of message losses and small delays. It effectively handles simultaneous multiple failures.

The implementation follows a three-layer protocol stack: Application, P2P, and EmulNet layers. Grading using the ./Grade.sh script validates the protocol's performance in scenarios like single node failure, multiple node failures, and single node failure under a lossy network. The project showcases a robust and accurate gossip-style heartbeating membership protocol for distributed systems.

Key Skills: C++, gossip-style heartbeating membership protocol

---

### All To All Heartbeating Membership Protocol
The "All-To-All Heartbeating Membership Protocol" project project implements a membership protocol that ensures complete and accurate failure detection in a distributed system. The protocol achieves the following requirements:

1. Completeness all the time: Every non-faulty process detects every node join, failure, and leave without missing any updates.
2. Accuracy of failure detection: The protocol maintains high accuracy in failure detection even in the presence of message losses and small delays. It effectively handles simultaneous multiple failures.

The implementation follows a three-layer protocol stack: Application, P2P, and EmulNet layers. Grading using the ./Grade.sh script validates the protocol's performance in scenarios like single node failure, multiple node failures, and single node failure under a lossy network. The project showcases a robust and accurate all-to-all heartbeating membership protocol for distributed systems.

Key Skills: C++, all-to-all heartbeating membership protocol

---

## Web Development
### Publication Explorer App with MySQL, MongoDB and Neo4j
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

Key Skills: Python, MySQL, MongoDB, Neo4j, Dash, database, dashboard design, classic three-tier design

---

### Elixir, PostgreSQL, and Phoenix - Employee Data Management API
The Payroll API is an Elixir Phoenix application that simplifies employee data management through a RESTful API. Key features include PostgreSQL integration, a user-friendly JSON-based interface, and the ability to create, retrieve, and delete employee records. 

[REPO](https://github.com/CHIHCHIEH-LAI/payroll_api)

Key skills: Elixir, PostgreSQL, Phoenix, RESTFul API

### NBA Player Narrative Visualization
The "NBA Players Visualization" project aims to create an interactive narrative visualization for NBA player contracts, offering valuable insights into player salaries and ratings. The visualization empowers users to explore the average salaries of NBA first-round drafted players based on their positions and analyze the relationship between salary and 2K rating for different positions and countries.

[DEMO](https://chihchieh-lai.github.io/)

[REPO](https://github.com/CHIHCHIEH-LAI/NBA-Player-Narrative-Visualization-Drilldown-D3js)


Key Skills: D3js, HTML, JavaScript, CSS, narrative visualization, front-end web development

---

## Amazon Web Service (AWS)
### US City Distance Calculator Chatbot with AWS Lambda, Lex and DynamoDB

This project builds a chatbot using AWS Lex and Lambda for calculating the shortest distance between two cities in a directed graph with unit-weight edges.

Key Skills: PyThon, AWS Lambda, AWS DynamoDB, AWS Lex, serverless computing

---

### AWS Storage Service with RDS, ElastiCache, and S3

The project aims to demonstrate the performance benefits of using AWS RDS, ElastiCache, and S3 to build a storage service with read and write APIs for a relational database. The architecture includes Amazon Aurora as the database, Amazon S3 for data storage, and a Redis cluster for caching. Lambda functions are used to implement the APIs.

Key Skills: Python, AWS RDS, AWS ElasticCache, AWS S3, SQL, serverless computing, caching

---

### Deep Learning Web Service using AWS EKS and Docker

This project set up and manage infrastructure using Kubernetes and Docker containers to provide an online image classification service for the given project. The service will be exposed through a web server interface that receives HTTP requests and launches corresponding machine learning (ML) jobs using Kubernetes.

Key Skills: Python, Flask, Docker, DockerHub, AWS EKS, http

---

## Artificial Intelligence (AI/ML/DL)
### Face-Aging Image Generation with PyTorch and GANs
The "Aging GAN" project trains a Generative Adversarial Network (GAN) from scratch to generate realistic aging faces. The GAN takes two inputs: a face image and the desired age (ranging from 20 to 70 years) and produces an output image of the face aged to the specified age.

[REPO](https://github.com/CHIHCHIEH-LAI/Aging-GAN-PyTorch)


Key Skills: Python, PyTorch, Numpy, generative adversarial network (GAN), conditional GAN, vision transformer, deep learning

---
