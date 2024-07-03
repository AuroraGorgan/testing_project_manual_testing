<h1>Database Project for Watch and Win</h1>

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

**Application under test: Watch and Win**

**Tools used: MySQL Workbench**

Database description: The **"Watch and Win"** database is designed to manage information related to ticket purchases within a cinema network for the **Watch and Win** application, which selects winners for a free 3-month cinema subscription. The primary purpose of this database is to provide an efficient platform for tracking and managing ticket sales, as well as for analyzing data related to customer preferences and movie performance.

**<h2>General information stored:</h2>**

1.	Ticket purchase details: these include the movie ID, client name, purchase date, number of tickets purchased, movie title and genre, IMDb rating and release date.
2.	Information on available subtitles: the database also contains details about the available subtitles for each movie, including a unique identifier, subtitle name and code.
3.	Client and cinema details: customer information (such as name, email address, and phone number) is stored in a separate table, while information about cinemas, including their names and locations, is managed in a different table.
4.	Link between movies and subtitles: through the "movie_subtitles" table, the database establishes the connection between movies and the available subtitles.

<ol>
  
**<li>Database Schema</li>**
<br>
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

![image](https://github.com/AuroraGorgan/testing_project_manual_testing/assets/162451396/47af38fe-f9a3-4a1b-9442-046661121e47)


The tables are connected in the following way:

<ul>
  <li> movie_purchases_July is connected with clients through a one-to-many relationship which was implemented through clients.client_id as a primary key and movie_purchases_July.client_id as a foreign key</li>
  <li> movie_purchases_July is connected with cinemas through a one-to-many relationship which was implemented through cinemas.cinema_id as a primary key and movie_purchases_July.cinema_id as a foreign key</li>
  <li> movie_subtitles is connected with subtitles through a one-to-many relationship which was implemented through subtitles.subtitles_id as a primary key and movie_subtitles.subtitles_id as a foreign key</li>
  <li> movie_purchases_July is connected with movie_subtitles through a one-to-many relationship which was implemented through movie_id as a primary key in movie_purchases_July and movie_id as a foreign key in movie_subtitles</li>
</ul><br>

<li>Database Queries</li><br>

<ol type="a">
  <li>DDL (Data Definition Language)</li>

The following instructions were written in the scope of CREATING the structure of the database **(CREATE INSTRUCTIONS)**

<code>create database Watch_and_Win;</code>

<code>create table movie_purchases
( movie_id int primary key,
client_name varchar(100),
purchase_occasion varchar(20),
tickets float,
movie_title varchar(20),
movie_genre varchar(20),
movie_subtitles char (2),
movie_rating_IMDB float,
release_date date);</code>

<code>create table subtitles
(subtitles_id int primary key,
subtitles_name varchar (20),
subtitles_code char (2));</code>

<code>create table movie_subtitles
(movie_subtitles_id int primary key,
movie_id int,
subtitles_id int,
foreign key (movie_id) references movie_purchases (movie_id),
foreign key (subtitles_id) references subtitles (subtitles_id));</code>

<code>create table clients 
(client_id int primary key auto_increment,
client_name varchar(100) not null,
email varchar(100),
phone varchar(30));</code>

<code>create table cinemas 
(cinema_id int primary key auto_increment,
cinema_name varchar(100) not null,
location varchar(100));</code>

After the database and the tables have been created, a few **ALTER instructions** were written in order to update the structure of the database, as described below:

<code>alter table movie_purchases drop column movie_subtitles;</code>

<code>alter table movie_purchases add cinema varchar(40);</code>

<code>alter table movie_purchases modify cinema varchar(40) after movie_id;</code>

<code>alter table movie_purchases rename movie_purchases_July;</code>

<code>alter table movie_purchases_July rename column tickets to number_of_tickets;</code>

<code>alter table movie_purchases_July modify column movie_title varchar(120);</code>

<code>alter table movie_subtitles modify column movie_subtitles_id int auto_increment;</code>

<code>alter table movie_purchases_July add client_id INT, add cinema_id INT;</code>

<code>alter table movie_purchases_July add constraint fk_client foreign key (client_id) 
references clients (client_id),
add constraint fk_cinema foreign key (cinema_id) references cinemas (cinema_id);</code>
  
  <li>DML (Data Manipulation Language)</li>

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the **insert instructions** that were created in the scope of this project:

<code>insert into movie_purchases_July (movie_id, cinema, client_name, purchase_occasion, number_of_tickets, movie_title, movie_genre, 
movie_rating_IMDB, release_date) values ('1','Dacia','John Anderson','date', 2, 'The Avengers', 'Action', '8.0', '2012-05-14'), 
('2','Victoria','Sujin Lee','fun', 5,'The Conjuring','Horror','7.5', '2013-07-19'),
('3', 'Cinema City Vivo','Rachel DeSantos','date', 2, 'Despicable Me', 'Animation', '7.6', '2010-07-09');
select * from movie_purchases_July;</code>

<code>insert into movie_purchases_July (movie_id, cinema, client_name, purchase_occasion, number_of_tickets, movie_title, movie_genre, 
movie_rating_IMDB, release_date) values ('4','Cinema City Iulius','Kim Soo-hyun','date', 2, 'The Dark Knight Rises', 'Action', '8.4', '2012-07-20');</code>

<code>insert into movie_purchases_July values ('5','Florin Piersic','Jane Morningstar','fun', 6, '21 Jump Street', 'Comedy', '7.2', '2012-03-16');</code>

<code>insert into movie_purchases_July (movie_id, cinema, client_name, purchase_occasion, number_of_tickets, movie_title, movie_genre)
values ('6','Cinema City Iulius','Patrik Green','fun', 12, 'The Watch', 'Comedy');
select * from movie_purchases_July;</code>

<code>insert into subtitles (subtitles_id, subtitles_name, subtitles_code)
values ('01', 'English', 'EN'), ('02', 'Spanish', 'SP'), ('03', 'Romanian', 'RO'), ('04', 'French', 'FR'), ('05', 'Korean', 'KO');
select * from subtitles;</code>

<code>insert into movie_subtitles (movie_id, subtitles_id)
values (1, '01'), (2, '02'), (4, '03'), (5, '04'), (3, '05');
select * from movie_subtitles;</code>

<code>insert into clients (client_name, email, phone) values ('John Anderson', 'john.anderson@gmail.com', '123-456-7890'),
('Sujin Lee', 'sujin.lee@gmail.com', '234-567-8901'), ('Rachel DeSantos', 'rachel.desantos@gmail.com', '345-678-9012'),
('Kim Soo-hyun', 'kim.soohyun@gmail.com', '456-789-0123'), ('Jane Morningstar', 'jane.morningstar@gmail.com', '567-890-1234'),
('Patrik Green', 'patrik.green@gmail.com', '678-901-2345');</code>

<code>insert into cinemas (cinema_name, location)
values ('Dacia', 'Location 1'), ('Victoria', 'Location 2'), ('Cinema City Vivo', 'Location 3'),
('Cinema City Iulius', 'Location 4'), ('Florin Piersic', 'Location 5');</code>

After the insert, in order to prepare the data to be better suited for the testing process, I **updated** some data in the following way:

<code>update movie_purchases_July
set client_id = (select client_id from clients where client_name = movie_purchases_July.client_name),
cinema_id = (select cinema_id from cinemas where cinema_name = movie_purchases_July.cinema);</code>

<code>alter table movie_purchases_July
drop column client_name,
drop column cinema;
select * from clients;
select * from cinemas;
select * from movie_purchases_July;</code>

<code>update movie_purchases_July
set number_of_tickets = 10
where movie_title = 'The Conjuring';</code>

<code>update movie_purchases_July
set movie_genre = 'Action Comedy'
where movie_id = 5;</code>

<code>update movie_purchases_July
set movie_rating_IMDB = movie_rating_IMDB + 0.1
where year (release_date) = 2012;
select * from movie_purchases_July;</code>

  <li>DQL (Data Query Language)</li>

After the testing process, I **deleted** the data that was no longer relevant in order to preserve the database clean: 

<code>select client_id from clients where client_name = 'John Anderson';
select movie_id from movie_purchases_July where client_id = 1;
delete from movie_subtitles where movie_id in (select movie_id from movie_purchases_July where client_id = 1);
delete from movie_purchases_July WHERE client_id = 1;
select * from movie_purchases_July;</code>

<code>select movie_id from movie_purchases_July where month(release_date) != 7;
delete from movie_subtitles 
where movie_id in (select movie_id from movie_purchases_July where month(release_date) != 7);
delete from movie_purchases_July 
where month(release_date) != 7;
select * from movie_purchases_July;</code>

In order to simulate various scenarios that might happen in real life I created the following **queries** that would cover multiple potential real-life situations:

<code>select * from movie_purchases_July
where movie_genre = 'Action';</code>

<code>select * from movie_purchases_July
where movie_genre = 'Animation';</code>

<code>select * from movie_purchases_July
where movie_genre = 'Comedy';</code>

<code>select * from movie_purchases_July
where movie_genre = 'Action' and year (release_date) = 2012;</code>

<code>select * from movie_purchases_July
where movie_genre = 'Comedy' and number_of_tickets >9;</code>

<code>select * from movie_purchases_July
where movie_genre = 'Action' or movie_genre = 'Comedy' or movie_genre = 'Animation';</code>

<code>select * from movie_purchases_July
where not movie_genre = 'Action';</code>

<code>select * from movie_purchases_July
where movie_title LIKE 'The%';</code>

<code>select mp.movie_id, mp.movie_title, s.subtitles_name
from movie_purchases_July mp
inner join movie_subtitles ms on mp.movie_id = ms.movie_id
inner join subtitles s on ms.subtitles_id = s.subtitles_id;</code>

<code>select mp.movie_id, mp.movie_title, s.subtitles_name
from movie_purchases_July mp
left join movie_subtitles ms on mp.movie_id = ms.movie_id
left join subtitles s on ms.subtitles_id = s.subtitles_id;</code>

<code>select mp.movie_id, mp.movie_title, s.subtitles_name
from movie_purchases_July mp
right join movie_subtitles ms on mp.movie_id = ms.movie_id
right join subtitles s on ms.subtitles_id = s.subtitles_id;</code>

<code>select mp.movie_id, mp.movie_title, s.subtitles_name
from movie_purchases_July mp
cross join subtitles s;</code>

<code>select AVG(number_of_tickets) as avg_tickets
from movie_purchases_July;</code>

<code>select MIN(number_of_tickets) as min_tickets
from movie_purchases_July;</code>

<code>select MAX(number_of_tickets) as max_tickets
from movie_purchases_July;
select SUM(number_of_tickets) as total_tickets
from movie_purchases_July;</code>

<code>select COUNT(*) as total_records
from movie_purchases_July;</code>

<code>select c.client_name,
       COUNT(*) as total_purchases,
       SUM(mpj.number_of_tickets) as total_tickets_purchased
from movie_purchases_July mpj
join clients c on mpj.client_id = c.client_id
group by c.client_name;</code>

<code>select ci.cinema_name,
COUNT(*) as total_purchases,
SUM(mpj.number_of_tickets) as total_tickets_purchased
from movie_purchases_July mpj
join cinemas ci on mpj.cinema_id = ci.cinema_id
group by ci.cinema_name;</code>

<code>select mpj.movie_genre, c.client_name, SUM(mpj.number_of_tickets) as total_tickets
from movie_purchases_July mpj
join clients c on mpj.client_id = c.client_id
group by mpj.movie_genre, c.client_name
having total_tickets > 2;</code>

<code>select * from movie_purchases_July
where movie_rating_IMDB = (select MAX(movie_rating_IMDB) from movie_purchases_July);</code>
<br>

</ol>

<li>Conclusions</li>

In summary, this task provided an opportunity to delve into relational database concepts, reinforcing the importance of clear design and understanding relationships between tables. Through this experience, I gained practical insights into database management and the importance of thoughtful schema design.

</ol>
