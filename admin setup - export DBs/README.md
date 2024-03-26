# SQL Setup
> The contents of this directory are liquibase schema setup files,
> sample data for this project, and scripts to assist in generating dump files for each database technology.

# Process
When making changes to the schema, these have to be done in the database-schema.yml file first. If you want to edit the initial data, then make the changes in the sql-data folder.

Then, use Liquibase to generate the dumps for each database. And finally, commit the new dumps in the respective folder under 'user setup'.

# Installing

Follow the steps on the [Liquibase Docs for your OS](https://docs.liquibase.com/start/install/home.html) to get the core CLI.

Here's the current current matrix of databases Baeldung is targeting:

| Database   | Liquibase Support                       |
|------------|-----------------------------------------|
| MySQL      | Supported, but must manually add driver |
| PostgreSQL | Out of the Box                          |
| Oracle     | Possible support out of the box?        |
| SQL Server | Out of the Box According to Docs        |

# Generating Dumps

Each database was run in docker, and used the provided tooling within the docker container to extract the dump.
The scripts each contain a liquibase command and a dump generation command.

# Liquibase Commands

## Export Database with Liquibase

The liquibase command to export each database is largely the same. Update this command with the appropriate connection string:

```bash
liquibase generate-changelog --url=jdbc:mysql://localhost:3306 \
     --username=root \
     --password=password \
     --schemas=university \
     --diff-types=columns,foreignkeys,indexes,primarykeys,tables,uniqueconstraints,views,sequences,data \
     --data-output-directory=./sql-data \
     --changelog-file=database-schema.yml \
     --overwrite-output-file=true
```

## Import Database from Liquibase

The `generate-*-dump` scripts are intended to showcase the commands required per database tech to use the liquibase schema
and apply it to each database technology.

Note: The oracle script is not complete yet.

# MySQL

## Liquibase Setup
To use Liquibase with MySQL, you must add the MySQL driver (jar file) in the liquibase installation directory, `./liquibase/lib`.

Download the platform-independent archive from [the official downloads page](https://dev.mysql.com/downloads/connector/j/), unzip it, and grab the jar file.

## MySQL Setup

Download and install the MySQL Community Server and MySQL Workbench: https://dev.mysql.com/downloads/ After connecting to the MySQL Workbench, create a new database called 'university'.

Run the MySQL Liquibase command to create the tables from the YML schema into the 'university' database.

Then, follow the instructions in generate-mysql-dump.sh to create a DB dump from the university database. 

# PostgreSQL

Liquibase works with Postgres out of the box.

Download and install PostgreSQL: https://www.postgresql.org/download/ and the pgAdmin management tool: https://www.pgadmin.org/download/
Connect to the pgAdmin tool and create a new database called 'university'.

Then, follow the instructions in generate-postgres-dump.sh to create a DB dump from the university database. 

# Oracle DB

Supposedly Liquibase should work with Oracle DB out of the box, initial attempts were made with this free version of their container [here](https://container-registry.oracle.com/ords/f?p=113:4:101629893045627:::RP,4:P4_REPOSITORY,AI_REPOSITORY,P4_REPOSITORY_NAME,AI_REPOSITORY_NAME:1863,1863,Oracle%20Database%20Free,Oracle%20Database%20Free&cs=3twiMjqN3EOSBKzXHBgMMaAo2j4hCTCd2AQ1jFYpQV7qHYFJmydU4adDMtYETB3n43WxXP7fuLAAbU2ZSD7hLsg),
but all attempts to connect to the database and import the schema and data were unsuccessful.

# SQL Server

Liquibase works with SQL Server out of the box.

Download and install SQL Server Express Edition: https://www.microsoft.com/en-us/sql-server/sql-server-downloads and SQL Server Management Studio (SSMS): https://learn.microsoft.com/en-gb/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16 
Open SSMS and connect to the SQL Server, then create a new database called 'university'. If you get an error during the Connection, set the option Encryption to Optional in the "Connect" window.


Then, follow the instructions in generate-sqlserver-dump.sh to create a DB dump from the university database. 

# Dumps

Dumps for each database are readily available in their respective folder under 'user setup'.