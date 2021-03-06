# Project Description:

- A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. Currently, there is no easy way to query the data for further analysis, since the data reside in a directory of CSV files on user activity on the app.

- As the data engineer assigned to the project, my role is to create an Apache Cassandra database which can create queries on song play data to answer the questions of the analytics team.

# Purpose:

- Modeling a NoSQL Database Tables Based on the Queries We Need To Extract From The Data.
- ETL Pipeline (Construct an ETL Pipeline to Copy Data From Multiple Log Files To a Single file).

# Queries We Need To Extract:

1. Give me the artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4
2. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182
3. Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

# Data pre-processing And ETL Pipeline:

- Data are stored as a collection of CSV files partitioned by date in the `event_data` folder.
- ETL copies data from the date-partitioned CSV files to a single CSV file `event_datafile_new.csv` which is used to populate the denormalized Cassandra tables optimised for the 3 queries we memtioned above.

# Data Modeling:

- We construct 3 tables based on the queries we mentioned above:

  1. `songs_heard` includes artist_name, song_title and song_duration information for a given sessionId and itemInSessionId.
  2. `users_history` includes artist_name, song_title , user_first_name and user_last_name for a given userId and sessionId.
  3. `users_per_song` includes users_first_name and users_last_name for a given song.

- Hint: The example Queries are returned as pandas DataFrame to facilitate further Data Manipulation.
