#Code to connect to database; newsdata.sql
import psycopg2

# Question 1. What are the most popular three articles of all time?
query1 = "SELECT articles.title, COUNT(log.id) AS cnt FROM log, articles WHERE log.path = CONCAT('/article/',articles.slug) GROUP BY articles.title ORDER BY cnt DESC LIMIT 3;"

# Question 2. Who are the most popular article authors of all time?
query2 = "SELECT name, vws from authors, numViews where authors.id = numViews.author ORDER BY vws DESC;"

# Question 3. On which days did more than 1% of requests lead to errors?
query3 = "select errors.dt, (cast(errors.cnt as float)/cast(totals.cnt as float))*100 as perc from errors, totals where errors.dt=totals.dt and (cast(errors.cnt as float)/cast(totals.cnt as float)*100)>1.0;"

DBNAME = "news"

def top3_articles():
 db = psycopg2.connect(database=DBNAME)
 c = db.cursor()
 c.execute(query1)
 result = c.fetchall()
 db.close()
 return result

def popular_authors():
 db = psycopg2.connect(database=DBNAME)
 c = db.cursor()
 c.execute(query2)
 result = c.fetchall()
 db.close()
 return result

def more_than_oneperc():
 db = psycopg2.connect(database=DBNAME)
 c = db.cursor()
 c.execute(query3)
 result = c.fetchall()
 db.close()
 return result

print top3_articles()
print popular_authors()
print more_than_oneperc()
