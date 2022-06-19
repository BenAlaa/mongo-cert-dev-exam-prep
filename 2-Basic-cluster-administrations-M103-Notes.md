# Chapter 1: The Mongod
---

## The Mongod Introduction
- daemon is a program or process that's meant to be run but not interacted with directly.
- mongod is the main daemon for mongodb, is the core server of the database to handle connections, requests and process data.
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


## Configuration File
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


## File Structure (WiredTiger)
You typicaly don't need to interact with this data folder to be modified may be to be read only.
These files isn't designed for user user modefications and if you modefied them you may face crashes or data lose.

- To List --dbpath directory:
    > ls -l /data/db
    ```
    WiredTiget       
    WiredTiget.wt  
    WiredTiget.lock 
    WiredTiget.turtle  
    WiredTigetLAS.wt   
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


## Basic Commands
- Shell helpers ---> wraps db commands
   ```
   db.<method>() --> data base commands
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


## Logging Basics
MongoDB provide to logging facilities to tracking activites on your database:
1. **Process Log**: collectes the activites into one of the following components: 
   - ACCESS - messages related to access controle, such as authentication
   - COMMAND - messages related to database commands
   - CONTROL - messages related to controle activities such as initialization
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
  - component.verbosity: for each individual component if set to -1 the will inherit the parent verbosity
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



## Profiling the Database

Profiling stores more detailed info than logging
not all actions are captured profiler

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

**slow ops**: by default any that takes longer than 100ms  and can be adjusted by setting the slowms variable

- To get the Profiling Level:
   ```db.getProfilingLevel()```
- To set the Profiling Level:
   ```db.setProfilingLevel(<level>, {slowms: <Number>})```


## Basic Security

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


## Roles
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



## Server Tools Overview

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