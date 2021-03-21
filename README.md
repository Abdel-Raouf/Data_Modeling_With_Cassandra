# Purpose:
- Modeling a NoSQL Database Tables Based on the Queries We Need To Extract From The Data.

# Queries We Need To Extract:
1. Give me the artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4
2. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182
3. Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

# Data pre-processing And ETL Pipeline (First Part in the Notebook):
- Data are stored as a collection of CSV files partitioned by date.
- ETL copies data from the date-partitioned csv files to a single csv file event_datafile_new.csv which is used to populate the denormalized Cassandra tables optimised for the 3 queries we memtioned above.

# Data Modeling (Second Part in the Notebook):
- We construct 3 tables based on the queries we mentioned above:
  1. `songs_heard` includes artist_name, song_title and song_duration information for a given sessionId and itemInSessionId.
  2. `users_history` includes artist_name, song_title , user_first_name and user_last_name for a given userId and sessionId.
  3. `users_per_song` includes users_first_name and users_last_name for a given song.

- Hint: The example Queries are returned as pandas DataFrame to facilitate further Data Manipulation.  

# Primary Keys Selection:
1. For the first query: 
  - We need to return songs that's heard with it's duration, during a specific session with the number of items that exists in this session:
    1. we made `SessionId` the partition key, as to partition data related to each session.
    2. We made `ItemInSession` the Clustering key, as to sort the data within each partition related to the `ItemInSession`.
2. For the second query: 
  - We need to return the users fullnames that heard a specific song(based on the `ItemInSession`), related to which session (users songs history):
    1. we made `UserId` the partition key, as to partition data related to each user.
    2. We made `SessionId` the Clustering key, as to sort the data within each partition related to the session the user was loged-in when he/she was hearing the song.
    3. We add another Clustering key `ItemInSession`, as to apply sorting by `SessionId` ,then a second level sorting(nested sorting) with `ItemInSession` related to each song.
3. For the third query:
  - We need to return every user listened to the specific song we want:
    1. we made `songTitle` the partition key, as to partition data related to each song title.
    2. we made `UserId` the clustering key, as to sort by the users related to each song within the partition.
