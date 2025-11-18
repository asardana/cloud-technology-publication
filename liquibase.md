Over the last few years, the proliferation of microservices and cloud native architecture patterns have surfaced new challenges that are resulting in new tools and techniques being adopted by enterprises. This allows for a seamless transition for enterprises in their cloud native journey to be more agile and nimble.

In this post we’ll look at how Liquibase can be used to manage a relational database when developing a microservice.  Liquibase is an open source library that is useful for managing the changes to the database schema and also the migration of the database.

Liquibase scripts can be written in multiple formats like JSON, XML, YAML etc that are agnostic to the actual database. This makes it quite easy to migrate the application from one relational database to another with Liquibase doing the heavy-lifting for  translating the Liquibase scripts to the database specific SQLs.

Liquibase consists of change sets which are basically a small set of changes applied to the database. Liquibase tracks the execution of the change sets and records them in a change log table. Change sets can be created representing multiple releases and organized in a sequential order in the change log file. This allows for versioning the database updates and maintaining all the scripts in a central location. Whenever a change needs to be applied to database, we can create a new change set and add it to the change log. This allows for having an immutable log of all the database schema changes and minimizes any risk of inadvertently applying a wrong patch during application install or database migration.

The code discussed in this post is available on Github.

Setting up MariaDB using Docker
First we’ll set-up the MariaDB using Docker.

Pull the MariaDB docker image
docker pull mariadb:10.0.22

Run the docker image to create the container
 docker run -p 3306:3306 –name loan-data-mariadb-10.0.22 -e MYSQL_ROOT_PASSWORD= -d mariadb:10.0.22

After the container is up and running, execute the following command to create a new bash session in the container
docker exec -it loan-data-mariadb-10.0.22 bash

VIM can be installed to view any MySQL files inside the container, if required
apt-get update

apt-get install vim

Login to MySQL shell
mysql –user=root –password=<password>

Create a new user
create user <user id> IDENTIFIED BY <password>;

Configure the new user for remote client access
GRANT ALL PRIVILEGES ON *.* TO <user id> IDENTIFIED BY <password>;

Ensure that the user is successfully created
SELECT User, Host FROM mysql.user WHERE Host <> ‘localhost’;

Liquibase Scripts
The sample application contains a changelog.xml under resources containing multiple change sets.

The first change set creates a new database table

```
<changeSet id="1" author="aman">
    <createTable tableName="loan_details">
        <column name="id" type="int">
            <constraints primaryKey="true" nullable="false"/>
        </column>
        <column name="loanAmt" type="DOUBLE(6,2)">
            <constraints nullable="false"/>
        </column>
        <column name="fundedAmt" type="DOUBLE(6,2)">
            <constraints nullable="false"/>
        </column>
        <column name="term" type="VARCHAR(50)">
            <constraints nullable="false"/>
        </column>
        <column name="intRate" type="VARCHAR(10)">
            <constraints nullable="false"/>
        </column>
        <column name="grade" type="VARCHAR(10)">
            <constraints nullable="false"/>
        </column>
        <column name="homeOwnership" type="VARCHAR(10)">
            <constraints nullable="false"/>
        </column>
        <column name="annualIncome" type="DOUBLE(8,2)">
            <constraints nullable="false"/>
        </column>
        <column name="addressState" type="VARCHAR(10)">
            <constraints nullable="false"/>
        </column>
        <column name="policyCode" type="VARCHAR(2)">
            <constraints nullable="false"/>
        </column>
    </createTable>
</changeSet>
```
With the above table, the application would fail due the errors with auto increment of primary key and type mismatch for few of the columns. After fixing the table by applying following change sets, the application should start and create the table in MariaDB without any issues.

```
<changeSet id="2" author="aman">
    <addAutoIncrement tableName="loan_details" columnDataType="int" columnName="id" incrementBy="1"
                      startWith="10000"/>
</changeSet>
 
<changeSet id="3" author="aman">
    <modifyDataType tableName="loan_details" columnName="loanAmt" newDataType="DOUBLE(10,2)"/>
    <modifyDataType tableName="loan_details" columnName="fundedAmt" newDataType="DOUBLE(10,2)"/>
    <modifyDataType tableName="loan_details" columnName="annualIncome" newDataType="DOUBLE(10,2)"/>
</changeSet>
```
The application has Liquibase dependency which provides the run-time feature for translating the XML scripts to MariaDB SQL statements. Liquibase will match the current state of the Database with the change sets defined in the change log and apply any necessary changes based on the information available in the change log table.

After the application is started successfully, we can see the following 3 tables created by Liquibase. There are 2 tables created by Liquibase, one for tracking the schema changes and the other for allowing it to take a lock while one instance of application is running the scripts against the database. This prevents multiple instances of the same microservice to run the Liquibase scripts against the single instance of the database.

MariaDB1
