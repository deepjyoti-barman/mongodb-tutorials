# MongoDB Notebook

## Introduction to MongoDB

- MongoDB is a database more specifically a NoSQL database. MongoDB is also the name of company who built it.
- MongoDB, the name came from the word 'Humongous' because this database is built to store lots and lots of data. You can store a lot of data and then you can also work with them efficiently.
- The following terminologies differs in NoSQL databases like MongoDB from SQL databases like Oracle, MySQL and Postgres:
  - SQL: Tables, NoSQL: Collections
  - SQL: Records, NoSQL: Documents
- It uses JSON (BSON) notation to store data as documents inside collections in MongoDB. Behind the scenes on the server, MongoDB converts our JSON data to a binary version of it which can basically be stored and queried more efficiently so some people call it BSON instead of JSON.
- Unlike SQL, in NoSQL collections are schema-less and this is the flexibility (flexibility to grow with your application and its needs) NoSQL databases like MongoDB gives you where SQL databases are very strict about the data you have to store in there.
- Another advantage of MongoDB is can store nested data as it uses JSON. This allows it to create complex relations between data and store them in one and the same document - which allows working and fetching data from it efficiently and also allows to store data in a logical way. In SQL databases we need to write complex joins and sub-queries to fetch data from different tables.

## Characteristics of MongoDB

- It is a NoSQL database and it has no schema. Instead of normalizing data which means storing it, distribute it across multiple tables where every table has a clear schema and then using a lot of relations, MongoDB goes for storing data together in a document and it also does not enforce a schema for you i.e. if we have multiple documents in a single collection they can have different structures. So as an advantage you can use MongoDB for apps that might still evolve, where the exact data requirements are just not set yet, you can get started and always add data with more information in the same collection at a later point of time.
- It has no / few relations i.e. less collections and within that you store data together. So instead of merging one table with another, MongoDB can query and get the results more efficiently.
- Popular use-cases for MongoDB is read/write heavy applications, applications with a lot of workload, applications that store a lot of data (i.e. smart devices which send some sensor data every second), building an online shop or blog.

## MongoDB (company) Ecosystem

- MongoDB database
  - Enterprise Server and Community Server
  - CloudManager / OpsManager (Tools for system admin or database admin to manage database)
  - Atlas (Cloud solution, all the managing a system admin would have to do is done for us there, so that we can focus on our data and logic there)
  - Mobile (One can install MongoDB on a mobile device, store data there and work without an internet connection)
  - Compass (GUI tool to connect to database and have a look at the data from there)
  - BI Connectors (Data science tool)
  - MongoDB Charts (Data science tool)
- Stitch (Serverless backend solution)
  - Serverless Query API (A tool which we can use ot efficiently query our database directly from inside our client side app - react/angular app)
  - Serverless Functions (Allows us to execute code in the cloud on demand, equivalent to AWS Lambda and Google Cloud functions)
  - Database Triggers (Service that allows us to listen to events in a database. Like if the document gets inserted, then execute a function to send an email to the customer in response to that)
  - Real-time Sync (Built to synchronize a database in a cloud with that mobile offline supporting database)

## Command to Install MongoDB in Mac

- Use the 'tap' command to download the official Homebrew formula for MongoDB and the Database Tools:  
`brew tap mongodb/brew`
- Use the following command in your macOS terminal to install MongoDB:  
`brew install mongodb-community`
- Change default database and log path for MongoDB:  
`vim /usr/local/etc/mongod.conf`
- Automatically start MongoDB server at login:  
`brew services start mongodb/brew/mongodb-community`
- Stop automatically startup of MongoDB server at login:  
`brew services stop mongodb/brew/mongodb-community`
- Command to manually run MongoDB server with the default database and log path:  
`mongod --config /usr/local/etc/mongod.conf`
- Command to manually run MongoDB server with custom database and log path:  
`mkdir -p ~/development/mongodb/data ~/development/mongodb/logs`  
`mongod --dbpath ~/development/mongodb/data --logpath ~/development/mongodb/logs/mongo.log`
- Command to connect to MongoDB server:  
`mongosh` (Note: Ensure that MongoDB server is running before attempting to start the mongosh shell)

## MongoDB Driver Downloads

- Drivers are packages we install for our app which may be written in the different programming languages and these drivers are then work as a bridge between your programming language and the MongoDB server.
- <https://docs.mongodb.com/drivers/>

## Basic MongoDB queries

- Lists all the existing databases that are currently running on the database server (when started fresh it will show 3 databases which will store some metadata):  
`> show dbs`

    ```log
    admin     41 kB
    config  61.4 kB
    local   73.7 kB
    ```

- To display the database you are using (operation may return test, which is the default database):  
`> db`

    ```log
    test
    ```

- We can switch to a database with the 'use' command and we can even switch to non-existing databases. But, in the second case the database won't be created before we start entering data in there. Once we do start entering data, such as by creating a collection, MongoDB will automatically and implicitly create the database so that we don't have to create the database in advance. To switch to a database:  
`> use shop`

    ```log
    switched to db shop
    ```

- Create a collection on the fly and insert a document in it:  
`> db.products.insertOne({ name: "A Book", price: 12.99 })`  
`> db.products.insertOne({ name: "A T-shirt", price: 29.99, description: "A new branded T-shirt" })`  
`> db.products.insertOne({ name: "A Computer", price: 1299.99, description: "A latest high quality gaming computer", details: { cpu: "Intel i7 8770", memory: 32 }} )`

    ```json
    {
        acknowledged: true,
        insertedId: ObjectId("61a2d3573020a42e0968c3e9")
    }
    ```

  __Note__:
  - db refers to the database currently connected to, in this case its 'shop'.
  - You can omit the quotation marks around the key name, not around the value because value is a text and quotation mark should be there around the text.
  - The output shows that the data was inserted into the database and also shows automatically generated the unique ID by MongoDB for it.

- List all the data in the collection  
`> db.products.find()`

    ```json
    [
        {
            _id: ObjectId("61a2d3573020a42e0968c3e9"),
            name: 'A Book',
            price: 12.99
        },
        {
            _id: ObjectId("61a2d6173020a42e0968c3ea"),
            name: 'A T-shirt',
            price: 29.99,
            description: 'A new branded T-shirt'
        },
        {
            _id: ObjectId("61a2d6b73020a42e0968c3eb"),
            name: 'A Computer',
            price: 1299.99,
            description: 'A latest high quality gaming computer',
            details: { cpu: 'Intel i7 8770', memory: 32 }
        }
    ]
    ```

## A Big Picture of MongoDB Working with the Clients

![Working With MongoDB](Diagrams/working-with-mongodb.png)
![A Closer Look to Storage Engine](Diagrams/a-closer-look-to-storage-engine.png)

__Note__: We always talk to the MongoDB server and behind that server, the server talks to the Storage Engine which manages our data and stores in in files in the end but also in memory in between so that we can work with the data in a very fast way,

## Understanding the Basics of MongoDB

### Spinning up the MongoDB server

- In MongoDB we have one or more databases on our database server. Each database can hold one or more collections. A collection would be a table in a SQL database. In each collection we have multiple documents, documents are the data piece in our database.
- Databases, collections and the documents are all automatically created for us implicitly when we start working with them, when we start storing data. We can also explicitly create collections which allows us to configure them a bit further.
- The default port for MongoDB server is 27017. If you have some other application using that port you can run the MongoDB server to another port using the following command:  
`mongod --port <port-no>`

### Connecting to the MongoDB Server via the mongo / mongosh Shell

- Local MongoDB Instance on Default Port:
  - You can run mongo shell without any command-line options to connect to a MongoDB instance running on your localhost with default port 27017.
  - Command: `mongo`

- Local MongoDB Instance on a Non-default Port:
  - To explicitly specify the port, include the --port command-line option. For example, to connect to a MongoDB instance running on localhost with a non-default port 28015.
  - Command: `mongo --port 28015`

- MongoDB Instance on a Remote Host:
  - (To explicitly specify the hostname and/or port) You can specify a connection string. For example, to connect to a MongoDB instance running on a remote host machine.
  - Command: `mongo "mongodb://mongodb0.example.com:28015"`
  - You can use the command-line option --host <host>:<port>. For example, to connect to a MongoDB instance running on a remote host machine.
  - Command: `mongo --host mongodb0.example.com:28015`
  - You can use the --host <host> and --port <port> command-line options. For example, to connect to a MongoDB instance running on a remote host machine.
  - Command: `mongo --host mongodb0.example.com --port 28015`

- MongoDB Instance with Authentication:
  - You can specify the username, authentication database, and optionally the password in the connection string. For example, to connect and authenticate to a remote MongoDB instance as user alice.
  - Command: `mongo "mongodb://alice@mongodb0.examples.com:28015/?authSource=admin"`
  - NOTE: If you do not specify the password in the connection string, the shell will prompt for the password.
  - You can use the --username <user> and --password, --authenticationDatabase <db> command-line options. For example, to connect and authenticate to a remote MongoDB instance as user alice.
  - Command: `mongo --username alice --password --authenticationDatabase admin --host mongodb0.examples.com --port 28015`
  - NOTE: If you specify --password without the user’s password, the shell will prompt for the password.

### The mongo / mongosh Shell

- The shell allows us to write queries which are very similar to the queries in the different drivers available for different programming languages (we use drivers to interact and perform transactions with the MongoDB server).
- The shell is based on JavaScript (queries are similar to Node.js driver).

## Basics of CRUD Operations

### Recap of Create / Insert Operation

- Insert one document in a collection:  
`> use flights`  
`> db.flightData.insertOne({ departureAirport: "MUC", arrivalAirport: "SFO", aircraft: "Airbus A380", distance: 12000, intercontinental: true })`
`> db.flightData.insertOne({ departureAirport: "TXL", arrivalAirport: "LHR" })`

  ```json
    {
        acknowledged: true,
        insertedId: ObjectId("61a402d6ee6d4171445ef746")
    }
  ```

  __Note__:
  - Data types in JSON notation can be:
    - string
    - number
    - boolean
    - array
    - object (JSON object)
    - null
  - Apart from the above data type MongoDB also supports some additional data types.
  - Every document we insert gets a unique ID assigned by MongoDB automatically.
  - The unique id can be found as a value for key '_id' in the document.
  - The unique id allows you to sort your documents because it will also have some timestamp data in it.
  - Behind the scenes MongoDB uses BSON data and this conversion is done by the MongoDB drivers. This basically takes our JSON code and converts it into binary data. This conversion is done because, it is more efficient to store than JSON data, hence its faster. It is more efficient from space and size perspective as well and it supports additional types e.g. ObjectId, Date, RegularExpression, Timestamp etc.
  - In MongoDB two documents in the same collection can have different schemas. We may have some intersections or completely equal schema, but it is not a must.

- Insert one document in a collection with a user defined custom unique id:  
`> db.flightData.insertOne({ departureAirport: "TXL", arrivalAirport: "LHR", _id: "txl-lhr-1" })`

  ```json
  { acknowledged: true, insertedId: 'txl-lhr-1' }
  ```

  __Note__:
  - If you use the same id for some other document you will get a duplicate key error.

### Syntaxes for Different CRUD Operations

- Create:
  - `insertOne(data, options)`
  - `insertMany(data, options)`

  __Note__:
  - options: Used to configure the process.

- Read:
  - `find(filter, options)`
  - `findOne(filter, options)`
  
  __Note__:
  - filter: Allows us to narrow down the data.
  - find() returns all matching documents, findOne() returns the first match matching document.

- Update:
  - updateOne(filter, data, options)
  - updateMany(filter, data, options)
  - replaceOne(filter, data, options)

  __Note__:
  - filter: Allows us to narrow down which documents to change / update.
  - data: Describes what should be changed.
  - replaceOne() replaces the document entirely with a new one.

- Delete:
  - deleteOne(filter, options)
  - deleteMany(filter, options)

### Demonstration of Create, Read, Update and Delete Operations

- Delete a document from a collection.
`> db.flightData.deleteOne({ departureAirport: "TXL" })`

  ```json
  { acknowledged: true, deletedCount: 1 }
  ```

  __Note__:
  - We can also use the unique id to delete any document from a collection.

- Update a document in a collection.

  ```json
  > db.flightData.updateOne(
    { distance: 12000 },
    {
        $set: { marker: "delete" }
    }
  )
  ```

  ```json
  {
    acknowledged: true,
    insertedId: null,
    matchedCount: 1,
    modifiedCount: 1,
    upsertedCount: 0
  }
  ```

  __Note__:
  - TODO: 
