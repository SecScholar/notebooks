At the basic level, a database is an organized collection of information on a computer. This information can be anything from names to files to transactions to even messages. This data is a vital part of an organization and allots the organization the capability to make informed decisions and plan effectively for the future.

With that being said, all databases do share some common characteristics.
1. Centralized access
	1. Multiple concurrent users
2. Security
	1. Privilege levels
3. Recovery
	1. Backup
4. Scalability

Any database is managed by a software package called a database management system (DBMS)

The DBMS determines the format and structure of the database and provides users with the tools to interact with that data.

Databases are comprised of many processes and server tools that are needed to create and use your database. Important processes are as follows:

- **Creation** - To determine what exactly will be stored  in the database, where the data will be stored, and how to make the data accessible to those who need it.
- **Input**/**Import** - To consider how the data will be entered into your database. In some cases much of the data can be entered manually. It's also possible to have the data imported from a different source.
- **Storage** - Some apps can be used to generate and present data for visual consumption only. The calculator in Windows is a good example. When the calculator is closed, all the data it generated is lost. However, many other apps need some place for its data to be stored so it wont be lost when the app is terminated. This storage is often referred to as data persistence. Persistent data is typically stored on a hard disk. A database is one option for persistent data storage.
- **Queries** - The information stored in a database is useful only if it can be easily retrieved. One of the best ways to retrieve database information is to use queries. A database query is a request to access data from the database.
- **Reports** - Closely related to queries is the ability to take the data queried and to transform it or format it in a way that makes it easy to read and interpret.
## Database Modeling
Data modeling is a way to help programmers and other related parties make sense of complex business flow.

Much like a blueprint that an architect creates to show how everything is supposed to fit together, these models help organize data and make sure there is a consistency in elements, such as naming conventions, default values, and the security apparatus within the database. They help identify missing and / or redundant data.

## Database Keys and other terms.
It's incredibly important to understand how database keys work if were going to be learning how to do this. Some important things to know and understand are:

- A primary key is found in one or more columns of data. 
- A primary key contains a unique identifier for the row. 
- Every entry must have a unique identifier in the primary key column.
- A foreign key is a column that refers to a primary key that exists in another table.
- These values can be duplicated, allowing an entry to reference a value from another table.
**Note**: In a database,
	an ***entity*** is a real-world element that's being used in a business.
		ex: Each customer of a business is an entity. A product a company sells is an entity.
	an ***attribute*** is a property or characteristic of an entity.
		 ex: The customers name and phone number are attributes.
	a ***relationship*** is an association or dependency between two entities.
		 ex: The sale of a product between a company and a customer is a relationship between two entities.

## Conceptual Data Models
 Conceptual data models are more simple since they only focus on the high level concepts. Some things to understand about the conceptual data models are:
 - This data model answers the question of what a system contains.
 - Is an organized view of the data you need to support the processes you business is running. 
 - It only looks at data that's being used and not how it's processed or its physical attributes.
 - It's purpose is to organize and define business concepts and rules.
 - It does not use a primary or foreign key.
## Logical Data Models
These models are more complex because it elaborates the details of the data.
- Answers the question of how a system *should* be structures. 
- Does not show how to implement the database with a specific DBMS.
- Does not take system hardware into account.
- It's purpose is to develop a technical map of rules and data structures.
- Provides the foundation on which a physical data model can be built.
- Can include both primary and foreign keys in addition to entity names, relationships, and attributes.
Logical data models typically include the following info:
	-  The objective and scope of the model. This communicates to the database developers what the end objective is.
	- The names of the objects or entries in the model. This can include any technical jargon that relates to the project.
	- Diagramming conventions
	- Business data points
	- Data abstractions
## Physical Data Models
This is the most complex type because its focused on how to implement a data model within a specific DBMS.
- Includes technical and performance requirements for the specific hardware system the database will run on.
- Describes the data that's going to be needed for a single project and should provide enough detail so the database itself can be created.