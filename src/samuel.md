# Deployment of Samuel’s code
* This set of code was written by Samuel, a senior student of IITM.
* It's designed to clean up data about web requests, like extracting dates and simplifying URLs.
* It has functionalities for conducting anomaly detection.
   <p align="center"><strong>Dataset Used</strong></p>
* This is an open source dataset available in Kaggle of an Iranian ecommerce website zanbil.ir.
* Heading of the dataset are IP Address, User, Timestamp, Request, Status Code, Bytes Sent, Referrer, User Agent and an Unknown Field indicated mostly by “-”.
* This contains more than 1 crore samples.
   <p align="center"><strong>Creating and Writing Database</strong></p>
* Initial step involves setting up the development environment by installing Go and Julia programming languages, along with their corresponding packages: DataFrame for Julia and SQLite for Go.
* Following the setup, we proceed with data preprocessing tasks. Initially, we access the specified access log file and compile a regular expression to extract pertinent information from each log line.
* Subsequently, we establish a connection to an SQLite database, "kaggleDataset.db," utilizing the go-sqlite3 driver. Within this database, we commence a transaction to facilitate batch processing of SQL statements.
* Next, we ensure the existence of a table named "parsed_data" within the database schema. This table encompasses columns to store IP addresses, requests, user agents, timestamps, status codes, and bytes sent.
* We then prepare a SQL query designed to insert parsed data into the "parsed_data" table. This query is instrumental in reading each line of the access log file, parsing it using the previously compiled regular expression, and subsequently inserting the parsed data into the database.
* Finally, to persist the changes made to the database, we commit the transaction. This encapsulates the entirety of the data preprocessing pipeline, enabling efficient storage and manipulation of pertinent log data for subsequent analysis and insights.
   <p align="center"><strong>Data Processing Functions</strong></p>
We have four key functions designed for data processing:
1. appendDate!(logs): This function enhances the DataFrame by incorporating a new column that extracts and appends the date component from a datetime column. For instance, it converts a timestamp like "2023-05-22 15:30:00" to a date format, "2023-05-22".
2. normalizeRequests!(logs): Here, the focus is on standardizing the request data within the DataFrame. This might entail actions such as scaling numeric columns to a uniform range or converting categorical data into a consistent format. For example, it could ensure that all IP addresses are presented in a consistent case or format.
3. removeMPrefix!(logs): This function targets the removal of a specific prefix, 'm', from designated fields or entries in the DataFrame. By doing so, it aids in data cleanup, transforming entries like "m12345" to "12345".
4. removeRapidGrails(logs): This function filters out specific types of data entries or patterns deemed as noise or irrelevant. It's particularly useful for eliminating multiple rapid requests originating from the same IP address within a short timeframe, thus enhancing the overall quality of the dataset.
   <p align="center"><strong>Data Analysis</strong></p>
**Count_stats table**
* This table encapsulates IP address frequency data, presenting the occurrence count of each IP address and facilitates the computation of rankings for individual IP addresses based on their frequency of occurrence.
* The occurrence of unusually high request rates or discernible patterns in accessing specific URLs within this dataset may suggest the presence of bot activity.

**Transition table**
* This involves traversing each row (request) within the provided DataFrame ip_requests. 
* Subsequently, it constructs a weighted graph wherein each edge signifies a transition from one HTTP request to another, with the weight denoting the frequency of that transition.
* Two variables first_request and prev_req are initialized. first_request is set to true to indicate that the current request is the first one, and prev_req is set to nothing to indicate that there is no previous request yet.
* For the first request, it sets the ip and prev_req variables based on the IP address and request of the first row.
* For each subsequent request, it checks if it's the same as the previous request. If it is, it continues to the next iteration without updating the graph, as transitions from the same request to itself are not considered.
* If the current request is different from the previous one, it forms an edge from the previous request to the current one.
* The edges of this weighted graph are then sorted in descending order of their weights, utilizing Introsort, a hybrid sorting algorithm amalgamating quicksort, heapsort, and insertion sort.
* Upon sorting, the source node, destination node, and weight (frequency) of each edge are extracted from the sorted graph. 
* Subsequently, a DataFrame named transition_table is generated, housing the source, destination, and weight columns. 
* These columns encapsulate transitions occurring between HTTP requests, thereby providing a structured representation of the transition dynamics within the dataset.
* A dictionary named weighted_graph is initialized to store the weighted edges of the graph.

**Why is this created**
* This transforms raw web request logs into a structured format, which is pivotal in unveiling the navigation patterns of users as they interact with the website.
* Following the generation of the transition table, the subsequent step involves the creation of the 'anomaly_score_raw' table, which calculates an anomaly score for each IP address based on the count of rare transitions. 
* This is done to pinpoint IP addresses exhibiting a high incidence of uncommon transitions, potentially indicative of anomalous behavior.
* Subsequently, the 'anomaly_ratio' table is created to calculate the normalized anomaly score for each IP address by dividing the raw anomaly score by the total number of edges. 
* This is done to standardize the anomaly scores in relation to the overall activity observed, thereby furnishing a more discernible indicator of anomalous behavior adjusted for the individual activity levels of each IP address.
