WebStudio platform
==================

## Introduction

The WebStudio is a web application that allows to manipulate energyml file (such as EPC or individual xml files)

## License

This project is licensed under the Apache 2.0 License - see the `LICENSE` file for details

## Related projects :

 - [cloud-storage-api](https://github.com/geosiris-technologies/cloud-storage-api)
 - [energyml-java-generator](https://github.com/geosiris-technologies/energyml-java-generator)
 - [energyml-utils](https://github.com/geosiris-technologies/energyml-utils)
 - [etpproto-java](https://github.com/geosiris-technologies/etpproto-java)
 - [etptypes-java](https://github.com/geosiris-technologies/etptypes-java)
 - [webstudio](https://github.com/geosiris-technologies/webstudio)



## Run standalone instance :

You can run the Webstudio-core (without user database nor workspace saving)

```console
docker run -d \
  -p 80:80 -p 443:443 \
  --env webstudio_enableUserDB=false \
  geosiris/geosiris-webstudio:1.0.2 
```

Or with docker-compose:
```bash
docker-compose -f docker-compose-core.yml -p webstudio-core up -d
```
**Note :** Remove the *-d* option for *up* command if you want to follow directly the main logs (not debug logs).


## Run the entire webstudio-platform :

Or with docker-compose:
```bash
docker-compose -f docker-compose.yml -p webstudio-platform pull
docker-compose -f docker-compose.yml -p webstudio-platform up -d
```
**Note :** Remove the *-d* option for *up* command if you want to follow directly the main logs (not debug logs).

This configuration uses the user database defined in `userDB`. The default login/password are : admin/nimda

### Reset all storage volumes (use carefully):

If you have trouble with your local docker volumes (user database or workspace save).

```bash
docker-compose -f docker-compose.yml -p webstudio-platform down --volumes
```


## Setting up project :

The webstudio can be setup with environment variables.

| Variable name                               | Value type                                                         | Working on version | Effect                                                                                                                                      |
|---------------------------------------------|--------------------------------------------------------------------|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| webstudio_enableUserDB                      | Boolean                                                            | \>= 1.0.1          | If **true** the application requires a user connexion at start (enable the user database)                                                   |
| webstudio_enableWorkspace                   | Boolean                                                            | \>= 1.0.1          | If **true** the user have his workspace saved between working sessions.<br/> *Note:* this is only availiable if the user database is enable |
| userdb_host                                 | String                                                             | \>= 1.0.1          | The user database (postgres) url                                                                                                            |
| userdb_port                                 | Integer                                                            | \>= 1.0.1          | The user database (postgres) port                                                                                                           |
| userdb_dbName                               | String                                                             | \>= 1.0.1          | The user database (postgres) name                                                                                                           |
| userdb_login                                | String                                                             | \>= 1.0.1          | The user database (postgres) login access                                                                                                   |
| userdb_password                             | String                                                             | \>= 1.0.1          | The user database (postgres) password                                                                                                       |
| userdb_hashSalt                             | String                                                             | \>= 1.0.1          | The user database (postgres) hashsalt (for passwords)                                                                                       |
| workspace_databaseType                      | String : <br/>- s3<br/>- azureblobstorage<br/>- googlecloudstorage | \>= 1.0.1          | Type of database to store the workspace                                                                                                     |
| s3_localstackEndpoint                       | String                                                             | \>= 1.0.1          | S3 bucket url                                                                                                                               |
| s3_accessKey                                | String                                                             | \>= 1.0.1          | S3 bucket access key                                                                                                                        |
| s3_secretKey                                | String                                                             | \>= 1.0.1          | S3 bucket secret Key                                                                                                                        |
| azureblobstorageproperties_connectionString | String                                                             | \>= 1.0.1          | Azure connection string                                                                                                                     |
| azureblobstorageproperties_containerName    | String                                                             | \>= 1.0.1          | Azure container name                                                                                                                        |
| googlecloudstorageproperties_keyfile        | String                                                             | \>= 1.0.1          | Google keyfile path                                                                                                                         |

## Check logs if run fails :

If an error like "org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs.", check the tomcat 
logs to see which servlet failed to start and why:

```bash
cat /usr/local/tomcat/logs/localhost.***.log
cat /logs/webstudio.log
```