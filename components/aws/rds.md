# ![](images/rds/rds_icon.svg)  AWS RDS (Relational Database System)

Amazon RDS is a managed relational database service that provides six familiar database engines to choose from, including
Amazon Aurora, MySQL, MariaDB, Oracle, Microsoft SQL Server, and PostgreSQL. Amazon RDS handles routine database tasks
such as provisioning, patching, backup, recovery, failure detection, and repair. Amazon RDS makes it easy to use replication
to enhance availability and reliability for production workloads.

The Sigma IDE currently supports the following database engines.
- Amazon Aurora MySQL
- MySQL

## RDS for Operations
Sigma IDE provides the ability to execute any database query on RDS resources. Users have the ability to either select an
existing RDS instance or startup a new instance with an initialization DDL. Sigma provides ability to configure both
operation and instance resource under a single window without any overhead to the user.

To use RDS resource for operations, it should be dragged from the resources panel and dropped on the required line of the
lambda code editor. Then user should select either **Existing Instance** tab or **New Instance** tab based on the
requirement.

### Using Already Existing RDS Instance

If it's required to execute a query on an instance which already exists, first user should select the **Existing Instance**
mode tab. Then existing instances will be listed in the Instance drop down list. The naming convention used there is
<Database_Name> from the <Instance_Identifier>. User can easily select required instance from that drop down list.

Once user selects the instance, if this is the first time the selected instance is used in the project, Master Password
parameter will be requested from the user.

<p align="center">
  <img width="400" src="./images/rds/rds_existing_instance.png">
</p>

### Using New RDS Instance

If it's required to startup a new instance and execute a query on the newly created instance, first user should select the
**New Instance** mode tab. Then required parameters should be configured for the instance which is going to be created and
queried.

#### Configuring New RDS Resource

Configuration for an RDS instance mainly divides into two categories. Parameters in the first category, are highly coupled
with the Engine Type. The other category is the general configurations which are common to all Engine Types.

##### Engine Type Related Parameters - Aurora

<p align="center">
  <img width="400" src="./images/rds/rds_new_instance_aurora.png">
</p>

Field               | Required          | Description
---                 | :---:             | :---:   
DB Engine           | :white_check_mark:| A list of supported Engine Types. In the case of Aurora the value Aurora should be selected.
Edition             | :white_check_mark:| A list of available editions for the Engine Type selected above. As of now Sigma only supports MySQL edition. The other option PostgreSQL will be supported in the near future.
DB Instance Class   | :white_check_mark:| A list of DB instance classes. Choose the DB instance class that allocates the computational, network, and memory capacity required by planned workload of this DB instance.

##### Engine Type Related Parameters - MySQL

<p align="center">
  <img width="400" src="./images/rds/rds_new_instance_mysql.png">
</p>

Field               | Required          | Description
---                 | :---:             | :---:   
DB Engine           | :white_check_mark:| A list of supported Engine Types. In the case of MySQL the value MySQL should be selected.
License Model       | :white_check_mark:| License type associated with the database engine.
DB Engine Version   | :white_check_mark:| Version number of the database engine to be used for this instance.
DB Instance Class   | :white_check_mark:| A list of DB instance classes. Choose the DB instance class that allocates the computational, network, and memory capacity required by planned workload of this DB instance.

##### Common Configurations
