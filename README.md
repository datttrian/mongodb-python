# How to Use Python with MongoDB

Python, the top programming language for data science, and MongoDB, with
its flexible and dynamic schema, are a fantastic match for building
modern web applications, JSON APIs, and data processors, just to name a
few. MongoDB has a native Python driver and a team of engineers
dedicated to making sure MongoDB and Python work together flawlessly.

## What is Python?

Python, the Swiss Army knife of today’s dynamically typed languages, has
comprehensive support for common data manipulation and processing tasks,
which makes it one of the best programming languages for data science
and web development. <a
href="https://docs.python.org/3/tutorial/datastructures.html?highlight=dictionary"
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">Python’s native dictionary and list data types</a>
make it second only to JavaScript for manipulating JSON documents — and
well-suited to working with
<a href="https://www.mongodb.com/blog/post/bson" class="css-14ltky7"
tabindex="0" target="_target" data-track="true">BSON</a>. PyMongo, the
standard MongoDB driver library for Python, is easy to use and offers an
intuitive API for accessing databases, collections, and documents.

Objects retrieved from MongoDB through PyMongo are compatible with
dictionaries and lists, so we can easily manipulate, iterate, and print
them.

### How MongoDB stores data

MongoDB stores data in JSON-like documents:

``` python
# Mongodb document (JSON-style)
document_1 = {
  "_id" : "BF00001CFOOD",
  "item_name" : "Bread",
  "quantity" : 2,
  "ingredients" : "all-purpose flour"
}
```

Python dictionaries look like:

``` python
# python dictionary
dict_1 = {
  "item_name" : "blender",
  "max_discount" : "10%",
  "batch_number" : "RR450020FRG",
  "price" : 340
}
```

Read on for an overview of how to get started and deliver on the
potential of this powerful combination.

## Prerequisites

<a href="https://www.python.org/downloads/" class="css-14ltky7"
tabindex="0" target="_target" data-track="true">Download</a> and install
Python on your machine. To confirm if your installation is right, type
python --version in your command line terminal. You should get something
similar to:

```
Python 3.9.12
```

You can follow the python MongoDB examples in this tutorial even if you
are new to Python.

We recommend that you set up a MongoDB Atlas free tier cluster for this tutorial.

- [Launch Your Free Tier Cluster Now](https://www.mongodb.com/cloud/atlas/register)
- [Learn more about Atlas](https://www.mongodb.com/products/platform/atlas-database)



## Connecting Python and MongoDB Atlas

PyMongo has a set of packages for Python MongoDB interaction. For the
following tutorial, start by creating a <a
href="https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/"
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">virtual environment</a>, and activate it.

``` sh
python -m venv env
source env/bin/activate
```

Now that you are in your virtual environment, you can install PyMongo.
In your terminal, type:

``` sh
python -m pip install "pymongo[srv]"
```

Now, we can use PyMongo as a Python MongoDB library in our code with an
import statement.

### Creating a MongoDB database in Python

The first step to
<a href="https://www.mongodb.com/cloud/atlas" class="css-14ltky7"
tabindex="0" target="_target" data-track="true">connect Python to
Atlas</a> is to create a cluster. You can follow the instructions from
the
<a href="https://docs.atlas.mongodb.com/tutorial/create-new-cluster/"
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">documentation</a> to learn how to create and set up
your cluster.

Next, create a file named `pymongo_get_database.py` in any folder to write
PyMongo code. You can use any simple text editor, like Visual Studio
Code.

Create the mongodb client by adding the following:

``` python
from pymongo import MongoClient
def get_database():
 
   # Provide the mongodb atlas url to connect python to mongodb using pymongo
   CONNECTION_STRING = "mongodb+srv://user:pass@cluster.mongodb.net/myFirstDatabase"
 
   # Create a connection using MongoClient. You can import MongoClient or use pymongo.MongoClient
   client = MongoClient(CONNECTION_STRING)
 
   # Create the database for our example (we will use the same database throughout the tutorial
   return client['user_shopping_list']
  
# This is added so that many files can reuse the function get_database()
if __name__ == "__main__":   
  
   # Get the database
   dbname = get_database()
```

To create a MongoClient, you will need a connection string to your
database. If you are using Atlas, you can follow the
<a href="https://www.mongodb.com/docs/guides/atlas/connection-string/"
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">steps from the documentation</a> to get that
connection string. Use the connection_string to create the mongoclient
and get the MongoDB database connection. Change the username, password,
and cluster name.

In this python mongodb tutorial, we will create a shopping list and add
a few items. For this, we created a database user_shopping_list.

MongoDB doesn’t create a database until you have collections and
documents in it. So, let’s create a collection next.

### Creating a collection in Python

To create a collection, pass the collection name to the database. In a
new file called pymongo_test_insert.py file, add the following code.

``` python
# Get the database using the method we defined in pymongo_test_insert file
from pymongo_get_database import get_database
dbname = get_database()
collection_name = dbname["user_1_items"]
```

This creates a collection named user_1_items in the user_shopping_list
database.

### Inserting documents in Python

For inserting many documents at once, use the pymongo insert_many()
method.

``` python
item_1 = {
  "_id" : "U1IT00001",
  "item_name" : "Blender",
  "max_discount" : "10%",
  "batch_number" : "RR450020FRG",
  "price" : 340,
  "category" : "kitchen appliance"
}

item_2 = {
  "_id" : "U1IT00002",
  "item_name" : "Egg",
  "category" : "food",
  "quantity" : 12,
  "price" : 36,
  "item_description" : "brown country eggs"
}
collection_name.insert_many([item_1,item_2])
```

Let’s insert a third document without specifying the \_id field. This
time, we add a field of data type ‘date’. To add date using PyMongo, use
the Python dateutil package.

Start by installing the package using the following command:

``` sh
python -m pip install python-dateutil
```

Add the following to pymongo_test_insert.py:

``` python
from dateutil import parser
expiry_date = '2021-07-13T00:00:00.000Z'
expiry = parser.parse(expiry_date)
item_3 = {
  "item_name" : "Bread",
  "quantity" : 2,
  "ingredients" : "all-purpose flour",
  "expiry_date" : expiry
}
collection_name.insert_one(item_3)
```

We use the insert_one() method to insert a single document.

Open the command line and navigate to the folder where you have saved
pymongo_test_insert.py.

Execute the file using the command:

``` sh
python pymongo_test_insert.py
```

Let’s connect to MongoDB Atlas UI and check what we have so far.

<a href="https://cloud.mongodb.com/" class="css-14ltky7" tabindex="0"
target="_target" data-track="true">Log in to your Atlas cluster</a> and
click on the collections button.

On the left side, you can see the database and collection name that we
created. If you click on the collection name, you can view the data as
well:

<img
src="https://webimages.mongodb.com/_com_assets/cms/kq11g98k18w078bkb-python1.png?auto=format%252Ccompress"
alt="view of the database and collection name" />

<img
src="https://webimages.mongodb.com/_com_assets/cms/kq11hnztcbjdu6n6n-python2.png?auto=format%252Ccompress"
alt="view of data on click" />

The \_id field is of ObjectId type by default. If we don’t specify the
\_id field, MongoDB generates the same. Not all fields present in one
document are present in others. But MongoDB doesn’t stop you from
entering data — this is the essence of a schemaless database.

If we insert item_3 again, MongoDB will insert a new document, with a
new \_id value. However, the first two inserts will throw an error
because of the \_id field, the unique identifier.

### Querying in Python

Let’s view all the documents together using find(). For that, we will
create a separate file pymongo_test_query.py:

``` python
# Get the database using the method we defined in pymongo_test_insert file
from pymongo_get_database import get_database
dbname = get_database()
 
# Retrieve a collection named "user_1_items" from database
collection_name = dbname["user_1_items"]
 
item_details = collection_name.find()
for item in item_details:
   # This does not give a very readable output
   print(item)
```

Open the command line and navigate to the folder where you have saved
pymongo_test_query.py. Execute the file using the command:

``` sh
python pymongo_test_query.py
```

We get the list of dictionary object as the output:

<img
src="https://webimages.mongodb.com/_com_assets/cms/kq11j5s9n9opy5i4b-python3.png?auto=format%252Ccompress"
alt="dictionary list" />

We can view the data but the format is not all that great. So, let’s
print the item names and their category by replacing the print line with
the following:

``` python
print(item['item_name'], item['category'])
```

Although MongoDB gets the entire data, we get a Python ‘KeyError’ on the
third document.

<img
src="https://webimages.mongodb.com/_com_assets/cms/kq11nqsv4v7j9q82b-python4.png?auto=format%252Ccompress"
alt="Python KeyError" />

To handle missing data errors in python, use pandas.DataFrames. <a
href="https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html"
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">DataFrames</a> are 2D data structures used for data
processing tasks. Pymongo find() method returns dictionary objects which
can be converted into a dataframe in a single line of code.

Install pandas library as:

``` sh
python -m pip install pandas
```

Now import the pandas library by adding the following line at the top of
the file:

``` python
from pandas import DataFrame
```

And replace the code in the loop with the following to handle KeyError
in one step:

``` python
# convert the dictionary objects to dataframe
items_df = DataFrame(item_details)

# see the magic
print(items_df)
```

The errors are replaced by NaN and NaT for the missing values.

<img
src="https://webimages.mongodb.com/_com_assets/cms/kq11pilx53dhlz6hk-python5.png?auto=format%252Ccompress"
alt="NaN and NaT for the missing values." />

### Indexing in Python MongoDB

The number of documents and collections in a real-world database always
keeps increasing. It can take a very long time to search for specific
documents — for example, documents that have “all-purpose flour” among
their ingredients — in a very large collection. Indexes make database
search faster and more efficient, and reduce the cost of querying on
operations such as sort, count, and match.

<a
href="https://docs.mongodb.com/manual/indexes/#:~:text=Indexes%20support%20the%20efficient%20execution%20of%20queries%20in%20MongoDB.&amp;text=Indexes%20are%20special%20data%20structures,the%20value%20of%20the%20field."
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">MongoDB defines indexes</a> at the collection level.

For the index to make more sense, add more documents to our collection.
Insert many documents at once using the insert_many() method. For sample
documents, <a
href="https://github.com/mongodb-developer/python-mongodb/blob/main/pymongo_test_insert_more_items.py"
class="css-14ltky7" tabindex="0" target="_target" data-track="true">copy
the code from github</a> and execute python
pymongo_test_insert_more_items.py in your terminal.

Let’s say we want the items that belong to the category ‘food’:

``` python
item_details = collection_name.find({"category" : "food"})
```

To execute the above query, MongoDB has to scan all the documents. To
verify this, <a href="https://www.mongodb.com/try/download/compass"
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">download Compass</a>. Connect to your cluster using
the connection string. Open the collection and go to the Explain Plan
tab. In ‘filter’, give the above criteria and view the results:

<img
src="https://webimages.mongodb.com/_com_assets/cms/kq11tngsy155wzx3n-python6.png?auto=format%252Ccompress"
alt="Query results without index" />

Note that the query scans 14 documents to get five results.

Let's create a single index on the ‘category’ field. In a new file named
pymongo_index.py, add the following code.

``` python
# Get the database using the method we defined in pymongo_test_insert file
from pymongo_get_database import get_database
dbname = get_database()
 
# Create a new collection
collection_name = dbname["user_1_items"]
 
# Create an index on the collection
category_index = collection_name.create_index("category")
```

Explain the same filter again on Compass UI:

<img
src="https://webimages.mongodb.com/_com_assets/cms/kq11v6spo66dzzu7w-python7.png?auto=format%252Ccompress"
alt="Query results with index" />

This time, only five documents are scanned because of the category
index. We don’t see a significant difference in execution time because
of the small number of documents. But we see a huge reduction in the
number of documents scanned for the query. Indexes help in performance
optimization for <a
href="https://pymongo.readthedocs.io/en/stable/examples/aggregation.html"
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">aggregations</a>, as well. Aggregations are out of
scope for this tutorial, but here’s an
<a href="https://www.mongodb.com/basics/aggregation" class="css-14ltky7"
tabindex="0" target="_target" data-track="true">overview</a>.

## Conclusion

In this Python MongoDB tutorial, we learned the basics of PyMongo and
performed simple database operations. As a next step, explore <a
href="https://www.mongodb.com/blog/post/getting-started-with-python-and-mongodb"
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">using PyMongo to perform CRUD operations</a> with
business data. If you did not work along with this tutorial, start now
by <a href="https://www.mongodb.com/cloud/atlas" class="css-14ltky7"
tabindex="0" target="_target" data-track="true">installing MongoDB Atlas
for free</a>. There is also a course available on that specific topic at
<a
href="https://learn.mongodb.com/learning-paths/using-mongodb-with-python"
class="css-14ltky7" tabindex="0" target="_target"
data-track="true">MongoDB University</a>.
