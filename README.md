# Log Analysis Project README

There are 3 procedures that are being called within the Python program:

	### top3_articles
	### popular_authors
	### more_than_oneperc
	
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

popular_authors is pulling data from the newsdata.sql file and using a SQL query to find the 
