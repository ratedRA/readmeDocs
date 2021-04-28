## Solr
- open source ssearch engine, built on top of apache lucene which is also an open source.
- we can create indexes over relational database and non-sql database on solr to execute query faster
- we can also use it as a standalone database


## Why Solr?
- first let's talk about the traditional database (mysql)
  <p> Assume we need to return all relevant result for text <b>buy a stock</b>, for the normal mysql database we would need to write the query something similar to
  </p>
  
  ```
  Select *
  from schema_name.table_name
  where title Like %buy%
  OR title like %a%
  OR title like %stock%
  ```
  the problem with above query is it is very expensive for a large dataset. - 
  1. also, it is just providing substring matching i.e if the title contained <b>a</b> with anything it would have returned that and we just don't need that (resulting in some unwanted results).
  2. It doesn't understand linguistic variations - such as buying vs buy
  3. it doesn't understand synonyms like buying and purchasing
  4. couldn't identify unimportant words like 'a' or 'the'
 
 as result this query would be very expensive with huge customer base or a huge dataset
- Now, coming to solr it provides all the important infos by running a text analysis (removes unimportant words looks for synonyms, retuns most important results first)
- it does all of it using <b>inverted index</b> technique (which is heart of for all kind search engines)
- inverted index -> Solr is able to achieve fast search responses because, instead of searching the text directly, it searches an index instead. This type of index is called an inverted index, because it inverts a page-centric data structure (page->words) to a keyword-centric data structure (word->pages).


## SolrCore
- jetty web server hosts solr and jetty web server runs over JVM

<p>a solr core represents a single physical solr index. Each solr home directory can host multiple cores for a server and each core has a separate directory. A sole core directory has two files -
</p>
	1. conf -  it has
		1. managed-schema - configuration file that governs the indexed structure
		2. solrConfig.xml - main solr config file that defines how a core should behave
	2. data - actual data
- <b>difference between q and fq</b>
	- q returns most relevant documents before lesser relevant, while fq have no effect on relevancy
	- fq caches the result
-	<b>fl field</b>
	- can specify which fields we want to return (it would also return the relevancy score of the doc and also will fasten the query

-	we can also query solr using an http request. ex - 

```
http://localhost:8983/solr/item-listing/select?q=\*:\*&q=name&fq=name&sort=price%20asec..
item-listing - solr core name
select - request handler
```

## How does solr processes documents to build an index when we add the document?
-	when we add a document, solr takes the information in the document fields and adds that information to an index.

## What is a document?
- A document is solr basic unit of information which is a set of data that describes something. Each document contains one or more fields which are more specific pieces of information.

## creating solr core
- we can create a solr core using http command. we would need to give the instance directory to the api request - 

```
http://localhost:8983/solr/admin/cores?action=create&name=search_twitter&instanceDir=configs/search_twitter
```
- in this case solr is automatically creating the schema by detecting types of the fields
- to get the solr core information

```
http://localhost:8983/solr/search_twitter/schema/fields
```

## Using the API schema

<b> why to use? </b>

-	it handles all the reloading of the cores. After a manual edit we would need to reload or restart the cores to pick the changes.
- it also checks if changes are valid.

## Designing the Schema

- managed schema section
	- fields/dynamic fields elements
	- miscellaneous elements -  uniqueKey, cpoyField
	- field type elements - text, date, number etc..

-	Document Granularity
	-	based on what users wants to see in the results

-	Document Identification
	-	consider it same as the the primary key for the relational databases.
	- 	properties of unique id field - stored = true, indexed = true.
	-  if not provided solr itself adds the id field to the document. (but this would also lead to an issue of duplicate documents)
	-  if a document with the id exists and we try to add another document with the same id it would overwrite the old document with the new one.

-	indexed field
	-	field on which search would happen - indexed = true 