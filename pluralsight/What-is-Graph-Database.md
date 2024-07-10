# What is a graph database

In your roll, you may have come across relational database models like this one:

![Complex ERD Diagram](./assets/chapter-01/complex-erd.png)

Sure, querying one entity is easy enough, but what if you want all the details of an entity? You'd probably have to write a query that is long, and more important, very slow. And that's because relations are not first-class citizens in a relational database.

Graph databases are very mind friendly compared to other data storage technologies.

## What is a graph?

A graph consists of nodes which are connected by directional relationships. A node represents an entity. An entity is typically an abstraction of something in the real world, like an animal, a plant, a customer, etc.

![What is a graph](./assets/chapter-01/what-is-a-graph.png)

For example, imagine a graph representing Twitter users. John and Peter are the nodes in the graph representing the users. And the relationship between the two has the name *follows* that goes from John to Peter. So we read the graph as "John follows Peter". Others nodes can be added to the graph, representing other users, which can lead to more relationships. A collection of nodes and relationships together is called a graph. We can also extend the graph with other types of nodes and relationships.

![Graph example](./assets/chapter-01/graph-example.png)

Here Joanna has the *writes* relationship to a tweet, which results in the *to* relationship from the tweet to Nick, and the last relationship between Joanna and Nick.

So a graph is extendable with ease, and it supports the way the human brain works. If you ever explain some structure to others, chances are that you have drawn a graph on a whiteboard.

## What is a graph database?

A graph database uses the graph paradigm to store data. It is very strong in storing and retrieving highly-related data, and even though there are many relationships between them, a graph database will remain very performant during data retrieval, even with millions of nodes.

The graph databases products are very flexible because they don't use a fixed key map for the nodes. That means you can add or delete nodes and property of nodes without affecting already stored nodes. Because of that, a graph database responds well in an agile environment where course changes over product are very common.

All graph databases support one or more query languages to retrieve and store data, and they all implement the property graph model. A graph model contains nodes and relationships. In a graph database, nodes and relationships can also contain properties. In that way a node representing Joanna has a property called *name* with a certain value, could have the additional property *age* with a certain value, and so on. So that relationships can also contain properties. On a property graph model a relationship is named and directed between exactly two nodes, the start and end node.

![Graph Database](./assets/chapter-01/graph-model-example.png)

## Why a graph database?

Due to the popularity of relational databases, maybe it seems that storing data in a traditional relational way is all there is. No matter of the type of application, the data must fit in a relational database. The reality is that even a relational database ins't a one size fits all solution, for certain kinds of applications, a relational database is great, but relational databases are not very good with highly-related data, and that is where a graph database comes in.

The bottom line is to consider the type of database for every application you're writing. If your application works with highly-related data, consider the graph database. Also graph databases tend to have a schema-less approach. The structure of nodes and relationships is not fixed. It's easy to add a property to newer nodes and leave the other properties as they were. This give you the same flexibility as you have with most document databases, but not with relational databases. And the human brain is more compatible with graphs than it is with a relational schema. It's easier to think of the data structure and also easier to write queries, which are more written like patterns, which are then matched by the database. Those patterns are much like the patterns the brain uses to fetch data or retrieve memories.

## Graph databases vs. relational databases

Here are the most important differences between a relational database and a graph database.

| **Relational**                  | **Graph**                           |
|:-------------------------------:|:-----------------------------------:|
| Tables                          | Nodes                               |
| Schema with nullables           | No schema                           |
| Relations with foreign keys     | Relations are first class citizens  |
| Related data fetched with joins | Related data fetched with a pattern |

A table has vertically-oriented columns and horizontally-oriented rows that have fields that match the columns. A node is an entity. Nodes are not present in a table or other container, you may think of them as free floating.

Tables also have a pre-defined schema, everything that goes into the table must match the schema exactly, if there are rows that have to go in a table, which have fields that are not applicable, you can mark a column as nullable in the schema, and then instead of a value, a field in a row will have a null value, which later you will have to add checks for null in your queries and application. Node as entities can have their properties added or deleted without affecting already stored nodes, because every node is on its own, so there is no need to work with null values, either it has a property or not.

In relational databases relationships between rows in different tables can be created using the foreign key paradigm, where a foreign key column is specified as part of the schema of a table, thus the foreign key on the row level is a copy of a unique field in the related table, so it points to a row in the other table. In graph databases besides the nodes, relationships are first class citizens, which directionally point from one node to another node, they are just as important as nodes and are separate from them, therefore there is no data duplication.

To get data from a related table, you have to write a query that's called a *join*, to get data from more than one related table, you could have multiple *joins*, and with more *joins* in the query, the more the performance suffers exponentially. The query language in graph databases is optimized for relationships, all you have to is to issue your query as a pattern that is matched by the database, querying nodes related to other nodes is easy and take more time to execute, but the increase in wait time doesn't compare to a multi-joined in relational databases.

Now that you know when to use a graph database, here are some advantages of using relational databases:

- Use relational databases if storage and tables of uniformly-structured data is important, which is very suitable for reporting.
- Relational databases are also very good at doing calculations within one table (summarizing data). They beat a graph database if, for example, a sum of all other totals have to be calculated, or the average age of customers.

Creating foreign keys in relational databases is fine, but relational databases are not very good at retrieving nested data. Let's take the following example:

![Relational Nested Schema](./assets/chapter-01/relational-nested-schema.png)

We need several complicated and costly joins just to find out what products a customer bought. But it gets even more complex if you want to ask the database which customers bought a certain product, and to find out which customers bought this product who also bought that product, like Amazon does for example, is very difficult and will take the database too long.

Graph databases don't have this problem because its main purpose is relationships that can be queried in all kinds of ways without adding much to the waiting time. Jonas Partner and Aleksa Vukotic wrote a book called [Neo4j in Action](https://neo4j.com/books/neo4j-in-action-book/) and conducted an experiment. They imagined a social network with users that have friends, and the friends can have friends as well. Then they created a data model in a MySQL relational database, and the graph database Neo4j, and populated each one with one million people, each with about 50 friends.

Now in the following table, Depth 2 represents the timescale to find all friends of a user's friends. Depth 3 represents the timescale to find all friends of the friends of the user's friends. And so on. The larger the depth, the more the time it takes to find friends of friends. The values are presented in seconds.

![Timescale to find friends](./assets/chapter-01/timescale-to-find-friends.png)

It turns out that at Depth 2, the waiting times for both MySQL and Neo4j are acceptable. But for Depth 3 the relational database becomes unusable for an online system, and at level 5 they had to break the MySQL off because it just took too long, when Neo4j produced the results in just over 2 seconds.

This doesn't prove that a relational database is worthless, it's just not suitable for highly-connected data.

## Graph databases vs. document databases

For document databases, let's start with an example of a document.

![Document Example](./assets/chapter-01/document-example.png)

This is the data from the previous example in a document. We see that everything there is to know about a customer is now presented in one document. Documents are stored in document stores, in this case the customer document store.

Document databases encourage data denormalization, they work best if all related data of an entity is stored in one document, so duplication of data is fine. The candle product in our example is stored with an order, and orders with a customer. In that way the candle is present in multiple customer entities, and multiple orders.

The application still needs master data, for example, the products in the catalog are probably stored in a separate store, not only in the customer. The manner in which a document database is mostly used is that a product is copied to the customer document every time that product is ordered. That means the product is stored the way it was at the time of the order placement.

If this is an advantage or a disadvantage depends. If this is not desirable, foreign keys and joins are supported is most document databases, but this will result in the same disadvantage as they have with relational databases.

Let's imagine that we have two customer documents in the store.

![Document query example](./assets/chapter-01/many-documents-example.png)

You can imagine it's probably easy to construct a query that retrieves a customer and see what the customer bought, but it's getting more difficult if you want to get all customers that bought a certain product, or get all products a customer bought besides the candle.

In such cases, it's of course possible to get all customers and scan through them for the result. This is called brute force processing, but that's possible with all databases. The basic assumption here is that we want to use a query to get the result.

Again graph databases offer the better solution for highly-connected data, and again document databases have their place for scenarios where you want to store, for example, a complete JavaScript object and retrieve it later again as a JavaScript object.

| **Document**                                | **Graph**                           |
|:-------------------------------------------:|:-----------------------------------:|
| Documents                                   | Nodes                               |
| No schema                                   | No schema                           |
| Relations with "foreign keys" or embedded   | Relations are first class citizens  |
| Related data fetched with joins or embedded | Related data fetched with a pattern |

So a document database stores entities in documents, they typically contain nested data. In a graph database, we store the entities in nodes, some graph databases products support nested data in nodes, but the idea of a graph is that we can easily create more nodes and relationships, you don't have to worry about performance degradation because the graph database is optimized for relationships.

In a document, there is no predefined schema, just like in a node.

Documents have related data embedded, but the foreign key paradigm is also supported. In a graph database, the relation is an entity on the same level as a node, and it can also contain data in the form of properties.

In terms of queries, when a document is fetched, it already contains all related data, or you can issue a join. A query in a graph database is a pattern, including relationships, that is matched by the database to produce results.

## Examples of graphs

Let's look at some examples of graph databases.

![Social graph database](./assets/chapter-01/social-graph-database.png)

Here is a part of a social graph database. There are three different kinds of entities. In the top layer are companies, on the middle are people, and on the bottom layer are interest. The red relationships (from middle to top) have the name *Works_For*. The black ones (from middle to bottom) have the name *Skilled_At*.

Let's say that a company that has these graphs is interested in matching people with particular skills. An interesting query would be one that answers the following question: Who shares Cathy's skills? Show the names with the number of skill matches. Remember that relationships can contain properties, so we could store a 1 to 10 score in the *Skilled_At* relationship, and show the total score in the result set. Or another question could be interesting: Who works in the same company as Cathy and shares the most skills. Or we could connect the people with a new relationship called *Knows*, and look for people in other companies that know people of a given company which have a particular skill set. Such queries would be fairly easy to construct and fast to execute.

![Security graph database](./assets/chapter-01/security-graph-database.png)

Here's a simplified example of a security database, the top layer consists of nodes representing rights. These are bundled in the second layer, which have the security groups. The relationships between the groups and the rights are called *Contains*. People are on the third layer and can be in one or more groups by the relationships *As_Group*. It can also be connected to individual rights by the *right_granted* or *right_denied* relationship. With a well-crafted simple query, graph databases are able to quickly compose a list of all rights an individual has, and it's equally simple to determine who has a certain right. Let's say this model is used in an application for blog posts. We could easily extend the model to include the blog post as nodes and create relationships like *inserted* between persons and posts. This could also contain the *date* as a property. In that way, it's child play to determine who did what with a blog post.

![Logistics graph database](./assets/chapter01/logistics-graph-database.png.png)

The final example is about logistics. Here is a part of the underground London map, which is a typical example of a graph. Stations are nodes with connections in between, which are relationships. The relationships could contain a travel time. With a query, is possible to calculate the shortest routes between one station to the other, or determine a route based on other criteria, for example, you might want to include a snack bar on the route.

## Conclusion

In this chapter, you've learned that:

- A graph is a collection of nodes connected by relationships. Both can contain properties.
- Graph databases are flexible because nodes are schema-less, and performant with highly-related data.
- This doesn't mean other types of databases are useless, you should consider the database based on your application needs. You can even combine database types to gather the benefits of each.
- Relational databases are great for reporting and summarizing data.
- Document databases aim to store an object with as many relations as possible in a single document. It's also not very efficient if documents have to relate to each other.
