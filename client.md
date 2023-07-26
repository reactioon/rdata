# RDATA
RDATA is a Data Layer that aim to be lightweight, fast, distributed and scalable. 

The main goal of this project is work with large amount of data inside on Reactioon services/tools. But on reactioon ecossystem the users can create their own tools, and on RDATA, can create anything.

# Client

RDATA has an client to perform actions without internet using the terminal. The client is full compatible with server, and it's easy to use, check more details below.

### Requirements

RDATA Client needs an running instance of RDATA Server, to work the host server needs to be running. The client don't have an install process, just run and that's it.

### Commands

RDATA has a lot of commands to interact with the server, these commands are created with an sequence of segments that is available for each command. 

#### Segments

The segments of command is separated in 3 blocks (scope, source, data).

* Scope  
The scope is the first part of command, that represent the command that action will be executed.

* Source  
The source is the second part of command, represent the source, that can be a collection or collection/book.

* Data  
The data can be any param that an scope needs.

Structure:
```
     { scope }                 { source }             {route}       {data}   
[ [area][.[option]]     [core][collection][/book]     [route]      [params] ]
     (required)                (required)            (optional)   (required)  
```

Check the examples below to see multiples situations:  

1. **Info about the core**
```
info .
```

2. Info about the collection and info about the collection and book
```
info test
info test/users
```

3. **Insert document 123 with value 456**
```
docs.insert test/users key=123&value=456
```

4. **Request with route**

```
req test/users collection.books.documents.list limit=10&meta=1
```

All commands have the structure above. So, the first part is an `scope`, second is an `source` and last part is to be the `data`.

**@Note:** only use route when work with direct request. This is used in most of cases in development, so you don't need it. The mapped scopes in contexts is more secure. 

#### Scopes
The scopes of RDATA commands has two groups, basic and context. Basic is related to core commands and context is related to commands about an collection/book.

#### 1. Basic 
| Scope    | Details |
| -------- | ------- |
| about  | Info about RDATA    |
| info  | Get info about core,collection or book.    |
| help / ?  | show the full help inside of client.    |
| clear  | clear screen.    |
| exit  | close the client.    |

#### 2. Context

| Scope    | Details |
| -------- | ------- |
| req | direct call with route     |
| docs    | interact with documents inside of book.   |
| indexes    | Interact with document indexes of book.    |
| variables    | Interact with variables inside of an collection or core.    |
| schemas    | Interact with document schema of book inside an collection.    |

#### Sources
The client is only an interface to use the protocol, so the sources are custom collection of books located inside of the machine that is running the server.

#### Routes
The RDATA server use routes to reference actions, this is used only in development/test of resources.

Available routes:
| Scope    | Details |
| -------- | ------- |
| core.metrics | core metrics     |
| collection.metrics    | collection metrics   |
| collection.books.metrics    | book metrics    |
| collection.variables    | Variables of collection    |
| collection.variables.core    | Variables of core    |
| collection.variables.get | Get variable of collection     |
| collection.variables.set | Set variable inside of collection     |
| collection.books.schema.field.add | Add new document field to book schema     |
| collection.books.schema.field.remove | Remove document field from book schema     |
| collection.books.schema.build | Start build the contents     |
| collection.indexes | List of indexes inside of an book     |
| collection.books.indexes.create | direct call with route     |
| collection.books.indexes.build | direct call with route     |
| collection.indexes.queue | List of fields indexing content     |
| collection.books.documents.insert | Add new document     |
| collection.books.documents.update | Update an document using key     |
| collection.books.documents.delete | Delete an document using key     |
| collection.books.documents.get | Get an document using key     |
| collection.books.documents.list | List documents inside of an book     |
| collection.books.documents.search | Search documents inside of an book     |
| collection.books.documents.filter | Filter documents using logical operators (>,<,=,>=,<=)     |
| collection.logs | direct call with route     |

@Note: THE ROUTES IS USED ONLY IN DEVELOPMENT, DON'T USE IT.  

#### Data

| Scope    | Details |
| -------- | ------- |
| key | The key of an document     |
| value    | The value of an document   |
| limit    | Max numbers of records that request can return    |
| meta    | Enable or disable the meta tags contents of an request.    |

### Example of most used commands

```json
info .
docs.insert test/users key=123
docs.update test/users key=123&data=456
docs.delete test/users key=123
docs.list test/users limit=10&meta=1
docs.get test/users key=123
```

## Considerations

The client is under development, if you find any problem while using it, call with the supoort. At this moment, we have tested in many situations and it's working fine, so you are free to use it too. 

That's all!   
Jose Wilker 
