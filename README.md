Project summary:
----------------
Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. Their data is stored in JSON logs in a directory. 

This project implements a database schema and ETL pipeline to facilitate the analysis of the songplay data using Postgres as DBMS and Python to performe the ETL.

How it works:
-------------
Since the analytics team is priamrily in understanding what songs users are listening to, we implement a star schema with songplays at its centre as a fact table, with as its dimensions: songs, users, artists and time.

sql_queries.py: containts the SQL queries to build the tables, to insert rows and a SELECT query to find song_id and artist_id based on the song name, artist name and song length. 

create_tables.py: connects to the database and builds a sparkify database if doesn't exists. Once the database is created, this fils executes the SQL queries in sql_queries.py to build tables in the schema.

etl.py: processes the JSON files containing song, artist, user and songplay data into the tables. It also uses pandas library to perform some aspects of the ETL

etl.ipynb: implements the same script as etl.py, but on a single song file and log file.

test.ipynb: contains a number of SQL query to test the contents of the various tables after the ETL has been performed


How to use the scripts:
-----------------------
To build the schema(or to drop the tables and rebuild them)
python create_tables.py

To execute the ETL to load data into the tables:
python etl.py

Some queries used to test the results of the ETL using test.ipynb:

to combine artist name and song title:
%sql SELECT name, title FROM songs NATURAL JOIN artists LIMIT 5;

to view rows that appear on all 5 tables:
%sql SELECT * \
FROM songplays JOIN songs ON songplays.song_id = songs.song_id \
JOIN artists ON songplays.artist_id = artists.artist_id \
JOIN time ON songplays.start_time =  time.start_time \
JOIN users ON songplays.user_id = users.user_id \
LIMIT 5;

to check which rows have a song_id:
%sql SELECT * FROM songplays WHERE song_id != 'None' LIMIT 5;
