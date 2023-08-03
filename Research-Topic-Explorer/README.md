# research-topic-explorer
## Overview
![image](https://github.com/CHIHCHIEH-LAI/Project-Portfolio-Collection/blob/main/Research-Topic-Explorer/imgs/Overall_Architecture.jpg)
## Basic Information
* Title: Professor Chang! What could be my thesis topic?
* Purpose
  * Application Scenario: The primary goal of this app is to aid individuals in search of research directions or relevant publications. Regardless of whether users are in the initial stages of identifying their research focus or searching for relevant papers, this app offers valuable guidance and support. Furthermore, the app features a function that enables users to find faculty members with similar research interests, providing an additional level of assistance and connectivity.

  * Target Users: This app caters to a diverse user base, including researchers, professors, and anyone interested in identifying their research direction or accessing relevant publications. Although our target audience is broad, we specifically designed the app to assist faculty members in identifying colleagues with similar research interests. Initially, we plan to limit this feature to within the university but extending it to other institutions would be a straightforward process.
  * Objectives: The primary objective of the app is to enable users to explore relevant publications based on their specific keywords of interest. By providing this functionality, the app aims to assist users in determining their research direction and identifying potential areas for further exploration.
                One of the main objective that we try to solve is to find the faculties with similar research interest. For this we plan to extend the current schema by doing some data cleaning and machine learning tools. 

* [Demo Link](https://youtu.be/dz1DilhuRcw)

## Installation
* Install Database Engine
  * MySQL: follow the instructions [here](https://dev.to/gsudarshan/how-to-install-mysql-and-workbench-on-ubuntu-20-04-localhost-5828)
  * MongoDB: follow the instructions [here](https://www.mongodb.com/docs/manual/administration/install-community/)
  * Neo4j: Go to [Neo4j Website](https://neo4j.com/download-neo4j-now), fill in the information box, and then click Download Neo4j Now, this will jump to the download page. (choose .AppImage for linux version)
  
  For more details, please check [here](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/materials/Setting%20Up%20Database%20Environments.pdf)
  
* Install Required Python Packages By [setup.sh](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/setup.sh)
  ```
  ./setup.sh
  ```
  
* Start the database engines

* Load Extra Data into MySQL Database
  1. Log in to mysql shell:
  ```
  $ mysql -u root -p
  Enter password: *******
  ```
  2. Import [database_update.sql](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/src/database_update.sql) file into the database:
  ```
  > source [file/to/database_update.sql]
  ```
  3. Exit from mysql shell:
  ```
  exit
  ```
  4. Run [cluster_research_topic_after_cleaning.py](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/src/cluster_research_topic_after_cleaning.py)
  ```
  cd src
  python3 cluster_research_topic_after_cleaning.py 
  ```
  
* Add indexing for MongoDB collection
  1. Log in to mongo shell:
  ```
  mongo
  ```
  2. Enter in the database
  ```
  use academicworld;
  ```
  3. add indexing
  ```
  db.publication.createIndex({ ‘keywords.name’: 1 })
  ```
  
* Setup database [configuration file](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/src/DBconfig.json) 
  
* Run the app
  ```
  python3 app.py
  ```

## Usage
1. Input interested word to search for related keywords
![image](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/imgs/related_keyword.jpg)
Once users input their topic of interest, the app generates a bar chart displaying related keywords sorted by the number of associated publications. This feature allows users to easily identify and explore relevant keywords that capture their attention, providing them with a starting point for their research direction.

2. Input interested keyword to see its trend and associated publications
![image](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/imgs/keyword_trend_publication.jpg)
After selecting a keyword of interest, users can input it into the designated section within the app. The app generates a chart displaying the frequency of publications related to that keyword over time. This feature enables users to observe trends in the popularity of their topic of interest and determine whether it is gaining or losing traction in the field. In addition to displaying the frequency of publications over time, the app provides users with a table of associated publications, including the publication ID, title, year of publication, and number of citations. This feature allows users to delve deeper into the research surrounding their topic of interest, gaining a comprehensive understanding of the current state of knowledge in the field.

3. Input interested publication to search for more similar publication
![image](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/imgs/similar_publication.jpg)
If users come across an interesting publication while using the app, they can input its title into the search function to find more related publications. The app generates a table of similar publications, including relevant keywords, publication title, year of publication, and number of citations. This feature allows users to further expand their research and stay up-to-date with the latest developments in their field of interest.

4. Choose university to find faculty with the same interest
![image](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/imgs/group_faculty.jpg)
The app not only helps users find suitable publications, but also recommends potential collaborators. Users can select a university from a dropdown menu, and the app will display several faculty clusters with similar research interests and publications in that university. This feature facilitates collaboration between researchers with similar interests and expertise, potentially leading to more impactful research outcomes.

5. Add new publication
![image](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/imgs/update_widgets.jpg)
The world of academia is constantly evolving, with new publications being released on a regular basis. To ensure that the app stays up-to-date, users have the option to add new publications and their associated keyword scores to the app's database. To do so, users simply input the publication information and publication keyword score, and then press the submit button to add the new data to the database. In the event that users accidentally add incorrect data, they can easily rectify their mistake by pressing the reset button to remove all added data.

## Design
![image](https://github.com/CHIHCHIEH-LAI/research-topic-explorer/blob/main/imgs/Overall_Architecture.jpg) \
The architecture of the application is divided into three layers, namely database, data access, and application layers. In the database layer, we have MySQL, MongoDB, and Neo4j servers where data is stored. The data access layer consists of mysql_utils, mongodb_utils, and neo4j_utils that serve as interfaces to the databases, allowing the application layer to query and update the databases. The application layer consists of app_widget, app_layout, and app. The app runs the application and displays the widgets mentioned above, while receiving the HTML layout from app_layout. The app_widget uses callback functions to update widget figures in html layout and tables whenever an input component's property changes. During the update process, the callback functions access databases through the data access layer.

## Implementation
* Dashboard specification \
  The app, app_widget, and app_layout use Dash and other libraries like Pandas to create widgets and their layout declaratively. The application layout is defined in the build_app_layout function in app_layout.py. This function returns an HTML div that contains various widgets, such as input fields, dropdown menus, and graphs. The widgets are created using Dash components such as dcc.Input, dcc.Graph, dash_table.DataTable, and dcc.Dropdown, and each widget has a unique ID that can be referenced in callbacks.

  Callbacks update the widgets and are defined using the @app.callback decorator. Each callback function takes one or more inputs, which typically are the values of the input fields or dropdown menus, and returns one or more outputs, which typically are the properties of the widgets that need to be updated. These callback functions are defined in the app_widgets.py file as classes. The widgets query the databases to retrieve results based on user input, and the callback functions update the figures, tables, or text messages accordingly.

  All the widgets follow the same pattern of getting user input, accessing the database, and returning figures or text to update the widget, even if their inputs and outputs differ.
  | widget id 	| widget name                                 	| widget type 	| input        	| output       	| database 	|
  |-----------	|---------------------------------------------	|-------------	|--------------	|--------------	|----------	|
  | 1         	| related keyword widget                      	| querying    	| dcc.Input    	| bar chart    	| MySQL    	|
  | 2         	| keyword trend widget                        	| querying    	| dcc.Input    	| line chart   	| MongoDB  	|
  | 3         	| keyword related publication widget          	| querying    	| dcc.Input    	| table        	| MySQL    	|
  | 4         	| similar publication widget                  	| querying    	| dcc.Input    	| table        	| Neo4j    	|
  | 7         	| faculty cluster widget                      	| querying    	| dcc.Dropdown 	| scatter plot 	| MySQL    	|
  | 5.1       	| add publication message widget              	| querying    	| dcc.Button   	| html.Div     	| MySQL    	|
  | 5.2       	| add publication table widget                	| updating    	| dcc.Button   	| table        	| MySQL    	|
  | 5.3       	| remove all added publication widget         	| updating    	| dcc.Button   	| table        	| MySQL    	|
  | 6.1       	| add publication_keyword message widget      	| querying    	| dcc.Button   	| html.Div     	| MySQL    	|
  | 6.2       	| add publication_keyword table widget        	| updating    	| dcc.Button   	| table        	| MySQL    	|
  | 6.3       	| remove all added publication_keyword widget 	| updating    	| dcc.Button   	| table        	| MySQl    	|
  
* Database access
  * MySQL \
    mysql_utils implements a database connection and query gateway in Python using the mysql-connector library. The code is organized into three general classes: DBConnection, QueryGateway, and many Interfaces.

    The DBConnection class handles the connection to the MySQL database and the execution of queries. The QueryGateway class contains a set of predefined SQL statements that can be executed using the execute_statement() method. The Interface class serves as an interface between the query gateway and the widget, providing methods for retrieving data and updating data from the database: `get_similar_keyword()`, `get_keyword_related_paper()` and many others.
    
  * MongoDB \
    mongodb_utils implements a MongoDB database connection and a query interface in Python. The MongoDBConnection class is used to establish a connection to the MongoDB database and execute queries using the pymongo library. The QueryInterface class is used to define specific queries to the database and returns the results.

    The pymongo library is used to establish a connection to the MongoDB database, and the MongoClient class is used to connect to the database. The execute_query method in the MongoDBConnection class takes the name of the collection and a MongoDB aggregation pipeline as input parameters and returns the query result.

    The QueryInterface class takes a MongoDBConnection instance as an input parameter, and the get_keyword_trend method returns the trend of a keyword in publications over a period. It executes a MongoDB aggregation pipeline and returns the result.
  
  * Neo4j \
    neo4j_utils implements a database connection and query in Python using the neo4j library. The code is organized into a single class Neo4jDBConnection.

    The Neo4jDBConnection class handles the connection to the Neo4j database and the execution of a single Cypher query to retrieve similar publications based on a given title. The get_similar_publication() method takes a title parameter, constructs a Cypher query with it, and executes the query using the run() method of the Session object.

    The neo4j library is used to interact with the Neo4j database, and the GraphDatabase.driver method is used to establish a connection to the database. The Cypher query is constructed as a string and executed using the run() method of the Session object returned by GraphDatabase.driver.
    
* Databases \
We have already created, in our MPs, the databases MySQL, MongoDB, and Neo4j with the Academic World dataset. These databases will be accessed through their network interfaces.

## Database Techniques
* Indexing \
  Keyword Trend Widget: Due to the use of MongoDB databases in the keyword trend widget, queries can take longer than when using a MySQL database. To address this issue, we have implemented indexing to speed up queries and improve the overall performance of the widget. 
  ```
  db.publication.createIndex({ ‘keywords.name’: 1 })
  ```

* Constraint \
  Group Faculty By Interest Widget: We have created a new table containing ID and cluster information. Users can reference the ID within the new table to access more detailed information about faculty within the faculty table. To ensure data integrity and prevent errors, we have implemented a foreign key constraint to ensure that the ID within the new table is always present within the faculty table. 
  ```
  ALTER TABLE faculty_group_by_interest ADD CONSTRAINT FOREIGN KEY (id) REFERENCES faculty(id);
  ``` 
  in src/database_update.sql

* Parallel query execution \
  Keyword Trend Widget: By utilizing asynchronous programming, we can take advantage of non-blocking I/O operations and significantly improve the performance of our application. This means that the application can continue performing other tasks while waiting for database queries to complete, allowing for parallel processing of multiple queries. With this approach, the application can leverage available hardware resources to reduce the time required to complete each query. To implement this functionality for MongoDB querying, we utilized asyncio.
Please refer to 161th - 163th lines of code in src/app.py and 60th - 80th lines of code in src/app_widgets.py

* Prepared statement \
  Initially we were using pymsql but later we realized that pymysql doesn't have client-side prepared statement therefore we switched back to mysql-connector library.
  MySQL Connector's prepared statement is a feature that allows for more efficient and secure execution of SQL queries. A prepared statement is a precompiled SQL statement that is executed multiple times with different parameter values. It has two stages: preparation and execution. In the preparation stage, the SQL statement is parsed, analyzed, and optimized by the database server, and a query execution plan is generated. In the execution stage, the prepared statement is executed with the supplied parameter values, which are substituted into the query plan, reducing the overhead of parsing and optimizing the SQL statement for each execution.

  Prepared statements are particularly useful in preventing SQL injection attacks, as they allow for parameterized queries that separate SQL code and user input, thereby preventing malicious input from affecting the SQL statement's syntax. This feature significantly improves the security of an application, making it a recommended practice for developers using MySQL Connector.


## Extra-Credit Capabilities
Our objective was to create a practical tool that faculty members could utilize. Among the implemented ideas, we developed a collaborative tool for faculty members within the same university. Although we intended to extend this tool to other universities, time constraints prevented us from doing so. To bring this idea to fruition, we completed the following tasks.
* Data expansion :- We extended existing schema with a table that groups faculties with similar research interests. For building a cluster used KMEANS clustering.
* Data Cleaning:- Initially, when we applied the clustering code on the existing research interest data, we were not satisfied with the resulting clusters. To improve the clustering outcome, we dedicated time to analyze the insignificant words in the research interests and proceeded to clean them before utilizing the clustering algorithm. By taking this extra step, we were able to enhance the accuracy and effectiveness of the clustering process.
* Machine Learning :- Incorporating the concepts we acquired from the Text Information Systems course, namely the Term Frequency-Inverse Document Frequency (TFIDF) and K-Means clustering, we successfully clustered faculty members with similar research interests. The results of our analysis were nothing short of fascinating, as they were confirmed both through manual analysis and quantitative results, which were highly comparable. Despite recognizing that there is always room for further improvements, we were forced to conclude the project at this point due to the constraints of time.

## Contributions 
* Sudhendu Sharma (Roughly around about 30 hours)
   * Implemented basic framework for Data Access Layer and proposed modular code base design
     * Implemented mysql_utils.py, mongodb_utils.py, and neo4j_utils.py and integrated with database.
   * Code Refactoring to accommodate various changes
   * Implemented asynchronous framework using asyncio.
   * Extension of existing database for extra credit capability 
   * Data Cleaning 
   * Using Machine Learning technique to group similar faculties
   * Code and Query Optimization
   * Wrote stored procedure but could not integrate with use case due to time constraints.
   * Implementing prepared statements

* Chih-Chieh Lai (Roughly 50~60 hours)
  * Demo Video Recording
  * Wrote README.md
    * Overview
    * Basic Information
    * Installation
    * Usage
    * Design
    * Implementation
    * Database Techniques
  * src/app_layout.py
    * Defined and created the frontend layout
  * src/app_widgets.py
    * Created 7 widget classes to handle callback functions and implemented the logic for accessing the data access layer
  * src/app.py
    * Implemented argparse to enable command-line arguments and allowed the program to read a JSON configuration file
    * Instantiated objects from the classes defined in app_widgets.py
  * src/widget_interface.py
    * Created several widget classess to exploit polymorphism.
  * src/mysql_utils.py
    * PublicationKeywordInterface class, functions and statements
    * PublicationInterface class, functions and statements
  * src/mongodb_utils.py
    *  get_keyword_trend function
  * src/neo4j_utils.py
    *  get_similar_publication function
