# Data Modeling with Cassandra

The second Udacity DEND project

### Python Modification

The Following was added for the column values could be extracted with the name instead of the index

```python
csvreader = csv.DictReader(f)
session.execute(query, (line['song'], line['firstName'], line['lastName']))
```


### Table Design:

#### 1. Give me the artist, song title and the song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4

```
CREATE TABLE IF NOT EXISTS artist_song_length(
sessionId int, 
itemInSession int, 
artist varchar, 
song varchar,
length float, 
PRIMARY KEY (sessionId, itemInSession));
```

The Primary Key Select was sessionId and itemInSession because those are the keys being used to access data.

##### Results: 

```
SELECT * FROM artist_song_length WHERE sessionId = 338 AND itemInSession  = 4
```


| artist        | song           | length  | iteminsession  | sessionid  |
| ------------- |:--------------:| -------:| --------------:| ----------:|
|Faithless      | Music Matters (Mark Knight Dub| 495.30731201171875 | 4 |338|


#### 2. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182

```
CREATE TABLE IF NOT EXISTS artist_song_user(
userid int,
sessionId int,
itemInSession int,
song varchar, 
firstName varchar,
lastName varchar, 
PRIMARY KEY ((userid, sessionId), itemInSession))
WITH CLUSTERING ORDER BY (itemInSession ASC);
```

Userid was used as a primary key because more than one user can have the same last and first names. Session id is part of that key as well because it is being used with userid to retrieve data.

A clustering Key was added so that we would be able to sort the data by itemInSession.

##### Results: 

```
SELECT * FROM artist_song_user WHERE  userid = 10 and sessionid = 182
ORDER BY itemInSession DESC
```

| iteminsession | userid | sessionid  | iteminsession | userid |sessionid|
| ------------- |:-------------:| -----:| -----:| -----:| -----:|
|3 | 10 | 182 | Catch You Baby (Steve Pitron & Max Sanna Radio Edit) | Sylvie | Cruz |
|2 | 10 | 182 | Kilometer | Sylvie | Cruz |
|1 | 10 | 182 | Greece 2000 | Sylvie | Cruz |
|0 | 10 | 182 | Keep On Keepin' On | Sylvie | Cruz |

 
#### 3. Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

```
CREATE TABLE IF NOT EXISTS user_song(
user_id int,
song varchar,
firstName varchar,
lastName varchar,
PRIMARY KEY (song, user_id));
```

Song was used as the primary key with userid to ensure a unique song. 

We do not have a song_id to use in case more than one song have the same name. 

##### Results: 

```
SELECT * FROM user_song WHERE Song = 'All Hands Against His Own'
```

| song | lastname | firstname  | 
| ------------- |:-------------:| -----:|
| All Hands Against His Own | Lynch | Jacqueline |
| All Hands Against His Own | Levine | Tegan |
| All Hands Against His Own | Johnson | Sara |
