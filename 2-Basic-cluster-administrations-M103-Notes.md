## Chapter 1: The Mongod
---

### The Mongod Introduction
- **daemon** is a program or process that's meant to be run but not interacted with directly.
- **mongod** is the main daemon for mongodb, is the core server of the database to handle connections, requests and process data.
- has the all main configurations options to make your data secure and consistent.
- each server has an instance of mongod
- if we have a cluster each server will run one mongod instance.
- can't be interacted with
- mongo client is what we interact with
- mongo client can communicate with the daemon
- We issue database commands like, insert and update to the client and the client take care to communicate with mongod to execute those commands.
- to start up a mongod process we need to run this command in the terminal "mongod"
- we can configure mongod by providing a configurations file or specifing flags
- There is  some defaults configurations when launch mongod without any options:
    - port 27017                                   ``--port <port>``
    - dbpath: /data/db                             ``--dbpath <path>`` where the data is stored like (dbs-collectoins-indexes-journaling info)
    - bind_ip: localhost                           ``--bind_ip <....ip1 , ....ip2 , ....>``
    - auth: disabled                               ``--auth`` auth enables authentication to control which users can access the database. When auth is specified, all database clients who want to connect to mongod first need to authenticate. 
    Before any database users have been configured, a Mongo shell running on localhost will have access to the database. We can then configure users and their permission levels using the shell. Once one or more users have been configured, the shell will no longer have default access
- will use mongo shell client to communicate with mongod by running this command "mongo"
- once mongo shell is connected to mongod you can issue db commands
- to shutting down the daemon we using the following commands:
    ```
    use admin
    db.shutdownServer()
    ```
- Other mongodb clients:
    - MongoDB Compass
    - Drivers (Node, swift, java, ...)


### Configuration File
#### command line options and their respective config commands

- ``--dbpath``   -->  ``storage.dbPath``
- ``--logpath``  -->  ``systemLog.path`` and systemLog.destination  ***must be provided if using fork***
- ``--bind_ip``  -->  ``net.bindIp``
- ``--replSet``  -->  ``replication.replSetName``
- ``--keyFile``  -->  ``security.keyFile``
- ``--sslPEMKey`` --> ``net.ssl.sslPEMKey``
- ``--sslCAKey``  --> ``net.ssl.sslCAKey``
- ``--sslMode``   --> ``net.sslMode``
- ``--fork``      --> ``processManagement.fork`` (to tell mongod to run as a daemon)  must be used with logpath or syslog
- ``--port``      --> ``net.port``

Commands Examples:
- Launch mongod with specified --dbpath and --logpath:
    ```
    mongod --dbpath /data/db --logpath /data/log/mongod.log
    ```
- Launch mongod and fork the process:
    ```
    mongod --dbpath /data/db --logpath /data/log/mongod.log --fork
    ```
- Launch mongod with many configuration options:
    ```
    mongod --dbpath /data/db --logpath /data/log/mongod.log --fork --replSet "M103" --keyFile /data/keyfile --bind_ip "127.0.0.1,192.168.103.100" --tlsMode requireTLS --tlsCAFile "/etc/tls/TLSCA.pem" --tlsCertificateKeyFile "/etc/tls/tls.pem"
    ```

All these options works fine but we need to rewrite all the options every time we need to startup mongod, instead we can use configurations file to store all the required options:

Config file is a YAML file (**Y**et **A**nother **M**arkup **L**anguage)

Example configuration file, with the same configuration options as above:
```
storage:
  dbPath: "/data/db"
systemLog:
  path: "/data/log/mongod.log"
  destination: "file"
replication:
# This is the name of the replica set for M103
  replSetName: M103
net:
  bindIp : "127.0.0.1,192.168.103.100"
tls:
  mode: "requireTLS"
  certificateKeyFile: "/etc/tls/tls.pem"
  CAFile: "/etc/tls/TLSCA.pem"
security:
  keyFile: "/data/keyfile"
processManagement:
  fork: true
```

To use config file there are to ways
- ```mongod --config "path_to_config_file"``` "/etc/mongod.conf"
- ```mongod -f "path_to_config_file"``` "/etc/mongod.conf"

For all available config file options you call check this:
```
systemLog:
   destination: file
   path: "/var/log/mongodb/mongod.log"
   logAppend: true
storage:
   dbPath: "/data/db"
   journal:
      enabled: true
processManagement:
   fork: true
net:
   bindIp: 127.0.0.1
   port: 27017
setParameter:
   enableLocalhostAuthBypass: false
...
##############################################################################################################
systemLog:
   verbosity: <int>
   quiet: <boolean>
   traceAllExceptions: <boolean>
   syslogFacility: <string>
   path: <string>
   logAppend: <boolean>
   logRotate: <string>
   destination: <string>
   timeStampFormat: <string>
   component:
      accessControl:
         verbosity: <int>
      command:
         verbosity: <int>

cloud:
   monitoring:
      free:
         state: <string>
         tags: <string>

net:
   port: <int>
   bindIp: <string>
   bindIpAll: <boolean>
   maxIncomingConnections: <int>
   wireObjectCheck: <boolean>
   ipv6: <boolean>
   unixDomainSocket:
      enabled: <boolean>
      pathPrefix: <string>
      filePermissions: <int>
   ssl:                            # deprecated since 4.2
      sslOnNormalPorts: <boolean>  # deprecated since 2.6
      mode: <string>
      PEMKeyFile: <string>
      PEMKeyPassword: <string>
      certificateSelector: <string>
      clusterCertificateSelector: <string>
      clusterFile: <string>
      clusterPassword: <string>
      CAFile: <string>
      clusterCAFile: <string>
      CRLFile: <string>
      allowConnectionsWithoutCertificates: <boolean>
      allowInvalidCertificates: <boolean>
      allowInvalidHostnames: <boolean>
      disabledProtocols: <string>
      FIPSMode: <boolean>
   tls:
      certificateSelector: <string>
      clusterCertificateSelector: <string>
      mode: <string>
      certificateKeyFile: <string>
      certificateKeyFilePassword: <string>
      clusterFile: <string>
      clusterPassword: <string>
      CAFile: <string>
      clusterCAFile: <string>
      CRLFile: <string>
      allowConnectionsWithoutCertificates: <boolean>
      allowInvalidCertificates: <boolean>
      allowInvalidHostnames: <boolean>
      disabledProtocols: <string>
      FIPSMode: <boolean>
      logVersions: <string>
   compression:
      compressors: <string>

security:
   keyFile: <string>
   clusterAuthMode: <string>
   authorization: <string>
   transitionToAuth: <boolean>
   javascriptEnabled:  <boolean>
   redactClientLogData: <boolean>
   clusterIpSourceAllowlist:
     - <string>
   sasl:
      hostName: <string>
      serviceName: <string>
      saslauthdSocketPath: <string>
   enableEncryption: <boolean>
   encryptionCipherMode: <string>
   encryptionKeyFile: <string>
   kmip:
      keyIdentifier: <string>
      rotateMasterKey: <boolean>
      serverName: <string>
      port: <string>
      clientCertificateFile: <string>
      clientCertificatePassword: <string>
      clientCertificateSelector: <string>
      serverCAFile: <string>
      connectRetries: <int>
      connectTimeoutMS: <int>
   ldap:
      servers: <string>
      bind:
         method: <string>
         saslMechanisms: <string>
         queryUser: <string>
         queryPassword: <string>
         useOSDefaults: <boolean>
      transportSecurity: <string>
      timeoutMS: <int>
      userToDNMapping: <string>
      authz:
         queryTemplate: <string>
      validateLDAPServerConfig: <boolean>

setParameter:
   <parameter1>: <value1>
   <parameter2>: <value2>

storage:
   dbPath: <string>
   journal:
      enabled: <boolean>
      commitIntervalMs: <num>
   directoryPerDB: <boolean>
   syncPeriodSecs: <int>
   engine: <string>
   wiredTiger:
      engineConfig:
         cacheSizeGB: <number>
         journalCompressor: <string>
         directoryForIndexes: <boolean>
         maxCacheOverflowFileSizeGB: <number> // deprecated in MongoDB 4.4
      collectionConfig:
         blockCompressor: <string>
      indexConfig:
         prefixCompression: <boolean>
   inMemory:
      engineConfig:
         inMemorySizeGB: <number>
   oplogMinRetentionHours: <double>

operationProfiling:
   mode: <string>
   slowOpThresholdMs: <int>
   slowOpSampleRate: <double>
   filter: <string>

replication:
   oplogSizeMB: <int>
   replSetName: <string>
   enableMajorityReadConcern: <boolean>

sharding:
   clusterRole: <string>
   archiveMovedChunks: <boolean>

auditLog:
   destination: <string>
   format: <string>
   path: <string>
   filter: <string>

snmp:
   disabled: <boolean>
   subagent: <boolean>
   master: <boolean>
```

- For all command line options check this doc [reference](https://www.mongodb.com/docs/manual/reference/program/mongod/#options)
- For all configurations file options check this doc [reference](https://www.mongodb.com/docs/manual/reference/configuration-options/)



**Note**
To create admin user for the lab use this example
```
mongo admin --host localhost:27000 --eval '
  db.createUser({
    user: "m103-admin",
    pwd: "m103-pass",
    roles: [
      {role: "root", db: "admin"}
    ]
  })
'
```


### File Structure (WiredTiger)
You typically don't need to interact with this data folder to be modified may be to be read only.
These files isn't designed for user modefications and if you modefied them you may face crashes or data lose.

- To List --dbpath directory:
    > ls -l /data/db
    ```
    WiredTiger      
    WiredTige.wt  
    WiredTiger.lock 
    WiredTiger.turtle  
    WiredTigerLAS.wt   
    _mdb_catalog.wt
    mongod.lock
    sizeStorer.lock
    collection-n.wt
    index-n.wt
    diagnostic.data
    journal
    storage.bson
    ```

**wiredTiger files**: 
this group of files is related to how wiredTiger storage engine keep track of info like cluster metadata and wiredTiger specific configurations options
  - wiredTiger.lock: act as a safty that prevent another mongodb process to point the same data folder


**Collections and Index files .wt**:
- The next group is the files end with .wt these files is the collection and index data itself.
- each collection and index has it's own file.
- These files are designed to interact with through the Mongodb server process rathera third party tool.
- Modeifying these can lead to data lose and crashes

**diagnostic.data folder**: 
  - to collect the dignostic data which is captured by Full Time Data Capture, or FTDC module.
  - These data is only use for diagnostic purpose by mongodb support engineers
  - It collects data from the following commands:
      ```
      serverStatus: db.serverStatus({tcmalloc: true})
      replSetGetStatus: rs.status()
      collStats for local.oplog.rs: db.getSiblingDB('local').oplog.rs.status()
      getCmdLineOpts: db.adminCommand({getCmdLineOpts: true})
      buildInfo: db.adminCommand({buildInfo: true})
      hostInfo: db.adminCommand(hostInfo: true)
      ```

**journal files**:
  - write operations are buffered in memory and flushed every 60 seconds or when the journal files reach 2 Gbytes
  - write ahead logging system to on-disk journal file. It's first buffered in memory and synced to disk every 50ms
  - journal file max 100 mega bytes in size
  - wiredTiger can use journal files to recover data in case of failure

**mongod.lock** is like wiredTiger.lock file

**mongod.log** file saves log files!

**mongodb-27017.sock** file in /tmp folder: socket file use to create socket connection to the specified port


### Basic Commands
- Shell helpers ---> wraps db commands
   ```
   db.<method>() --> database commands
   rs.<method>() --> replica set commands
   sh.<method>() --> sharding commands
   ```
- > db.<'Collection'>.<'method'>
- user management:
   ```
   - db.createUser()
   - db.dropUser()
   ```

- collection management:
   ```
   - db.renameCollection()
   - db.collection.createIndex()
   - db.collection.drop()
   ```

- Database management:
   ```
   - db.dropDatabase()
   - db.createCollection()
   ```

- Database status:
   ```
   - db.serverStatus()
   ```

- Database commands: under the hood
   ```
   - db.runCommand({ <'Command'> })
   - db.commandHelp("command")
   ```

- Creating index with Database Command:
   ```
   db.runCommand({
      "createIndexes":"<collection_name>",
         "indexes":[
            {
               "key":{ "product": 1 },
               "name": "name_index"
            }
         ]
      }
   )
   ```

- Creating index with Shell Helper:
   ```
   db.<collection>.createIndex(
      { "product": 1 },
      { "name": "name_index" }
   )
   ```


### Logging Basics
MongoDB provide to logging facilities to tracking activites on your database:
1. **Process Log**: collectes the activites into one of the following components: 
   - ACCESS - messages related to access control, such as authentication
   - COMMAND - messages related to database commands
   - CONTROL - messages related to control activities such as initialization
   - FTDC - messages related to the diagnostic data collection mechanism
   - GEO - messages related to parsing geo-spatial shapes
   - INDEX - messages related to indexing operations
   - NETWORK - messages related to network activities such as  accepting connections
   - QUERY - messages related to query planner and other query activities
   - REPL - messages related to replica set, such as initial sync or hearbeats
   - REPL_HB - messages related to  replica set heartbeats  nested under replication
   - ROLLBACK - messages related to replica set ROLLBACK  nested under replication
   - SHARDING - messages related to sharding operations
   - STORAGE - messages related to storage activities
   - JOURNAL - messages related to journaling activities
   - WRITE - messages related to write operations, such as update commands

to retrive a log commponents from db I can use this command:
   ```db.getLogComponents()```
#### Log verbosity Levels
  - verbosity: parent verbosity on the object level  ranges (0 -> 5) the higher the more verbose
  - component.verbosity: for each individual component if set to -1 it will inherit the parent verbosity
  - -1 : inherit from parent
  - 0: Default verposity to include informational message
  - 1-5: increase  the verbosity level to include debug messages

To look to logs we can use one of the following:
   1. ```db.adminCommand({"getLog": "global"})```
   2. ```tail -f 100 /path/to/log/file```

To change the verbosity level of one component use the following command:
   ```db.setLogLevel(<level>: "<component>")```


#### Severity levels: 
> F - fatal
E - error
W - warning
I - info (Verbosity Level 0)
D - debug (Verbosity Level 1-5)


#### Log Message sections:
``` 
<timestamp> 
<severity> 
<component> 
[<connection>] 
action <action>  ex command admin.$cmd  $ indicates db command
appName: <clientThatTriggeredOperation> ex mongoShell 
command: <the command itself> 
<metadata>
<operationTime>   10ms
```



### Profiling the Database

- Profiling stores more detailed info than logging
- not all actions are captured on profiler
- Profiler is enabled on database level for each db separatly
- if enabled, stores all operations on db in a new collection called ```system.profile```

**Events Captured by the Profiler:**
   - CRUD
   - Adminstrative operations
   - Configuration operations events are captured by profiler

**Profiler Settings:**
   - 0 -> Profiler is off and does not collect any data, this is the default.
   - 1 -> Profiler collects data for the operations that take longer than the value of ```ms```
   - 2 -> Profiler collect all data for all operations

**slow ops**: by default any operation that takes longer than 100ms  and can be adjusted by setting the slowms variable

- To get the Profiling Level:
   ```db.getProfilingLevel()```
- To set the Profiling Level:
   ```db.setProfilingLevel(<level>, {slowms: <Number>})```


### Basic Security

#### Authentication Mechanisms:
1. SCRAM(**S**alted **C**halleng **R**esponse **A**uthentication **M**echanism): is the default and the most basic form of client authentication (**Community Edition**)

2. X.509: this form using X.509 certificate for authentication. this is more secure and complex(**Community Edition**)

3. LDAP(**L**ightweight **D**irectory **A**ccess **P**rotocol)(**Enterprise Only**)

4. KERBEROS:(**Enterprise Only**)



#### Authorization: Role Based Access Control

- Each user has one or more roles.
- Each role has one or more privileges.
- A privilege represents a group of actions and the resources to those actions apply to.
- Roles support a high level of responsibility isolation for operational tasks.

**Note**
> when the mongodb is run for the first time, no users exist in the db
to connect you must connect from the local machine on which the server is run
after creating the first user, the local host execution is closed

Localhost Exceptions:
- Allows you to access a MongoDB server that enforces authentiaction but doesn't yet have a configured user for you to authenticate with.
- Must run the mongo shell from the same host
- The local exception closes after you created the first user
- Always create a user with adminstrative privileges first


Create new user with the root role (also, named root):
```
use admin
db.createUser({
  user: "root",
  pwd: "root123",
  roles : [ "root" ]
})
```

Connect to mongod and authenticate as root:
```
mongo --username root --password root123 --authenticationDatabase admin
```


### Roles
- Database users are granted roles
- **Custom Roles** : tailored roles to attend  specific needs of sets of users
- **Built-In Roles**: Pre-packed MongoDB Roles
- **Role structure**: 
   - set of privileges each privilege defines a set of actions over a resource
   - resources:
      - specific db and specific collection ```{db: "products", collection: "inventory"}```
      - all dbs and all collections ```{db: "", collection: ""}```
      - all dbs and specific collection ```{db:"", collection:"accounts"}```
      - specific db and any collection ```{db:"products", collection: ""}```
      - cluster --> replicasets or shards ```{cluster: true}```
   - Actions allowed over a resource ```{ resource: { cluster: true }, actions: ["shutdown"] }```
   - role can inherit from other roles
   - network auth previliges  (ip whitelist)

**Built-in Roles Sets**:
- Per Database level:
   - Database User
      - read
      - readWrite
   - Database Administration
      - dbAdmin
      - userAdmin
      - dbOwner
   - Cluster Administration
      - clusterAdmin
      - clusterManager
      - clusterMonitor
      - hostManager
   - Backup/Restore
      - backup
      - restore
   - Super user
      - root

- All Databases level:
   - readAnyDatabase
   - readWriteAnyDatabase
   - dbAdminAnyDatabase
   - userAdminAnyDatabase
   - root


**Main Common Roles**:
- **userAdmin role**: 
  - have all actions on user management (create user, change password, grant role, view role .....)
      - changeCustomData
      - changePassword
      - createRole
      - createUser
      - dropRole
      - dropUser
      - grantRole
      - revokeRole
      - setAuthenticationRestriction
      - viewRole
      - viewUser
  - doesn't have any access on data cannot list, read or write any data other than the users
      ```
      use admin
      db.createUser(
         {
            user: "m103-application-user",
            pwd: "m103-application-pass",
            roles: [
               {
                     db: "applicationData",
                     role: "readWrite"
               }
            ]
         }
      );
      ```

- **dbAdmin role**:
  - has access to DDL operations only.
  - Provides the ability to perform administrative tasks such as schema-related tasks, indexing, and gathering statistics. 
  - This role does not grant privileges for user and role management.
       ```
      db.creatUser(
         {
            user: "user_name",
            pwd: "p@ssw0rd",
            roles: [
               {
                     db: "m103",
                     role: "dbAdmin"
               }
            ]
         }
      );
      ```
  
  - system.profile collection:
      - changeStream
      - collStats
      - convertToCapped
      - createCollection
      - dbHash
      - dbStats
      - dropCollection
      - find
      - killCursors
      - listCollections
      - listIndexes
      - planCacheRead
  
  - All non-system collections (i.e. database resource):
      - bypassDocumentValidation
      - collMod
      - collStats
      - compact
      - convertToCapped
      - createCollection
      - createIndex
      - dbStats
      - dropCollection
      - dropDatabase
      - dropIndex
      - enableProfiler
      - listCollections
      - listIndexes
      - planCacheIndexFilter
      - planCacheRead
      - planCacheWrite
      - reIndex
      - renameCollectionSameDB
      - storageDetails
      - validate
   ```
- **dbOwner role**:
   - The database owner can perform any adminstrative action on the database
   - This role combines the privileges granted by  the **readWrite**, **dbAdmin** and **dbUser** roles.


**Note**
- in most cases you create user using admin database.
- we can add roles to user using the foloowing
   ```
   db.grantRolesToUser( "dba",  [ { db: "playground", role: "dbOwner"  } ] )
   ```
- to Show role privileges:
   ```
   db.runCommand( { rolesInfo: { role: "dbOwner", db: "playground" }, showPrivileges: true} )
   ```



### Server Tools Overview

List mongodb binaries: this will list all tools installed with mongodb
```
find /usr/bin/ -name "mongo*"
```

- Use **mongostat** to get stats on a running mongod process:
   ```
   mongostat --help
   mongostat --port 30000
   ```
- Use **mongodump** to get a BSON dump of a MongoDB collection:
   ```
   mongodump --help
   mongodump --port 30000 --db applicationData --collection products
   ls dump/applicationData/
   cat dump/applicationData/products.metadata.json
   ```
- Use mongorestore to restore a MongoDB collection from a BSON dump:
   ```
   mongorestore --drop --port 30000 dump/
   ```
- Use mongoexport to export a MongoDB collection to JSON or CSV (or stdout!):
   ```
   mongoexport --help
   mongoexport --port 30000 --db applicationData --collection products
   mongoexport --port 30000 --db applicationData --collection products -o products.json
   ```

- Tail the exported JSON file:
   ```
   tail products.json
   ```

- Use mongoimport to create a MongoDB collection from a JSON or CSV file:

   ```
   mongoimport --port 30000 products.json
   ```

**Notes**
- Mongodump can create a data file  and a metadata file, but mongoexport just create a data file only.
- By default, mongoexport send the output to standard output, but mongodump write to a file
- mongoexport is slower because its convert every file to json before export






## Chapter 2: Replication
---

### What is Replication?
Replication is the concept of manitaining multiple copies of your data.
This because you never assume that all your servers will be all over available.
To make sure at anytime any server is down you can still access your data **Availability**.

### MongoDB Replica Set
- Replicaset is a group of mongod nodes that work on the same data
- consists of one primary node that handles the data and secondary nodes that sync up with the primary
- if the primary fails, a secondary node takes it's place in a process called **failover** where nodes vote for which node will become the primary in a process called **election**
- Default Replication protocol ----> **pv1** which is based on **raft** protocol
- Read more about the [Simple Raft Protocol](http://thesecretlivesofdata.com/raft/) and the [Raft Consensus Algorithm](https://raft.github.io/).
- Operation log **oplog** is statement based log for each node in the Replicaset that keeps track for all write operations.
- every time a write is successfully applied to the primary node will get recorded in the oplog
- Arbiter member in a Replicaset doesn't hold data, cann't be primary and is used only as a tie-breaker in leader-primary node election
- odd number of nodes is prefered for election
- if you used even number make sure the magority are available as you will need to have at least 3 nodes available
- any failer or vote happen is add as s topology change in the replica set configuration which is defined in one node and shared between all the nodes
- the election will not happen if the majority isn't available for example the majority of 4 is 3 so if 2 nodes down the election won't happen
- Avoid using Arbiter
- We can defin a **hidden** node to provide a read only node or have a copy off the data hidden of the application.
- We can set a node as **delayed** for a specific time to work as a backup
- Replicaset can have up to 50 member
- only max of 7 nodes can be voting to minimize election time



### Setting Up a Replica Set
We will independtly launching 3 mongod process and try to connect them to replicate data for us.

1. adding keyFile to security section in config file so all members can authentiacte each other using this keyFile
2. This is adition to client auth
3. Create keyFile using openssl   ```openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile```
4. change permission of file to read permission ```chmod 600 /var/mongodb/pki/103-keyfile```
5. add replication.replSetName to config file

   ```
   storage:
      dbPath: /var/mongodb/db/node1
   net:
      bindIp: 192.168.103.100,localhost
      port: 27011
   security:
      authorization: enabled
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node1/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```

6. Create the dbPath folder ```mkdir -p  /var/mongodb/db/node1```
7. Start mongod using this config file ```mongod -f node1.conf```
8. create other 2 nodes with the same replSetName and keyfile just edit the dbPath, logPath and port by coping the configurations file
   ```
   cp  node1.conf node2.conf
   cp  node1.conf node3.conf
   ```
   ```
   storage:
      dbPath: /var/mongodb/db/node2
   net:
      bindIp: 192.168.103.100,localhost
      port: 27012
   security:
      authorization: enabled
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node2/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```
   ```
   storage:
      dbPath: /var/mongodb/db/node3
   net:
      bindIp: 192.168.103.100,localhost
      port: 27013
   security:
      authorization: enabled
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node3/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```
9. Create the dbPath folder for node2 ```mkdir -p  /var/mongodb/db/node2```
10. Start mongod using this config file of node2 ```mongod -f node2.conf```
11. Create the dbPath folder for node3 ```mkdir -p  /var/mongodb/db/node3```
12. Start mongod using this config file of node3 ```mongod -f node3.conf```
13. Connecting to node1: ```mongo --port 27011```
14. Initiating the replica set on one like node1 ``` rs.initiate()```
15. Creating a user: to use to connect to the rest node using it
      ```
      use admin
      db.createUser({
         user: "m103-admin",
         pwd: "m103-pass",
         roles: [
            {role: "root", db: "admin"}
         ]
      })
      ```
16. Exiting out of the Mongo shell and connecting to the entire replica set:
      ```
      exit
      mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin"
      -p "m103-pass" --authenticationDatabase "admin"
      ```
17. Getting replica set status: ```rs.status()```
18. Adding other members to replica set:
      ```
      rs.add("m103:27012")
      rs.add("m103:27013")
      ```
19. Getting an overview of the replica set topology: ```rs.isMaster()```
20. Stepping down the current primary: ```rs.stepDown()```
21. Checking replica set overview after election: ```rs.isMaster()```


### Replication Configuration Document
- BSON doc that holds the configuration of the replicaset and is shared across all nodes
- JSON Object that define the configuration options of our replica set.
- Can be configured manually from the shell
- There are set of mongo shell  replication helper methods that make it easier to manage :
   ```
   rs.add
   rs.initiate
   res.remove
   rs.reconfig
   rs.config || rs.conf
   ```


```
{
    _id: string,  // the name of the replicaset
    version: int,  // gets incremented every time the configuration changes
    members: [
        {
            _id: string, // can't be changed once set
            host: string,
            arbiterOnly: bool,
            hidden: bool,  // Not visible to app handles specific operations
            priority: number,  // range (0 - 1000) higher priority members tend to be elected more often, 
                               // changing the priority triggers an election
                               // priority 0 is excluded from being a primary  (arbiteronly and hidden should be 0)
            slaveDelay: int  // the riplication delay of the node in seconds
                             // setting this setting implies that the node will be hidden and the priority is 0
        },
        ...
    ]
}
```


### Replication Commands
**rs.status()**:
  - reports health of the nodes
  - uses data from heartbeats so it can be seconds out of date
  - optime: the last time the node did an operation from the oplog

**rs.isMaster()**:
  - describes the role of the node

**db.serverStatus()['repl']**:
  - is a section of the server status full output
  - similar to isMaster
  - rbid field doesn't appear on isMaster and it shows the number of times a rollback occured on the node

**rs.printReplicationInfo()**:
  - returns the oplog data relative to the current node
  - contains timestamp to the first and last oplog event in the node



### Local DB

- Display all databases (by default, only admin and local):
   ```
   mongo
   show dbs
   ```
- Display collections from the local database (this displays more collections from a replica set than from a standalone node):
   ```
   use local
   show collections
   ```
- has startup_log collection only in standalone node
- has several collections on replicset:
   >me
   oplog.rs
   replset.electoin
   replset.minvali
   startup_log
   system.replset
   system.rollback.id
   
   **oplog.rs**: 
   - is the center point of the replication mechanism 
   - It keeps track of all statements that is being replicated
   - is a capped collection (has max size)
   - if you run ```var stats = db.oplog.rs.stats()``` then:
    - ```stats.capped --> true``` if a capped collection
    - ```stats.size``` --> Get current size of the oplog
    - ```stats.maxSize ```--> Get size limit of the oplog 
   - by default it has 5% of the free disk space but can be cofigured through oplogSizeMB option under replication in the config file
   - is created after creating replset
   - once the limit is reached, the early operations are overriden
   - the replication window: 
      - the time it takes to fill the oplog completely and start overriding old statements
      - important to determine the time a node can afforded to be down without requireing human intervention to help it recover
      - it's inversly proportional to the system load
   - one operation can reslut in many entries in the oplog such as updateMany
   - data written in local dbs won't be replicated



### Reconfiguring a Running Replica Set

Let's assume we have a replica set of 3 nodes and we need to add 2 more 1 as Arbiter and 1 as secodary node.
- **node4.conf**:
   ```
   storage:
      dbPath: /var/mongodb/db/node4
   net:
      bindIp: 192.168.103.100,localhost
      port: 27014
   systemLog:
      destination: file
      path: /var/mongodb/db/node4/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```
- **arbiter.conf**:
   ```
   storage:
      dbPath: /var/mongodb/db/arbiter
   net:
      bindIp: 192.168.103.100,localhost
      port: 28000
   systemLog:
      destination: file
      path: /var/mongodb/db/arbiter/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```

- Starting up mongod processes for our fourth node and arbiter:
   ```
   mongod -f node4.conf
   mongod -f arbiter.conf
   ```
- From the Mongo shell of the replica set, adding the new secondary and the new arbiter:
   ```
   rs.add("m103:27014")
   rs.addArb("m103:28000")
   ```
- Checking replica set makeup after adding two new nodes:
   ```
   rs.isMaster()
   ```

- Removing the arbiter from our replica set:
   ```
   rs.remove("m103:28000")
   ```
- Assigning the current configuration to a shell variable we can edit, in order to reconfigure the replica set:
   ```
   cfg = rs.conf()
   ```

- Editing our new variable cfg to change topology - specifically, by modifying cfg.members:

   ```
   cfg.members[3].votes = 0
   cfg.members[3].hidden = true
   cfg.members[3].priority = 0
   ```
- Updating our replica set to use the new configuration cfg:
   ```
   rs.reconfig(cfg)
   ```



### Reads and Writes on a Replica Set
- By default read and write operations aren't allowed on secondary nodes
- to enable reading on a secondary node: ```rs.slaveOk()```
- writing is forbidden to replica sets
- if no nodes are secondary the primary will be secondary and we will not be able to write to the replica set


### Failover and Elections
suppose the following scenario: 
  - replicaset with 3 nodes 1p and 2s
  - to do a rolling upgrade we do:
      1. stop one of the secondary and bring it up with  the new version
      2. do the same to the other secondary node
      3. perform an election by running rs.stepDown() -> this will lead to that the primary node becomes a secondary
      4. do the step 1 to the secondary that was a primary


**Election**: happens when
  - there is a change in topology
  - reconfiguring a replicaset
  - the primary node becomes unavailable    (must elect a new node to be primary)
  - using rs.stepDown()    (must elect a new node to be primary)



  **Election candidates**:
  - if all nodes has the same priority then the one with the latest data will vote for itself and ask other nodes for support

  - if two nodes run for election simaltaneously in case of odd number of nodes ---> the odd node will decide the winner
   - in case of even number of nodes ---> a tie can happen then election will be repeated
  
  - a node with priority of 1 or higher can be elected
  - a node with priority of 0 can't run election and can't be primary, but can vote
  - a higher priority has a higher chance of being a primary
  - **Note** when running ```rs.isMaster()``` nodes that can't be elected show up as passives
  - if a primary can't reach any voting secondary, it wil automatically step down and be a secondary and if no primary in the replset then it won't be reachable


### Write Concerns
- write concerns are acknowledgement mechanism to increase durability
- for a write to be durable, majority of the nodes must acknowledge the success of the write
- **Levels**:
  1. 0          ```no wait for acknowledgement means the write might successed or failed```
  2. 1          ```(Default) wait for acknowledgment from primary only```
  3. greater than 1        ```waint for primary and one or more secondary members```
  4. majority   ```wait for the majority of the replicaset```

- **Options** :
  1. ```wtimeout```: (int) the time to wait for write concern before marking the operation as failed
  2. ```j```: (bool) node acknowledge the write and commit it in the journal files before returning an acknowledge. if j is false then the node will report success when the write stored in memory and before waiting for journaling

- **Write Concern Commands**:
   - insert
   - update 
   - delete 
   - findandmodify
- write concern is 1 by default
```
db.collection.insert(
   { }, 
   { writeConcern: { w: "majority", wtimeout: 60 } }
)
```



### Read Concerns
- returns the data if it has been saved to a number of nodes
**Levels**: 
  - ```local``` --> (default) most recent data to the cluster on primary only and doesn't guarantee that it is durable
  - ```available``` --> same as local and default against secondary differs in sharded clusters
  - ```majority```  --> returns if in a majority of the nodes not the latest note supported in MMApv1 storage engine
  - ```linearizable``` -->  like majority and read your only write functionality

**Notes**:
- local & available  ->> fast and latest but not safe
- majority  ->> fast and safe not latest
- linearizable  ->> safe and latest - not fast - single doc reads only
- for secondary reads ->> local & available is fast only but not always latest


**READ preference**
- Route read operations to secondary nodes
- is a driver-side setting
modes: 
  - ```primary``` (default) only to primary
  - ```primaryPreferred```  (primary and if not available the secondary)
  - ```secondary```   routes to only secondary
  - ```secondaryPreferred```    if no secondary then primary
  - ```nearest```  the least network latency to the host







## Chapter 3: Sharding
---
### What is Sharding?
- higher cost of vertical scaling
- scaling horizontaly by divid the data into multiple instances
- impact on operational tasks like (backup)  ---> backing up several 2 TB hard disks in parallel is faster than 20 TB single hard disk
- single threaded operations benefit from distributed environment ex: aggregation framework
- geographically distributed datasets


### Sharding Architecture
- we setting up a router process that accept queries from clients this router process called **Mongos**
- We can have any number of Mongos processes 
- Mongos using Metadata stored in config servers that has info about where each piece is stored.
- We need to make sure that the Metadata is highly availabe using replication 
- We deploy a Config Server Replica Set.
- **Primary Shard**
   - every database has a primary shard
   - that holds non-sharded collections
   - merge aggregation data from different shards




### Setting Up a Sharded Cluster
The minumum requirements to have a sharded cluster is to have (mongos process, one shard, CSRS)



1. deploy config server replicaset with mongod config file csrs_1.conf, csrs_2.conf, csrs_3.conf:
   ```
   sharding:
      clusterRole: configsvr
   replication:
      replSetName: m103-csrs
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   net:
      bindIp: localhost,192.168.103.100
      port: 26001
   systemLog:
      destination: file
      path: /var/mongodb/db/csrs1.log
      logAppend: true
   processManagement:
      fork: true
   storage:
      dbPath: /var/mongodb/db/csrs1
   ```
   ```
   sharding:
      clusterRole: configsvr
   replication:
      replSetName: m103-csrs
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   net:
      bindIp: localhost,192.168.103.100
      port: 26002
   systemLog:
      destination: file
      path: /var/mongodb/db/csrs2.log
      logAppend: true
   processManagement:
      fork: true
   storage:
      dbPath: /var/mongodb/db/csrs2
   ```
   ```
   sharding:
      clusterRole: configsvr
   replication:
      replSetName: m103-csrs
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   net:
      bindIp: localhost,192.168.103.100
      port: 26003
   systemLog:
      destination: file
      path: /var/mongodb/db/csrs3.log
      logAppend: true
   processManagement:
      fork: true
   storage:
      dbPath: /var/mongodb/db/csrs3
   ```
2. Starting the three config servers:
   ```
   mongod -f csrs_1.conf
   mongod -f csrs_2.conf
   mongod -f csrs_3.conf
   ```
3. Connect to one of the config servers:```mongo --port 26001```
4. Initiating the CSRS: ```rs.initiate()```
5. Creating super user on CSRS:
   ```
   use admin
   db.createUser({
      user: "m103-admin",
      pwd: "m103-pass",
      roles: [
         {role: "root", db: "admin"}
      ]
   })
   ```
6. Authenticating as the super user: ```db.auth("m103-admin", "m103-pass")```
7. Add the second and third node to the CSRS:
   ```
   rs.add("192.168.103.100:26002")
   rs.add("192.168.103.100:26003")
   ```
8. prepare mongos config file:
   ```
   sharding:
      configDB: m103-csrs/192.168.103.100:26001,192.168.103.100:26002,192.168.103.100:26003
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   net:
      bindIp: localhost,192.168.103.100
      port: 26000
   systemLog:
      destination: file
      path: /var/mongodb/db/mongos.log
      logAppend: true
   processManagement:
      fork: true
   ```
9. Start the mongos server: ```mongos -f mongos.conf```
10. Connect to mongos:
   ```
   mongo --port 26000 --username m103-admin --password m103-pass --authenticationDatabase admin
   ```
11. Check sharding status: ```sh.status()```
12. Updated configuration for node1.conf:
   ```
   sharding:
      clusterRole: shardsvr
   storage:
      dbPath: /var/mongodb/db/node1
      wiredTiger:
         engineConfig:
            cacheSizeGB: .1
   net:
      bindIp: 192.168.103.100,localhost
      port: 27011
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node1/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-repl
   ```
13. Updated configuration for node2.conf:
   ```
   sharding:
      clusterRole: shardsvr
   storage:
      dbPath: /var/mongodb/db/node2
      wiredTiger:
         engineConfig:
            cacheSizeGB: .1
   net:
      bindIp: 192.168.103.100,localhost
      port: 27012
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node2/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-repl
   ```
14. Updated configuration for node3.conf:
   ```
   sharding:
      clusterRole: shardsvr
   storage:
      dbPath: /var/mongodb/db/node3
      wiredTiger:
         engineConfig:
            cacheSizeGB: .1
   net:
      bindIp: 192.168.103.100,localhost
      port: 27013
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node3/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-repl
   ```
15. Connecting directly to secondary node (note that if an election has taken place in your replica set, the specified node may have become primary):
   ```
   mongo --port 27012 -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
   ```
16. Shutting down node:
   ```
   use admin
   db.shutdownServer()
   ```
17. Restarting node with new configuration:
   ```
   mongod -f node2.conf
   ```
18. Stepping down current primary:
   ```rs.stepDown()```
19. Adding new shard to cluster from mongos:
   ```sh.addShard("m103-repl/192.168.103.100:27012")```




### Config DB
Config DB maintained and used internally by mongodb, so generally you should never write any data to it. However it's got some useful information so we are going to read from it.

If you'd like to explore the collections on the config database, you can find the instructions here:
   - Switch to config DB:
      ```
      use config
      ```
   - Show config DB collections
      ```
      show collections
      ---------------------------
      actionlog
      changelog
      chunk
      collections
      databases
      lockpings
      locks
      migrations
      mongos
      shards
      tags
      transactions
      version
      ```
   - Query config.databases: will return each database in our cluster as one document
      ```
      db.databases.find().pretty()
      -------------------------
      {"_id": "m103", "primary": "m103-repl", "partitioned": true}
      ```
   - Query config.collections: gives us info on collections that have been sharded and the shard key used
      ```
      db.collections.find().pretty()
      -----------------------------
      {
         "_id": "config.system.sessions",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": ISODate("------------------------"),
         "dropped": false,
         "key": {
            "_id": 1
         },
         "unique": false,
         "uuid": UUID("f5f4ff-gfggfdgdg45-4455-d4f544f5d5df4")
      },
      {
         "_id": "m103.products",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": ISODate("------------------------"),
         "dropped": false,
         "key": {
            "salePrice": 1
         },
         "unique": false,
         "uuid": UUID("f5f4ff-gfggfdgdg45-4455-d4f544f5d5df4")
      }
      ```
   - Query config.shards: This tell us about the shards in our cluster
      ```
      db.shards.find().pretty()
      -------------------------
      {
         "_id": "m103-repl",
         "host": "m103-repl/192.168.103.100:27011,192.168.103.100:27012,192.168.103.100:27013",
         "state": 1
      },
      {
         "_id": "m103-shard-2",
         "host": "m103-shard-2/192.168.103.100:27014,192.168.103.100:27015,192.168.103.100:27016",
         "state": 1
      }
      ```
   - Query config.chunks: Each chunk for every collection in this database is returned as one document. The enclusive and exclusive maximum define the chunk range of the shard key value. That means that any document in the associated collection who's shard key value falss into this chunks range will end up in this chunk, and this chunk only
      ```
      db.chunks.find().pretty()
      -------------------------
      {
         "_id": "m103.products-salesPrice_MinKey",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": Timestamp(2,0),
         "ns": "m103.productions",
         "min": {
            "salePrice": {"$minKey": 1}
         },
         "max": {
            "salePrice": 14.99
         },
         "shard": "m103-shard-2"
      },
      {
         "_id": "m103.products-salesPrice_14.99",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": Timestamp(2,1),
         "ns": "m103.productions",
         "min": {
            "salePrice": 14.99
         },
         "max": {
            "salePrice": 33.99
         },
         "shard": "m103-shard-2"
      },
      {
         "_id": "m103.products-salesPrice_33.99",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": Timestamp(2,1),
         "ns": "m103.productions",
         "min": {
            "salePrice": 33.99
         },
         "max": {
            "salePrice": {"$maxKey": 1}
         },
         "shard": "m103-shard-2"
      }
      ```
   - Query config.mongos: holds data about the mongos processes connected to the cluster
      ```
      db.mongos.find().pretty()
      --------------------------
      {
         "_id": "m103:26000",
         "mongoVersion": "3.6.2-rc0",
         "ping": ISODate("------------------------"),
         "up": NumberLong(3892),
         "waiting": true
      }
      ```



### Shard Keys
This is the indexed field(s) used to partition collection on shards in our cluster.

**How the shard key is used to distribute your data?**

Consider a collection with some number of documents in them.
MongoDB uses the shard key to divide up these documents into logical groupings that MongoDB then distributes across our sharded cluster.

MongoDB he refers to these groupings as chunks.
The value of the field or fields we choose as our shard key help to define the inclusive lower bound, and the exclusive upper bound of each chunk.

Because the shard key is used to define chunk boundaries, it also defines which chunk a given document is going to belong to.


Every time you write a new document to the collection, the MongoS router checks which shard contains the appropriate chunk for that documents key value, and routes the document to that shard only.




- The shard key must be present on every document in the collection, and every new inserted document.
- The shard key Fields must be indexed and indexes must exists first before you can select the indexed fields for your shard key.
- Shard Keys are immutable:
   - cannot be changed after sharding
   - cannot specify another key or update a value for that key in any document
- Shard Keys are permanent:
   - cannot unshard a collection unless dropped and restored again.

**How To Shard**:
1. Use ```sh.enableSharding("<database>")``` to enable sharding for the specified database.
2. Use ```db.collection.createIndex()``` to create Index on shard key fields
3. Use ```sh.shardCollection("database.collection", {shard key: 1}) to shard the collection





