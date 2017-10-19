# Log Analysis Project README

There are 3 procedures that are being called within the Python program:

	top3_articles
	popular_authors
	more_than_oneperc
	
This procedure is only importing 1 module,psycopg2(needed to be installed before use).  Each query for these procedures are defined in the beginning of the program:

	**query1**
	**query2**
	**query3**
	
Then each procedure is stated the same, the 2 changes for each are changing the name of the procedure and adding the appropriate 
query in the "c.execute()."  The procedure that is ran states:
  
  _DBNAME="news"_
  
  _def ():
    db = psycopg2.connect(database=DBNAME)
    c = db.cursor()
    c.execute(query*)
    result = c.fetchall()
    db.close()
    return result_

top3_articles is pulling data from the newsdata.sql file and using a SQL query to find the Top 3 articles of all time, ordered by the highest amount first.  The SQL query that is being ran states:

	SELECT articles.title, COUNT(log.id) AS cnt 
	FROM log, articles 
	WHERE log.path = CONCAT('/article/',articles.slug) 
	GROUP BY articles.title 
	ORDER BY cnt DESC 
	LIMIT 3
	
This query has been defined in the procedure as "query1".

popular_authors is pulling data from the newsdata.sql file and using a SQL query to find the most popular authors of all time and populating the views.  This SQL query is using 1 CREATE VIEW:

	CREATE VIEW numViews as 
	SELECT articles.author, count(path) as vws 
	FROM log, articles 
	WHERE articles.slug = substring(path,10) 
	GROUP BY articles.author; 
	SELECT name, vws 
	from authors, numViews 
	where authors.id = numViews.author 
	ORDER BY vws DESC
	
This query has been defined in the procedure as "query2".

more_than_oneperc is the final procedure to be defined using the data from newsdata.sql file.  The SQL query is using 2 seperate CREATE VIEWs and dividing them against each other to find the date that has errored out more than 1%.  The SQL query states:

	CREATE VIEW totals AS 
	SELECT date(time) as dt, count(*) as cnt 
	from log 
	GROUP BY date(time); 
	
	CREATE VIEW errors as 
	select date(time) as dt, count (*) as err 
	from log 
	where status like '%4%' or status like'%5%' 
	GROUP BY date(time); 
	
	select errors.dt, (cast(errors.cnt as float)/cast(totals.cnt as float))*100 as perc 
	from errors, totals 
	where errors.dt=totals.dt and (cast(errors.cnt as float)/cast(totals.cnt as float)*100)>1.0
	
After each of these procedures were defined, all 3 are printed with each of the answers to each question.
