# Log Analysis Project README
## What is it???

The database that is being used in this program contains information about a fictitious newspaper's web site about the articles on the site, the articles' authors, and a log of when clients attempted to access an article on the web site.  What we will be doing is calling 3 procedures within this program(top3_articles, popular_authors and more_than_oneperc).  There are 3 questions that are being asked in this procedure, by running:
	top3_articles; this is answering: What are the most popular three articles of all time?
	popular_authors; this will be answering: Who are the most popular article authors of all time?
	more_than_oneperc; will be answering: On which days did more than 1% of requests lead to errors?

### Software Needed:

	Python Program
	PostgreSQL
	Installing the psycopg2 Library
	
This pogram is being ran through a Virtual Machine environment using Vagrant.  You will have to download both programs, depending on your operating system (windows, Linux, Apple)
	Virtual-Machine: https://www.virtualbox.org/wiki/Download_Old_Builds_5_1
	Vagrant: https://www.vagrantup.com/downloads.html


top3_articles is pulling data from the newsdata.sql file and using a SQL query to find the Top 3 articles of all time, ordered by the highest amount first.  The SQL query that is being ran will not need a CEATE VIEW, just the stored SQL SELECT Query in the Python Program.  This query has been defined in the procedure as "query1".

popular_authors is pulling data from the newsdata.sql file and using a SQL query to find the most popular authors of all time and populating the views.  This SQL query is using 1 CREATE VIEW:

	CREATE VIEW numViews as 
	SELECT articles.author, count(path) as vws 
	FROM log, articles 
	WHERE articles.slug = substring(path,10) 
	GROUP BY articles.author; 
	
This query has been defined in the procedure as "query2".

more_than_oneperc is the final procedure to be defined using the data from newsdata.sql file.  The SQL query is using 2 seperate CREATE VIEWs and dividing them against each other to find the date that has errored out more than 1%.  The SQL query states:

	CREATE VIEW totals AS 
	SELECT date(time) as dt, count(*) as cnt 
	from log 
	GROUP BY date(time); 
	
	CREATE VIEW errors as 
	select date(time) as dt, count (*) as cnt 
	from log 
	where status like '%4%' or status like'%5%' 
	GROUP BY date(time); 
	
After each of these procedures were defined, all 3 are printed with each of the answers to each question.
