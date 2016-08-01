## SQL Questions

First create a database called fringe_shows
```
  #terminal
  psql
  create database fringe_shows;
  \q
```

Populate the data using the script - fringeshows.sql
```
  #terminal
  psql -d fringe_shows -f fringeshows.sql
```

Using the SQL Database file given to you as the source of data to answer the questions.  Copy the SQL command you have used to get the answer, and paste it below the question, along with the result they gave.


## Section 1

  Revision of concepts that we've learnt in SQL today

  1. Select the names of all users.

  SELECT name FROM users;

   id |       name       
  ----+------------------
    1 | Rick Henri
    2 | Jay Chetty
    3 | Keith Douglas
    4 | Ashleigh Adams
    5 | Euan Blackledge
    6 | Chris Flint
    7 | Nico di Lillo
    8 | Joe Maher
    9 | Marie Moyles
   10 | Iain Stewart
   11 | Megan Strachan
   12 | Russell Williams
   13 | Sam Werngren
   14 | Natalie Simpson
   15 | Davide de Lerma
   16 | Josh Kearns
   17 | Renwick Drysdale
   18 | Brian Morrice
  (18 rows)



  2. Select the names of all shows that cost less than £15.

  SELECT name FROM shows WHERE shows.price>15;
                    name                   
  -----------------------------------------
   Shitfaced Shakespeare
   Camille O'Sullivan
   Game of Thrones - The Musical
   Joe Stilgoe: Songs on Film – The Sequel
   Aaabeduation – A Magic Show
   Edinburgh Royal Tattoo
   Balletronics
  (7 rows)

  3. Insert a user with the name "Val Gibson" into the users table.

  INSERT INTO "users" (name) VALUES ('Val Gibson');

   id |       name       
  ----+------------------
    1 | Rick Henri
    2 | Jay Chetty
    3 | Keith Douglas
    4 | Ashleigh Adams
    5 | Euan Blackledge
    6 | Chris Flint
    7 | Nico di Lillo
    8 | Joe Maher
    9 | Marie Moyles
   10 | Iain Stewart
   11 | Megan Strachan
   12 | Russell Williams
   13 | Sam Werngren
   14 | Natalie Simpson
   15 | Davide de Lerma
   16 | Josh Kearns
   17 | Renwick Drysdale
   18 | Brian Morrice
   19 | Val Gibson
  (19 rows)

  4. Insert a record that Val Gibson wants to attend the show "Two girls, one cup of comedy".

  INSERT INTO "shows_users" (show_id, user_id) VALUES (12, 19);

  5. Updates the name of the "Val Gibson" user to be "Valerie Gibson".

  UPDATE "users" SET name = 'Valerie Gibson' WHERE name = 'Val Gibson';

  6. Deletes the user with the name 'Valerie Gibson'.

  DELETE FROM "users" WHERE name = 'Valerie Gibson';

  7. Deletes the shows for the user you just deleted.

  DELETE FROM "shows_users" WHERE user_id = 19;


## Section 2

  This section involves more complex queries.  You will need to go and find out about aggregate funcions in SQL to answer some of the next questions.

  9. Select the names and prices of all shows, ordered by price in ascending order.

  SELECT name, price FROM "shows" ORDER BY price;

                    name                   | price 
  -----------------------------------------+-------
   Two girls, one cup of comedy            |  6.00
   Best of Burlesque                       |  7.99
   Two become One                          |  8.50
   Urinetown                               |  8.50
   Paul Dabek Mischief                     | 12.99
   Le Haggis                               | 12.99
   Joe Stilgoe: Songs on Film – The Sequel | 16.50
   Game of Thrones - The Musical           | 16.50
   Shitfaced Shakespeare                   | 16.50
   Aaabeduation – A Magic Show             | 17.99
   Camille O'Sullivan                      | 17.99
   Balletronics                            | 32.00
   Edinburgh Royal Tattoo                  | 32.99
  (13 rows)



  10. Select the average price of all shows.

  SELECT AVG(price) FROM "shows"

           avg         
  ---------------------
   15.9569230769230769
  (1 row)

  11. Select the price of the least expensive show.

  SELECT MIN(price) FROM "shows";

   min  
  ------
   6.00
  (1 row)

  12. Select the sum of the price of all shows.

  SELECT SUM(price) FROM "shows";
    sum   
  --------
   207.44
  (1 row)

  13. Select the sum of the price of all shows whose prices is less than £20.

  SELECT SUM(price) FROM "shows" WHERE price<20;
    sum   
  --------
   142.45
  (1 row)

  14. Select the name and price of the most expensive show.

  SELECT name, price FROM "shows" ORDER BY price DESC LIMIT 1;

            name          | price 
  ------------------------+-------
   Edinburgh Royal Tattoo | 32.99
  (1 row)

  15. Select the name and price of the second from cheapest show.

  SELECT name, price
  FROM (
    SELECT ROW_NUMBER() OVER (ORDER BY price ASC) AS SrNo, name, price
    FROM "shows") AS NAME
  WHERE
  SrNo = 2

        name        | price 
  -------------------+-------
   Best of Burlesque |  7.99
  (1 row)


  16. Select the names of all users whose names start with the letter "N".

  SELECT name FROM "users" WHERE name LIKE 'N%';

        name       
  -----------------
   Nico di Lillo
   Natalie Simpson
  (2 rows)



  17. Select the names of users whose names contain "er".

  SELECT name FROM "users" WHERE name LIKE '%er%';

        name       
  -----------------
   Joe Maher
   Sam Werngren
   Davide de Lerma
  (3 rows)

  SELECT name FROM "users" WHERE name LIKE '%er%';



## Section 3

  The following questions can be answered by using nested SQL statements but ideally you should learn about JOIN clauses to answer them.

  18. Select the time for the Edinburgh Royal Tattoo.

  SELECT time 
  FROM "times" 
  INNER JOIN "shows" on "shows".id = "times".show_id
  WHERE "shows".name = 'Edinburgh Royal Tattoo'; 

   time  
  -------
   22:00
  (1 row)

  19. Select the number of users who want to see "Shitfaced Shakespeare".

  SELECT count(user_id)
  FROM "shows_users"
  INNER JOIN "shows" on "shows".ID = "shows_users".show_id
  where "shows".name = 'Shitfaced Shakespeare';

   count 
  -------
       7
  (1 row)

  20. Select all of the user names and the count of shows they're going to see.

  SELECT "users".name, count("shows_users".user_id)
  FROM "shows_users"
  INNER JOIN "users" on "users".id = "shows_users".user_id
  GROUP BY "users".name;

         name       | count 
  ------------------+-------
   Chris Flint      |     4
   Euan Blackledge  |     4
   Ashleigh Adams   |     4
   Keith Douglas    |     6
   Marie Moyles     |     7
   Iain Stewart     |     6
   Natalie Simpson  |     8
   Davide de Lerma  |     7
   Joe Maher        |     5
   Russell Williams |     6
   Nico di Lillo    |     4
   Megan Strachan   |     5
   Sam Werngren     |     5
   Jay Chetty       |     5
   Rick Henri       |     5
  (15 rows)


  21. SELECT all users who are going to a show at 17:15.

  SELECT "users".*
  FROM "users"
  INNER JOIN "shows_users" on "users".id = "shows_users".user_id
  INNER JOIN "times" on "shows_users".show_id = "times".show_id
  WHERE "times".time = '17:15';

   id |       name       
  ----+------------------
    1 | Rick Henri
    3 | Keith Douglas
    5 | Euan Blackledge
    8 | Joe Maher
    9 | Marie Moyles
   13 | Sam Werngren
   15 | Davide de Lerma
   11 | Megan Strachan
   12 | Russell Williams
   13 | Sam Werngren
   10 | Iain Stewart
   14 | Natalie Simpson
  (12 rows)
