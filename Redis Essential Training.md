# Redis Essential Training

## Installing Redis

1. Install it from the [website](https://redis.io/docs/install/install-redis/).
2. Use `redis-server` to run the server.
3. Use `redis-cli` to run the redis command line interface.
    1. **Redis Server**: All command logic runs from here.
    2. **Redis Client**: Only useful to test commands on the server. You can have as many instances of this running as you want.
        - You can use only one command per line
        - The Client will give you hints on the command you are trying to use

## Basics of Redis

It is a key-value, in memory, noSQL database. It allows you to store hashmaps in memory without any database schema. 
- No Complex data models
- No hard drives
- No delays in data access
- Writing and reading is really fast as it all happens in the memory
- Use redis to store info which requires quick IO
- In-memory data can always be lost
- Usually used to store data to improve performance of your overall system and not as the main DB

### Data Structures

- Strings
- Lists
- Sets
- Hashes
- Sorted sets
- Bitmaps
- HyperLogLogs
- Streams
- Geospatial Indices

### Redis SET Command

- **Primary Use**: The SET Command in Redis is used to assign a value to a key.
- **Syntax**: `SET keyname value [other options]`
- **Key Components**:
  - `keyname`: Acts like a container or variable for the value.
  - `value`: The data that needs to be stored.
- **Data Type**: Primarily, the SET command handles string types.
- **Atomicity**: The SET Command is atomic, meaning it either completely succeeds or fails without partial execution.
- **Usage**:
  - To store simple strings: `SET name Fernando`
  - To store strings with spaces, use quotes: `SET city "Madrid, Spain"`
- **Key Creation**: If a key does not exist, the SET command will automatically create it.
- **Reliability**: If the SET command returns a successful response, the data is guaranteed to be stored in Redis.

### Redis GET Command

- **Purpose**: The GET command is used to retrieve data from Redis, serving as the counterpart to the SET command.
- **Syntax**: `GET keyname`
- **Functionality**:
  - Retrieves the value associated with the specified key.
  - Returns the string stored at the key, or `nil` if the key does not exist or is empty.
- **Key Names**:
  - Key names containing spaces must be enclosed in double quotes.
- **Usage Examples**:
  - To retrieve a stored name: `GET name` → returns the stored value (e.g., "Fernando").
  - To retrieve a value from a non-existent key: `GET my first name` → returns `nil`.
- **Key Characteristics**:
  - GET command is straightforward and returns the direct value stored in the key or `nil`.

### String Manipulation Commands

- **INCR and DECR**: 
  - Increment or decrement a string value treated as a number by one.
  - Syntax: `INCR keyname` or `DECR keyname`
  - Example: `INCR counter` increments the value of `counter`.

- **INCRBY and DECRBY**: 
  - Increment or decrement a string value by a specified amount.
  - Syntax: `INCRBY keyname amount` or `DECRBY keyname amount`
  - Example: `INCRBY counter 4` increases the value of `counter` by 4.

- **STRLEN**: 
  - Get the length of the string stored at a key.
  - Syntax: `STRLEN keyname`
  - Example: `STRLEN name` returns the length of the string in `name`.

- **APPEND**: 
  - Append a value to the string stored at a key.
  - Syntax: `APPEND keyname value`
  - Example: `APPEND name " Smith"` appends " Smith" to the current value of `name`.

- **GETRANGE**: 
  - Retrieve a substring from the string stored at a key, specified by a range of indices.
  - Syntax: `GETRANGE keyname start end`
  - Example: `GETRANGE name 0 3` retrieves the first four characters of the string in `name`.

### Practical Applications

- Use `INCR` and `DECR` for simple counter functionality.
- Use `INCRBY` and `DECRBY` for more controlled counter adjustments.
- `STRLEN` can be used to determine the length of data stored in a key.
- `APPEND` enables the concatenation of strings, useful for building longer strings or logs.
- `GETRANGE` can be used to extract specific portions of a string, beneficial for parsing or analyzing stored data.

## Hashes in Redis

- **Definition**: A HashMap, or hash, is a data structure that maps keys to values. In Redis, this concept is used to represent complex data structures.
- **Structure**: In a hash, each key (or field) is associated with a single value, allowing for the storage of multiple properties within a single entity.
- **Use Cases**:
  - Ideal for representing complex structures or objects like a user profile, a shopping cart, or any entity with multiple attributes.
- **Redis Optimization**:
  - Redis has optimized its hash data structure to efficiently store up to four billion properties in a single hash.
- **Applicability**:
  - Beyond typical uses like user sessions or shopping carts, Redis hashes are versatile and can be used for a wide range of data storage needs.

### Writing to Hashes

- **Command**: `HSET`
- **Usage**: To store data in a hash, where each field represents a property of an entity.
- **Example**: 
  - Store user data in a hash: `HSET logged_user:123 name "Fernando" avatar_url "http://example.com/avatar.jpg" role "admin"`
  - This command creates a hash named `logged_user:123` and sets multiple fields (name, avatar_url, role) with their respective values.

### Reading from Hashes

- **Single Field Retrieval**: `HGET`
  - Retrieves a single field value from a hash.
  - Example: `HGET logged_user:123 name` returns "Fernando".

- **Multiple Fields Retrieval**: `HMGET`
  - Retrieves multiple field values from a hash in a single command.
  - Syntax: `HMGET keyname field1 field2 ...`
  - Example: `HMGET logged_user:123 name avatar_url` returns the name and avatar URL of the user.

### Practical Insights

- Hashes are ideal for storing related data fields of an entity, like a user profile, in a structured way.
- `HSET` allows for setting multiple fields within a single hash, making it efficient for creating or updating data.
- `HGET` and `HMGET` provide flexible options for retrieving one or more fields from a hash, respectively.
- Redis hashes facilitate quick access and manipulation of structured data, supporting efficient data operations.

## Lists and Sets in Redis

### Storing Complex Data in Hashes

- **Scenario**: Storing a user's shopping cart in Redis.
- **Data Requirements**:
  - User ID for identifying the cart.
  - Product details such as ID, name, price, and quantity.

### Representing Complex Structures

- **Limitation**: Cannot store a list directly inside a hashmap.
- **Solution**: Utilize a naming convention that combines key, ID, and property for storing complex data.
  - Example: `HSET cart:123 prod1:name "Oranges" prod1:price 23 prod1:amount 1`

### Modifying and Querying Data

- **Incrementing a Field**: `HINCRBY`
  - Increments a field value in the hash by a specified amount.
  - Example: `HINCRBY cart:123 prod1:amount 3` adds three more oranges to the cart.

- **Retrieving All Fields**: `HGETALL`
  - Fetches all field-value pairs from the hash.
  - Example: `HGETALL cart:123` returns all details of the products in the cart.

- **Counting Fields**: `HLEN`
  - Returns the number of fields in the hash.
  - Example: `HLEN cart:123` might return 6, indicating two products with three fields each.

### Key Insights

- Hashes are versatile for representing and managing complex data structures.
- Utilizing naming conventions in keys allows for structured and accessible data storage.
- Commands like `HINCRBY`, `HGETALL`, and `HLEN` facilitate dynamic data manipulation and retrieval.
- Understanding and applying these techniques can significantly enhance data handling efficiency in Redis.

### Basics of Pop and Push

- **Pop Mechanism**:
  - Removing an element from a list.
  - Can be done from either the head (left) or the tail (right) of the list.

- **Push Mechanism**:
  - Adding an element to a list.
  - Can be performed on either the head (left) or the tail (right) of the list.

### Commands in Redis

- **Push Commands**:
  - `LPUSH`: Adds elements to the head (left) of the list.
  - `RPUSH`: Adds elements to the tail (right) of the list.
  - Example: `LPUSH MYLIST "First element" "Second element" "Final element"` adds elements to the head of `MYLIST`.

- **Pop Commands**:
  - `LPOP`: Removes an element from the head (left) of the list.
  - `RPOP`: Removes an element from the tail (right) of the list.
  - Example: `LPOP MYLIST` will remove and return the most recently added element from the head of `MYLIST` if `LPUSH` was used last.

### Understanding Order and Behavior

- The order of elements in the list is influenced by where (head or tail) and how (LPUSH or RPUSH) they are added.
- Popping elements (using LPOP or RPOP) modifies the list by permanently removing the element from it.
- Specifying a number after the list name in a pop command (e.g., `LPOP MYLIST 2`) will remove and return multiple elements at once.

### Adding People to the Queue in Redis

- Goal: Populate a queue with the names of people waiting for service.
- Approach: Add people to the queue both in groups and individually, depending on their arrival time.

### Implementation Steps

1. **Group Addition at Opening**:
   - Use `RPUSH` to add multiple people to the queue at once.
   - Example: `RPUSH thequeue "Carol Shaw" "Elizabeth Carr" "Ernest Ramos"` adds these three individuals to the queue as soon as the service starts.

2. **Individual Addition After Opening**:
   - Continue using `RPUSH` to add each person as they arrive.
   - Example: 
     - `RPUSH thequeue "Jane Carter"` adds Jane to the queue following the initial group.
     - `RPUSH thequeue "Martha Cooper"` adds Martha to the queue as the last person.

### Result

- The queue named `thequeue` is now populated with five people, in the order of their arrival or grouping at the service opening.
- This approach ensures that the queue maintains the correct order of arrival, adhering to the FIFO (First In, First Out) principle.

### Redis Commands Used

- `RPUSH thequeue "Person's Name"`: Adds a person to the end of the queue named `thequeue`.
- This method allows for both batch and individual additions to the queue, making it flexible for real-world scenarios.

### Queuing System Objectives

- **Actions**:
  - Add individuals to the queue.
  - Remove individuals from the queue.
  - View the current queue.
  - Allow priority access for premium customers.
  - Identify the last person in the queue.

### Data Representation

- **Elements**: Represented by strings, typically names or IDs that can be expanded into more detailed data externally.

### Queue Types and Operations

- **FIFO (First In, First Out)**:
  - Ideal for a standard queuing system where the order of arrival and departure is linear.
  - Use `RPUSH` to add people to the end of the queue.
  - Use `LPOP` to remove people from the front of the queue.

- **LIFO (Last In, First Out)**:
  - Not typical for fair queuing systems as the last person to arrive is served first.
  - Use `LPUSH` to add people to the front of the queue.
  - Use `LPOP` to remove people from the front of the queue.

### Practical Implementation

1. **Adding to the Queue**:
   - For a FIFO queue, `RPUSH queue_name "Person Name"` adds a person to the end of the queue.

2. **Removing from the Queue**:
   - For a FIFO queue, `LPOP queue_name` removes the person at the front of the queue.

3. **Premium Access Handling**:
   - Use `LPUSH queue_name "Premium Person Name"` to add a premium customer to the front of the queue in a FIFO system.

4. **Checking Queue Status**:
   - Use `LRANGE queue_name 0 -1` to view all people in the queue.
   - `RPOP queue_name` can be used to see who is last in a FIFO queue without removing them, if managed carefully.

### Process of Serving Clients

- Clients are served one at a time, reflecting the typical operation of a queue.
- The `LPOP` command is used to remove and return the name of the person at the front of the queue.

### Removing People from the Queue in Redis

- To serve the next person, execute: `LPOP queue_name`.
- Example: Executing `LPOP thequeue` would first return "Carol Shaw", followed by "Elizabeth Carr", "Ernest Ramos", "Jane Carter", and so on, in the order they were added.

### Queue Behavior

- After each `LPOP` operation, the next person in line is served, and their name is removed from the queue.
- Continually using `LPOP` will eventually empty the queue.
- Once the queue is empty, `LPOP thequeue` will return `nil`, indicating there are no more people to serve.

### Observations

- The queue maintains the order of arrival through the FIFO (First In, First Out) mechanism.
- The `LPOP` command effectively serves and manages the queue, ensuring an orderly process of attending to clients.
- An empty queue state is managed gracefully, returning `nil` when no individuals are left to serve.

### Viewing Queue Contents

- To check who is currently in the queue without removing anyone, the `LRANGE` command is used.
- This command allows you to view a specific range of elements in the list without altering it.

### Usage of LRANGE

- Syntax: `LRANGE queue_name start end`
- The parameters `start` and `end` define the range of elements to view, based on zero-indexed positions.
- For example, `LRANGE thequeue 0 2` returns the first three elements of the queue (`0` is the first element, `2` is the third).

### Understanding Ranges

- The range is zero-based, so the first element is at index `0`, the second at index `1`, and so on.
- Negative values are allowed and count from the end of the list backwards, with `-1` being the last element.
- To view the entire queue, you can use a command like `LRANGE thequeue 0 -1`, which will return all elements in the queue from start to end.

### Practical Example

- If the queue has elements like "Carol Shaw", "Elizabeth Carr", and "Ernest Ramos", executing `LRANGE thequeue 0 2` will show "Carol Shaw", "Elizabeth Carr", and "Ernest Ramos" without removing them from the queue.

### Key Points

- `LRANGE` is essential for monitoring and managing the queue as it provides visibility without impacting the queue's state.
- The command is flexible, allowing for partial or complete inspection of the queue based on specified range values.

### Scenario: Premium Customer Placement

- Aim: Add a premium customer, Patricia Fox, into the middle of the queue, specifically after Ernest Ramos.

### Using LINSERT Command

- `LINSERT` allows inserting a new element into the list relative to an existing element without replacing it.
- Operates based on values, not indices, enabling insertion before or after a specified element.

### Command Syntax and Usage

- Syntax: `LINSERT queue_name BEFORE|AFTER pivot value`
- Example: `LINSERT thequeue AFTER "Ernest Ramos" "Patricia Fox"` inserts Patricia Fox right after Ernest Ramos in the queue.

### Command Behavior

- Adds the new element (e.g., Patricia Fox) into the list without removing or replacing existing elements.
- Works on the first occurrence of the pivot value (e.g., Ernest Ramos).

### Result

- After execution, Patricia Fox is positioned immediately after Ernest Ramos in the queue.
- The command increases the list's length without disturbing the existing order of other elements.

### Key Considerations

- `LINSERT` is ideal for scenarios where position-relative insertion is required, such as adding priority customers to a specific spot in the queue.
- The command targets the first instance of the pivot value, which is crucial for lists with duplicate values.

### Practical Implications

- Enables flexible queue management by allowing mid-list insertions based on existing queue members.
- Facilitates the implementation of priority or tiered access systems within the queue.

### Inspecting Queue Elements with LRANGE

**Objective**: To view specific elements in the queue without altering it.

**Inspecting the Queue**:
- `LRANGE` command is used to view elements within the queue at specific positions.
- For example, `LRANGE queue_name 0 2` will return the first three elements because indexes are zero-based.

**Range Details**:
- Zero-based indexing: `0` refers to the first element, `2` is the third element.
- Negative values start counting from the end of the list, e.g., `-1` is the last element.

### Inserting Premium Customers with LINSERT

**Scenario**: Inserting a premium customer, Patricia Fox, into a specific position in the queue.

**Using LINSERT**:
- `LINSERT` command adds a new element relative to an existing element.
- Syntax: `LINSERT queue_name BEFORE|AFTER existing_element new_element`.
- Example: `LINSERT thequeue AFTER "Ernest Ramos" "Patricia Fox"` inserts Patricia after Ernest in the queue.

### Retrieving Edge Elements with LRANGE

**Objective**: To determine the first or last person in the queue without removing them.

**Retrieving First and Last Elements**:
- Use `LRANGE queue_name 0 0` to view the first element.
- Use `LRANGE queue_name -1 -1` to view the last element, utilizing negative indexes to start from the end of the list.

**Understanding Indexes**:
- Zero-based and inclusive: Specifying the same index twice (`0 0` or `-1 -1`) targets a single element.
- Negative indexes allow you to reference elements from the end of the list, providing flexibility in accessing list elements.

### Problems with Repeated Members in Lists

**Issue Identified**:
- The queuing system can accept duplicate entries for the same name, leading to potential inaccuracies in the queue.
- There is no built-in mechanism in lists to prevent the addition of duplicate content.

**Implications**:
- Requires manual checks before adding an entry to ensure it is not a duplicate.
- This characteristic of lists can conflict with business logic that requires unique entries.

### Sets as a Solution

**Introduction to Sets**:
- Sets in Redis are collections that prevent duplicate entries, making them suitable for maintaining unique element lists.
- Unlike lists, sets do not keep elements in a specific order.

**Differences Between Sets and Lists**:
- **Addition**: Use `SADD` to add elements to a set, automatically ignoring duplicates.
- **Ordering**: Sets do not maintain insertion order or any order.
- **Removal**: Use `SREM` to remove specific elements. The `POP` command in sets removes random elements, differing from list behavior.

### Queuing System with Sets

**Adapting to Sets**:
- Adding people to the set queue uses `SADD`, which ensures no duplicate entries.
- Viewing all members in the set uses `SMEMBERS`, showing all current elements without order.
- Removing specific individuals is done with `SREM`, allowing targeted removal.

**Limitations and Considerations**:
- The lack of order in sets means the concept of "last in the queue" is irrelevant.
- Sets allow for more flexible management, like serving any member without following the queue order.

## Sorted Sets

### Understanding Sorted Sets in Redis

**Basic Concept**:
- Sorted sets, like sets, do not allow duplicate entries.
- They maintain a sorted order of the elements, not based on the insertion order but on a specified score or value.

**Key Features**:
- **Order**: Elements in a sorted set are ordered by a score you assign upon insertion.
- **Data Type**: Allows storing pairs of elements: the string value and a sorting score.
- **Dynamic Sorting**: The set automatically re-sorts itself when the scores are updated.

### Usage in Applications

**Suitable For**:
- Priority queues, where elements are ordered based on their priority level.
- Leaderboards, ranking entries according to points or scores.
- Task scheduling systems, organizing tasks by their scheduled times or priority.

### Implementation Example: Leaderboard

**Leaderboard Characteristics**:
- A ranking system where players are sorted by their points.
- Higher points mean a higher position in the leaderboard.

**Why Sorted Sets**:
- Lists could store names but wouldn't handle points or automatic sorting.
- Sorted sets manage both the storing of points and the dynamic ordering based on these points.

**Operational Commands**:
- `ZADD`: Adds elements to the sorted set with a specified score. 
  - Example: `ZADD leaderboard 1000 "Sandra Ortiz"` adds Sandra Ortiz with 1000 points.
- The sorting is inherent, with no additional commands required for ordering.

**Advantages**:
- Sorted sets simplify the process of maintaining an ordered collection of elements, automatically handling reordering as scores change.
- Ideal for real-time ranking systems where the order of elements frequently updates.

## Key naming strategies

### Comparison Between Redis and Relational Databases

**Key Differences**:
- **Data Structure**: Redis is key-value based, not relational. It doesn’t use tables, rows, or relations like SQL databases.
- **Relationships**: Redis does not natively support relationships between records (no foreign keys or joins).
- **Data Access**: In Redis, data is accessed via single keys, not through complex queries combining multiple tables.

### Relating Records in Redis

**Limitations and Workarounds**:
- No inherent mechanism to relate records as in relational databases.
- Relationships can be simulated using naming conventions, like `collection_name_id` for keys, to logically group related data.

### Complex Data Models in Redis

**Handling Multi-Key Models**:
- Complex models require a more intricate naming scheme, e.g., `users_1_name`, `users_1_address`, to represent different properties of an entity.
- Relationships between entities (like users and addresses) are managed through related keys, not direct links as in relational databases.

### Efficiency and Flexibility

**Optimization**:
- Redis allows for highly optimized data access by direct key lookups.
- The flat, non-relational structure can be an advantage for specific use cases requiring high performance and simple data models.

**Design Considerations**:
- Requires careful planning of key naming and structure to represent complex and related data efficiently.
- Redis's model offers high-speed access but lacks the relational integrity and query versatility of SQL databases.

In summary, Redis provides a high-performance, key-value storage solution, ideal for scenarios requiring rapid access to simple or well-structured data. While it lacks the relational features of SQL databases, it compensates with speed and flexibility, necessitating creative approaches for representing complex or related data.

## Beyond Data Storage

1. **Message Bus**: Redis can function as a message bus, facilitating asynchronous communication between different systems. This setup allows data distribution between producers (entities creating data) and consumers (entities consuming the data).

2. **Pub/Sub (Publish/Subscribe) System**:
   - A fundamental messaging pattern in Redis where producers publish messages to channels, and consumers subscribe to these channels to receive messages.
   - Operates on a "fire-and-forget" mechanism, meaning messages are not stored and are lost if no subscriber is available at the time of message publication.

3. **Streams**:
   - A more complex and powerful data type in Redis, designed for handling a sequence of ordered messages similar to a log file.
   - Streams support appending new entries and allow consumers to read data in various ways, including by date range or in real-time.
   - Provides the capability to handle schema-less data entries, making it versatile for storing and querying complex datasets.

4. **Real-time Communication**:
   - Using the Pub/Sub model, Redis enables real-time communication between different components of an application.
   - Suitable for building real-time message buses where immediate notification and data propagation are required.

Redis is versatile, supporting use cases from simple key-value storage to complex, real-time message systems. Its fast in-memory processing capabilities make it suitable for tasks requiring quick data access and real-time updates, such as caching, session management, message queuing, and more.

### Introduction to Redis Keyspace Notifications

**Overview**:
- Keyspace Notifications in Redis allow subscribing to specific events related to keys, such as setting or expiring a value. This feature turns Redis into an event-driven system where it acts as the producer, emitting notifications on key events.

### Practical Use Case: Session Timeout Feature

**Scenario**:
- Implementing a session timeout feature in a web application using Redis.
- Workflow involves checking user session validity and automatically ending sessions after a specified time.

### Implementing Session Timeout

**Setup**:
- After user login, a key is set with an expiration time using the `SET` command with `EX` flag (e.g., `SET logged_in_user:1234 timestamp EX 3600`).
- Subsequent interactions refresh the session timeout using the `XX` flag to ensure the key exists.

### Keyspace Notifications for Session Management

**Mechanism**:
- When a key expires (e.g., session timeout), a keyspace notification is emitted.
- A separate service can subscribe to these expiration events to perform actions (like user logout) independently of the main application flow.

### Solution Breakdown

1. **Setting Session Keys**:
   - Use `SET` with `EX` for expiration and `XX` to update only existing keys.
   - This approach avoids extra checks for key existence, streamlining the session management process.

2. **Receiving Notifications**:
   - Subscribe to keyspace notifications for key expiration events to trigger additional actions or alerts.

3. **Advantages**:
   - Enables building reactive architectures with Redis as a central event emitter.
   - Optimizes session management by leveraging Redis's fast key-value operations and event notification system.
