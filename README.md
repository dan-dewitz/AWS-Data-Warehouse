## Moving Sparkify to AWS and the cloud
Sparkify has sucessively grown their userbase and has projected this growth to continue. Moving to the cloud and AWS allows Sparkify to easily scale their data warehouse as the app usage grows, by simply renting more machines. AWS is also a good choice for a startup as it reduces the need for an interal IT staff consisting of database and network administrators.

## Schema and Design
The database schema is a star schema with one fact table and four dimension tables. Here, the fact table is designed to indentify each time a user plays a song on the Sparkify app. With this design, we can do basic descriptive analysitcs, like summarizing the most popular artists and songs, and we can also determine what day, time, and season when users are the app the most.

Using a star schema reduces the replication of data and also allows for intuitive joins to the dimension tables. For example, if we need to update user information, we only need to update one row in the `users` table, instead of updating every row in the fact table. Similarly, the time dimenson allows analysts to easily summarize data by the hour or day of the week, making finding the most popular time and day a simple query. 

### Fact Table
The `songplays` table is the fact table.

Columns: songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent    
Primary Key: songplay_id    

A songplay is each time a user plays a song on the app. The songplay_id uniquely defines each songplay event.

### Dimension Tables
The four demension tables are defined as follows:

`users`     
Columns: user_id, first_name, last_name, gender, level     
Primary Key: user_id

`songs`    
Columns: song_id, title, artist_id, year, duration     
Primary Key: song_id     

`artists`     
Columns: artist_id, name, location, lattitude, longitude          
Primary Key: artist_id    

`time`     
Columns: start_time, hour, day, week, month, year, weekday     
Primary Key: start_time     

### Analytical Queries

##### Determine the most popular day of the week
<pre>
select      
    t.weekday as day_of_week,      
    count(songplay_id) as songplay_count     
from songplays s     
left join time t ON (s.start_time=t.start_time)     
group by day_of_week     
order by day_of_week    
</pre>

##### Determine the most popular artist

<pre>
select 
    a.name as artist_name,
    count(s.artist_id) as artist_count
from songplays s
left join artists a ON (s.artist_id=a.artist_id)
group by a.name
order by artist_count desc
limit 10;
</pre>





