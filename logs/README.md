# **Logs Analysis** #

## **Discription** ##
The following questions were provided from Udacity:
When running vagrant and Oracle VM VirtualBox, you will be able to run my python file to see the output of each of the 3 questions.

To start, please extract all files from the compressed .zip file.

1.) Make sure vagrant and virtualization are running by running vagrant up, and logging in using vagrant ssh.
2.) Navigate to /vagrant/newsdata
3.) Run python news.py in bash.
4.) Output should display in bash.
5.) Text file should also be available in newsdata directory.

I have created a reporting tool to answer these question

## **Question One** ##
I did not use any views to get my results.


## **Question Two** ##
Created an articles_views:
CREATE VIEW article_views AS SELECT title, count(*) as views
    FROM articles a, log l
    WHERE a.slug = substring(l.path, 10)
    GROUP BY title
    ORDER BY views DESC;


## **Question Three** ##
Created a percent_error, error_view, all_view and query_3 view:

percent_error
CREATE VIEW percent_error AS
    SELECT a.date, CAST (a.errors as float) / a.all * 100 AS percent
    FROM query_3 a
    WHERE CAST (a.errors as FLOAT) / a.all * 100 > 1;

error_view
CREATE VIEW error_view AS
    SELECT date(time), count(status)
    FROM log
    WHERE status != '200 OK'
    GROUP BY date(time)
    ORDER BY count(status);

all_view
CREATE VIEW all_view AS
    SELECT date(time), count(status)
    FROM log
    GROUP BY date(time)
    ORDER BY count(status);

query_3
CREATE VIEW query_3 AS
    SELECT a.date, e.count AS errors, a.count AS all
    FROM all_view a
    LEFT JOIN error_view e
    ON a.date=e.date;

