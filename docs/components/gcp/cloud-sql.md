---
description: Run SQL queries and transactionally manage data in your SLAppForge Sigma serverless functions, with Google Cloud SQL managed DB instances
---

# ![Cloud SQL](images/cloud-sql/sql-icon.svg) [Google Cloud SQL](https://cloud.google.com/sql/)

Google Cloud SQL is a service that offers fully-managed SQL DBMS instances for use in cloud and non-cloud applications.
It currently supports MySQL and PostgreSQL, and instance capacities ranging up to 64 CPU cores and 400 GB of RAM.

The Cloud SQL model comprises
* [DB instances](https://cloud.google.com/sql/docs/mysql/create-instance) (actual VMs/containers allocated for DB hosting),
* [Databases](https://cloud.google.com/sql/docs/mysql/create-manage-databases) (actual DBs hosted in a given instance) and
* [Users](https://cloud.google.com/sql/docs/mysql/create-manage-users) (DB users associated with a given instance).

This model allows multiple databases to be hosted in the same instance,
and the same DB to be accessible via multiple users (similar to the model used by engines like MySQL).

Cloud SQL includes dynamic storage expansion (optional), data imports and exports,
and advanced features like replication, automatic backups and scheduled maintenance.


## Cloud SQL for Operations

From the developer's point of view, cloud SQL ultimately boils down to SQL.
Sigma generates Cloud SQL code using a FaaS-friendly SQL wrapper library
[`slappforge-sdk-gcp`](https://www.npmjs.com/package/slappforge-sdk-gcp).
This allows you to easily run SQL queries and transactions within your serverless function against a desired instance,
while the library takes care of DB connection management transparently.

Cloud SQL is supported on all platforms.

**NOTE:**

* `slappforge-sdk-gcp` (and hence Sigma) currently only supports MySQL databases.
* Sigma currently works with *already existing* (externally created) Cloud SQL instances, databases and users.
Ability to define new instances, databases and users within a GCP project will be made available soon!
* An active database connection (in fact, any application-initiatiaed TCP socket)
will usually prevent your serverless function from completing execution.
Hence you should **always** finalize/destroy the DB connection after your work is complete.
This is easily done by calling `end()` on the library-provided
[`connection` object](https://www.npmjs.com/package/mysql#user-content-establishing-connections).


### Prerequisites

1. Create a [Cloud SQL instance](https://cloud.google.com/sql/docs/mysql/create-instance).
2. Create a [database](https://cloud.google.com/sql/docs/mysql/create-manage-databases) under the instance.
3. Create a [user](https://cloud.google.com/sql/docs/mysql/create-manage-users) under the instance.
and associate him with the database with appropriate privileges.
The user should be able to access the database from all hosts (`%`)
since we would be accessing it from cloud functions that do not have fixed hostnames/IP addresses.
4. [Allow connectivity](https://cloud.google.com/sql/docs/mysql/connect-external-app#dynamicIP)
to your Cloud SQL instance from all IP addresses (for the same reason as above).
5. [Assign a public IP](https://cloud.google.com/sql/docs/mysql/configure-ip)
to your Cloud SQL instance, so that your client code can connect to it.

**NOTE:**

* You are [charged for the public IP](https://cloud.google.com/sql/pricing#instance-ip) while the instance is off.
* Cloud Functions offer [local socket-based access](https://cloud.google.com/functions/docs/sql#connecting_to_cloud_sql)
to Cloud SQL instances. Sigma does not currently support this, but you might be able to configure access manually
within your GCP-based Sigma project.


### Adding a Cloud SQL Operation

1. Drag a **Cloud SQL** entry from the **GCP Resources** pane on to the desired line in the editor.
2. Switch to the **Existing Instance** tab if you are not already on it.
3. Under **Cloud SQL Instance**, pick your desired instance.
4. **SQL Database** field would be populated automatically with databases available in the selected instance.
Under it, select the appropriate database.
5. **SQL User** field would be populated automatically with users available in the selected instance.
Under it, select the appropriate user.
6. Enter the password for the selected user. This will be stored as a
[non-persistent environment variable](../../features/environment-variables/environment-variables.md#persistence).
7. Select the appropriate SQL operation (**Run Query** or **Begin Transaction**).
8. For **Run Query**, enter an appropriate *parameterized query* under the **Query** field,
and the respective parameter values as a comma-separated list under **Inserts**.
   * Use quotes (`"JohnDoe"`) to denote literal strings.
   * Use simple variable names and JS snippets (`event.id`) to pass variables directly into the parameter list.
   * Use the `@` notation (`"@{person.first_name} @{person.last_name}"`) to generate composite strings.
9. Click **Inject** (or **Update**).

**NOTE:**

* `slappforge-sdk-gcp` uses an autogenerated file `SqlConnMgr.js` to maintain connection configurations.
**Do not** modify this file, unless you know what you are doing!
* Ensure that you call `connection.end()` once your work is complete.


### Available Cloud SQL Operations

Currently Sigma supports the following Cloud SQL operations.

- Begin Transaction
- Run Query


#### Begin Transaction

Starts a new SQL transaction against the selected instance and database. This does not need any parameter configurations.

The callback function will receive either:
* an `error` (first parameter) if the transaction could not be initiated, or
* a `connection` (second parameter) on success, bound with an active transaction


#### Run Query

Runs a SQL query (read or update) against the selected instance and database

**NOTE:** If you already have a DB connection object (e.g. one that was obtained from a **begin transaction** call)
you should pass it as the third parameter of the auto-generated code block in order to use it during the operation.
If not, a new connection will be created for the selected DB instance.
This is particularly important when you want to **run queries inside a previously initiated transaction**.

Field | Required | Supports Variables | Description
--- | :---: | :---: | ---
Query | :white_check_mark: | :white_check_mark:| The SQL query to run, can be *parameterized* if you want to pass external parameter values
Inserts | :x: | :white_check_mark:| Comma-separated list of parameters for the query (of any type, not only for inserts); use variables and literals as described under [Adding a Cloud SQL Operation](#adding-a-cloud-sql-operation)

The callback function will receive (in that order):
* an `error` if the query failed
* a `results` object containing the results of the SQL query execution:
e.g. `changedRows` for an `UPDATE`, `insertId` for an `INSERT`, an array of results for a `SELECT`, etc.;
refer [`mysql` client library docs](https://www.npmjs.com/package/mysql#user-content-getting-the-id-of-an-inserted-row)
for more examples
* the `connection` object that was used in the query

**NOTE:** If you are inside a transaction, make sure to call `connection.commit()`
or `connection.rollback()` appropriately within the callback.