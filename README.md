# RDATA
RDATA is a Data Layer that aim to be lightweight, fast, distributed and scalable. 

The main goal of this project is work with large amount of data inside on Reactioon services/tools. But on reactioon ecossystem the users can create their own tools, and on this tool, can create anything.

## Why build it ?

At Reactioon we need to work with large amount of data (~50GB) in seconds to make predictions, and we need flexibility to create/upgrade our solutions. Actually Reactioon use 3 types of Databases (Elasticsearch, MySQL and MongoDB) and files to store data, all of these have a lot of benefits that increase speed and stability of reactioon, but in some cases, the things can go wrong. We want to be self-sufficient, so we search for alternatives that can offer the benefits inside of these apps in a lightweight and easy distribution solution (mongo is fast, but not easy to scale/distribute). To create an pattern for development of reactioon tools, we choose create a new data layer called `RDATA`.

## How it works ?

RDATA is based on `LevelDB`, so we are talking about a KVS (Key-Value-Store) that store the data inside of a specific location using hash table as reference to the data, and avoid random access. the kvs concept don't have server-client to access the data, because it is standalone. So, we create our own server-client around of it. The data inside of a node can be accessed over network using HTTP for multiple requests without locking. LevelDB only grown in a vertical way, with RDATA, we grow horizontally with multiples databases shared in the same machine or over network.

## KVS

Our focus is not create a KVS, but a storage layer that can work as database to serve data over network and with a minimal install as possible.

## Requirements

`RDATA` is projected to be easy to install and distribute. So, you don't need anything more than the file inside of `builds` folder.

* 1GB HD
* 256MB RAM
* Linux Debian 11 / OSX Ventura

**Note:** I've tested with debian and osx, you can check if works fine on other distros.

## Concept

`RDATA` have books concept. So, the whole flow of the RDATA is based on how a library works. 

First we have a `Collection`, and inside of a collection we have `Books`, and inside of `Books` we have `Documents`. Documents can be compared as a line of book, but in our case a line can store a lot of things, not only simple text, but structured data like numbers, documents, images, videos or only byte chars.

COLLECTION (aka, Database) -----> BOOKS (aka, TABLE) -----> DOCUMENTS (aka, RECORDS)

## Usage

RDATA is projected to be easy to install and use. all common operations can be made with an http request in the server API, passing the right arguments to desired method you will get a return of request.

You can found the full list of available endpoints [here]().

## Library

RDATA has support only to Go lang. If you want support in more languages, check out the Go library and create your own library.

## Features
* **Ready-to-Use, Easy-distribution, Lightweight and FAST**  
RDATA is projected to be fast, very fast with minimal setup and support heavy work loads.

* **Multi-model data store**  
Support for store multiple data types in a single book. The data can be text, number, documents, images, videos or only byte chars.

* **Horizontal scale & Data shards**  
RDATA has native support to scale data horizontally with distributed shards of data over network or inside the same machine.

* **Linear store**  
When the data changes, the order of a record do not change.

* **Content Revision**  
Internal revisions of a document show how much times the data changes.

* **Indexes Support**  
Indexes improve search results and resources when works with large amount of data.

* **Shared variables**  
Support to work with the same key-value from multiples locations.

* **Dynamic variables**  
Support to work with a key-value dynamically increasing or decreasing their value. (like redis, increase/decrease a number)

RDATA has some other useful features like Logs, REST API, Connection Pool, ACL (Whitelist) and much more.

## Install

The install a server node is easy, see the full command below:

```sh
./rdata-server -install
```

After execute the command, options will be asked to fill or you can use the default values.

```sh
Welcome to RDATA
The installer will start soon...

Location: (default: /reactioon/rdata)
/reactioon/rdata

Host: (default: localhost)
192.168.0.1

Port: (default: 60162)
60162

Create an service ? (Y = Yes) (default: No)
N

The install has been completed.
```

When the installer is working, the folder base will be created and a copy of this file will be saved on desired location. Inside of location a file of settings called `rdata.yaml` will be created, your can change nodes config inside of it.

**Notes:**

**(1):** If you are running a RDATA node with Linux that have `systemctl` installed, you can create the service automatically. This will enable the server start with the command `systemctl start rdata`.  

**(2):** `Location` is where your RDATA version will be saved, inside of it has a folder called data, inside of this folder you have the whole collection data.

## Run

The start a server node run the RDATA with `-run-server` flag, see the full command below:

```sh
./rdata-server -run-server
```

**Note:** When it's running the server-client will be started a can be open on browser. Check the list of endpoints below.

### Postman

To easy explore the features inside of RDATA, a collection of methods are available to Postman. So, you only import it and it's done, you can explore all options.


**@Note:** the Postman collectioon will be updated when new resources is added. So, remember to check it regularly.

## Endpoints

All communications with RDATA are performed using the HTTP protocol, whether to obtain a document, save or update.


#### Core / Home

```
curl --location 'http://{host}:{port}'
```

```json
{
    "_timestamp": "2023-07-18 04:59:10",
    "greetings": "Welcome to RDATA"
}
```

#### Core / Metrics  
**Endpoint (GET):** /rdata

```sh
curl --location 'https://{host}:{port}/rdata'
```

```json
{
    "_timestamp": "2023-07-18 06:05:23",
    "core": {
        "collections": {
            "length": 1,
            "names": [
                "test"
            ]
        },
        "hosts_allowed": [
            "192.168.1.101"
        ],
        "size": 85
    }
}
```  

#### Collection / Metrics
**Endpoint (GET):** /rdata/`{collection}`

```sh
curl --location 'https://{host}:{port}/rdata/test'
```

```json
{
    "_timestamp": "2023-07-18 21:26:52",
    "collection": {
        "_keys": "10",
        "_name": "test",
        "_size": 73,
        "books": {
            "_length": 2,
            "list": [
                {
                    "name": "users",
                    "pages": 4
                }
            ]
        },
        "indexes": {
            "_length": 1,
            "books": {
                "users": [
                    {
                        "docs": "1",
                        "index": "friends.0.age",
                        "location": "./buckets/test/_indexes/users/.friends.0.age"
                    }
                ]
            }
        },
        "variables": {
            "_length": 1,
            "list": [
                {
                    "_created": "2023-07-18 18:20:57",
                    "_revision": 0,
                    "_type": "string",
                    "_updated": "",
                    "_variable": "name",
                    "value": "Lorem ipsum dolor",
                    "value_last": ""
                }
            ]
        }
    }
}
```

#### Books / Metrics
**Endpoint (GET):** /rdata/`{collection}`/`{book}`/_metrics

```sh
curl --location 'https://{host}:{port}/rdata/test/users'
```

```json
{
    "_timestamp": "2023-07-18 21:30:17",
    "book": {
        "book": "users",
        "collection": "test",
        "documents": 10,
        "size": 76
    }
}
```

## Basic Operations `( GET | INSERT | UPDATE | DELETE )`

### Documents / `Get`
**Endpoint (GET)**: /rdata/`{collection}`/`{book}`/_get/`{document}`

```json
{
    "_timestamp": "2023-07-18 20:24:13",
    "document": {
        "_aid": 1,
        "_book": "users",
        "_content": {...},
        "_key": "1a41c839-62c1-40e0-8307-23100c593a82",
        "_revision": 0,
        "_timestamp": "2023-07-18 18:20:57"
    }
}
```

### Documents / `Insert`
**Endpoint (POST)**: /rdata/`{collection}`/`{book}`/_insert

```sh
curl --location 'https://{host}:{port}/rdata/{collection}/{book}/_docs/_insert' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'key=teste123' \
--data-urlencode 'value=blablabla'
```

```json
{
    "_timestamp": "2023-07-20 03:30:36",
    "document": {
        "key": "teste123",
        "status": {
            "msg": "the document has been created.",
            "type": "success"
        }
    }
}
```

### Documents / `Update`
**Endpoint (POST)**: /rdata/`{collection}`/`{book}`/_update

```sh
curl --location 'https://{host}:{port}/rdata/{collection}/{book}/_docs/_update' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'key=teste123' \
--data-urlencode 'value=blablabla2'
```

```json
{
    "_timestamp": "2023-07-20 03:03:26",
    "document": {
        "key": "teste123",
        "status": {
            "msg": "the document has been updated.",
            "type": "success"
        }
    }
}
```

### Documents / `Delete`
**Endpoint (POST)**: /rdata/`{collection}`/`{book}`/_delete

```sh
curl --location 'https://{host}:{port}/rdata/{collection}/{book}/_docs/_delete' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'key=teste1234'
```

```json
{
    "_timestamp": "2023-07-20 03:31:44",
    "document": {
        "key": "teste1234",
        "status": {
            "msg": "the document has been removed.",
            "type": "success"
        }
    }
}
```

## List
List is a easy way to get a list of documents inside of a book, and the data returned will stay in the order that they are added. So when the data changes the list still the same.

```sh
curl --location 'https://{host}:{port}/rdata/test/users/_docs/?limit=1&meta=1'
```

```
{
    "_timestamp": "2023-07-20 14:50:46",
    "book": "users",
    "documents": {
        "_length": 1,
        "data": [
            {
                "_aid": 13,
                "_book": "users",
                "_content": "blablabla",
                "_key": "teste1234",
                "_revision": 0,
                "_timestamp": "2023-07-20 14:50:42"
            }
        ]
    }
}
```


## Advanced resources

### Variables
**Endpoint (GET)**: /rdata/`{collection}`/_vars

Inside of reactioon the data are temporary or persistent, to work with these scenarios inside a collection of books has a group of documents called `variables` that can be used as a temporary/persistent storage. The content of a variable can be accessed from multiples locations without lacking. So, to improve the performance of your solution, the variables can be used to be a fast way to distribute data inside of a application like config settings.

```sh
curl --location 'https://{host}:{port}/rdata/{collection}/_variables'
```

```json
{
    "_timestamp": "2023-07-18 20:23:54",
    "collection": {
        "location": "./buckets/test"
    },
    "variables": {
        "_length": 4,
        "list": [
            {
                "_created": "2023-07-18 18:20:57",
                "_revision": 0,
                "_type": "string",
                "_updated": "",
                "_variable": "name",
                "value": "Lorem ipsum dolor",
                "value_last": ""
            },
        ]
    }
}
```
More details about VARIABLES, can be found in [VARIABLES]() file.

### Indexes
RDATA supports indexes to improve performance when searching for a document. Currently, the content to be indexed must be in JSON, if the content stored in the book is in JSON, it can be indexed and the document will be available with `_search`.

**Endpoint (GET)**: /rdata/`{collection}`/_indexes

```sh
curl --location 'https://{host}:{port}/rdata/test/_indexes'
```

```json
{
    "_timestamp": "2023-07-18 18:24:17",
    "collection": {
        "location": "./data/test"
    },
    "indexes": {
        "_length": 1,
        "books": {
            "users": [
                {
                    "docs": "1",
                    "index": "friends.0.age",
                    "location": "./data/test/_indexes/users/.friends.0.age"
                }
            ]
        }
    }
}
```

#### Create Index

```sh
curl --location 'https://{host}:{port}/rdata/{collection}/_indexes/_new?book=users&index=teste'
```

```json
{
    "_timestamp": "2023-07-20 05:03:21",
    "msg": "The index 'teste' has been created on book 'users'.",
    "status": "success"
}
```

#### Build Index
After creating the index the contents of the index must be constructed, then the construction must be communicated, and then the indexing will begin.

```sh
curl --location 'https://{host}:{port}/rdata/test/_indexes/_build?book=users&index=friends.age'
```

```json
{
    "_timestamp": "2023-07-20 05:20:54",
    "msg": "The build of index 'friends.age' will start in a few seconds.",
    "status": "success"
}
```

## Search

RDATA has resources to create a way search documents very fast, but the search only works with indexed fields for structured documents like JSON.

#### Documents / Search
**Endpoint (GET)**: /rdata/`{collection}`/`{book}`/_docs/_search?field=`{field}`&condition=`{condition}`&value=`{value}`&limit=`{limit}`&meta=`{meta}`

Search endpoint has a aditional number of fields to determine what is the context of the search. check the details below:

| Param    | Values | Details |
| -------- | ------- | ------- |
| collection  | * | collection name    |
| book  | * | book name    |
| field  | * | field name in a structured document like json    |
| condition | equal / contains | equal|contains     |
| value    | * | the value    |
| limit    | 0-N | Number of records to search    |
| meta    | 1 or 0 | Aditional details of the document called 'meta'. 1=enabled,0=disabled    |

```sh
curl --location 'https://{host}:{port}/rdata/test/users/_docs/_search?field=friends.0.age&condition=equal&value=44&limit=5&meta=1'
```

```json
{
    "_timestamp": "2023-07-18 20:24:16",
    "book": "users",
    "documents": {
        "_length": 5,
        "list": [
            {
                "_aid": 7,
                "_book": "users",
                "_content": {...},
                "_content_hash": "",
                "_content_length": 365,
                "_key": "teste123",
                "_revision": 0,
                "_timestamp": "2023-07-18 18:21:09"
            },
            {
                "_aid": 6,
                "_book": "users",
                "_content": {...},
                "_content_hash": "",
                "_content_length": 365,
                "_key": "teste1234",
                "_revision": 0,
                "_timestamp": "2023-07-18 18:21:07"
            },
            ...
        ]
    }
}
```

## Logs
RDATA has support for log operations, you can disable or custom logs inside of settings file (rdata.yaml).

```json
{
    "_timestamp": "2023-07-18 18:10:25",
    "collection": {
        "location": "./buckets/test"
    },
    "logs": {
        "_length": 10,
        "logs": [
            {
                "_lid": "10",
                "_timestamp": "2023-07-18 17:32:36",
                "collection": "test",
                "context": "book",
                "log": "added new record",
                "method": "test.users.Documents.Insert",
                "type": "INSERT"
            },
            {
                "_lid": "9",
                "_timestamp": "2023-07-18 17:32:34",
                "collection": "test",
                "context": "book",
                "log": "update new record",
                "method": "test.users.Documents.Update",
                "type": "UPDATE"
            },
        ]
    }
}
```
**Note:** When logs are active, performance will be reduced. If you are working with large loads of data, it is best to disable logging.

## Security

Most of solutions don't need high level of security, in most of cases, they prevent data using a Firewall. The default install makes the security disabled but only enable the access only from the machine installed (using ipv4).

If you want control the security of your data, you can setup ACL (Access Control Layer) to add hosts that can call with a collection of data. to change this, check the `SERVER_WHITELIST` inside of `rdata.yaml`.

## Version

This is a Release Candidate (RC) version, so this tool will be used in the whole ecossystem of Reactioon.

## Performance

RDATA is projected to be fast and to work with low resources requirements. So, the tool don't need dependencies, the data will be distributed horizontally to easy management/repair and performance.

At now, when use logs and with an database in ~50GB (Reactioon), the transfer of data between server-client are in miliseconds. To our case, it's like a dream.

You can do your own tests to check the performance in your enviroment, check the list of available tests below.

## Tests

RDATA has a set of arguments to help developers test the tool before starting their project. So, the developer can check performance and how it works.

**Basic tests**

**1. Populate**  
To test the data layer, we can populate the node with some data to see how it works. So, let's add some records. 

**@Note:** Don't worry about the data, you can delete the data folder and start again.

```sh
./rdata-server -run-test -populate -qty=10
```

With the command above we will populate the database with 10 records, to do stress test we can use a large amount os records.

**Loop tests**

Looped test is a way to do stress test of a node without intervention. So, you can use some actions to see how it will perform after a while. See the options below:

**1. Read variables**

```
./rdata -run-test -loop -variables-read
```
This test will perform read of a variable each times.

**3. Variable increase**

```
./rdata -run-test -loop -variables-increase
```
This test will perform read of a variable each times.

**4. Variable decrease**

```
./rdata -run-test -loop -variables-decrease
```
This test will perform read of a variable each times.

**5. Insert X documents inside of a book**

```
./rdata -run-test -loop -documents-insert -qty=10
```
**Note:** If the book don't exists, they will be created automatically.

**6. Get the last X documents and Update**

```
./rdata -run-test -loop -documents-update -qty=10
```
**Note:** The changes can be checked looking the `revision` of document.

**6. Get the last X documents and Delete**

```
./rdata -run-test -loop -documents-delete -qty=10
```
**Note:** The changes can be checked looking the `revision` of document.

## Use case

* **Blockchain L1/L2/...**  
The blockchain is a sequence of data blocks connecting each block using a signature, the most common usage of signature is a hash sha256/512. The same structure of a blockchain can be created inside of RDATA, on this is case RDATA it's a KVS (Key-Value-Store) that can scale horizontally and can be accessed over network, with shared variables and indexes support.

* **Standalone Apps**  
Many online solutions needs their own database installed locally to increase performance, on browser most of softwares can use indexedb to save data locally, but when works with multiple computers, a standalone database for each computer is needed. RDATA can be shipped inside of any software to create a data layer that can be accessed over network.

* **Internet of Things**  
Because of latency the network some devices without their own network has some problems with slow responses. The solution is an private network for each context, RDATA can be used to created multiples networks for each client, each network has their own data node and their own endpoints.

* **Web App**  
Web apps needs to be flexible, fast, secure and scalable. RDATA can be used to create a network to serve any type of data, to show as you want inside of your app using HTML+JAVASCRIPT. Your app will have their own database (installed locally).

* **DApps**  
DApps are solutions that use a smart contract inside of a blockchain as reference to execute a process, many of types processes can be executed with smart contracts. The problem with smart contracts to DApps are the cost of network with high usage, but the cost can be solved with a Layer 2 of an blockchain. It's mean the DApp will store their own data or make their own blockchain. So, you can use RDATA to store your own data with a  network and send to the blockchain only a signature of your content.

* **Data Science**  
Data science needs to work with large amount of data to make predictions, RDATA can help scientists work with large amount of data inside of a distributed environment over a network public/private.

## License

The usage of the RDATA is restricted to reactioon users, so the user needs a reactioon account and RTN to use it inside of their project.

The current version of RDATA is open and free to use to anyone, and can be used until 12/28/2023, after which the software will expire and stop. After that, the user must upgrade to the next version.

## Next steps

* WebSocket
* Transactions
* Filters
* Group & Order
* Live Events

## Considerations

RDATA is in development but can be used in development/production, actually the public version is only to test performance and resources comsumption, not a version ready to production. But, after a lot of tests inside of reactioon, RDATA can be the solution to all of our data structure. 

<br>

That's all! Folks!  
@author José Wilker

