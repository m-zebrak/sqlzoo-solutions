# sqlzoo-solutions

My solutions to all of the main 9 and 4 additional topics posted on the [sqlzoo.net](http://sqlzoo.net/)  tutorial.

All answers were :100: percent correct and generated correct feedback on the day they were uploaded.

Feel free to use and if u have any suggestions don't hesitate to contact me . :smiley:

## Sections:

1. [SELECT basics](#select-basics)
2. [SELECT from world](#select-from-world)
3. [SELECT from nobel](#select-from-nobel)
4. [SELECT in SELECT](#select-in-select)
5. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)
7. [More JOIN](#more-join)
8. [Using NULL](#using-null)
9. [Self JOIN](#self-join)

Additionally:

10. [SELECT names](#select-names)
11. [Numeric examples](#numeric-examples)
12. [Window functions](#window-functions)
13. [COVID 19](#covid-19)

## SELECT basics

1. The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data)
   should be in 'single quotes'; Modify it to show the population of Germany

```sql
SELECT population
FROM world
WHERE name = 'Germany';
```

2. Checking a list: The word IN allows us to check if an item is in a list. The example shows the name and population
   for the countries 'Brazil', 'Russia', 'India' and 'China'. Show the name and the population for 'Sweden', 'Norway'
   and 'Denmark'.

```sql
SELECT name, population
FROM world
WHERE name IN ('Sweden', 'Norway', 'Denmark');
```

3. Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of
   boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the
   country and the area for countries with an area between 200,000 and 250,000.

```sql
SELECT name, area
FROM world
WHERE area BETWEEN 200000 AND 250000;
```

## SELECT from world

1. Read the notes about this table. Observe the result of running this SQL command to show the name, continent and
   population of all countries.

```sql
SELECT name, continent, population
FROM world;
```

2. How to use WHERE to filter records. Show the name for the countries that have a population of at least 200 million.
   200 million is 200000000, there are eight zeros.

```sql
SELECT name
FROM world
WHERE population > 200000000;
```

3. Give the name and the per capita GDP for those countries with a population of at least 200 million.

```sql
SELECT name, gdp / population
FROM world
WHERE population >= 200000000;
```

4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by
   1000000 to get population in millions.

```sql
SELECT name, population / 1000000
FROM world
WHERE continent = 'South America';
```

5. Show the name and population for France, Germany, Italy

```sql
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');
```

6. Show the countries which have a name that includes the word 'United'

```sql
SELECT name
FROM world
WHERE name LIKE '%United%';
```

7. Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more
   than 250 million. Show the countries that are big by area or big by population. Show name, population and area.

```sql
SELECT name, population, area
FROM world
WHERE area > 3000000
   OR population > 250000000;
```

8. Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250
   million) but not both. Show name, population and area.

    - Australia has a big area but a small population, it should be included.
    - Indonesia has a big population but a small area, it should be included.
    - China has a big population and big area, it should be excluded. United Kingdom has a small population and a small
      area, it should be excluded.

```sql
SELECT name, population, area
FROM world
WHERE ((population > 250000000) OR (area > 3000000))
  AND NOT ((population > 250000000) AND (area > 3000000));
```

    COMMENT: 'XOR' operator is not supported in sqlzoo.net interpreter

9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'.
   Use the ROUND function to show the values to two decimal places. For South America show population in millions and
   GDP in billions both to 2 decimal places.

```sql
SELECT name, ROUND(population / 1000000, 2), ROUND(gdp / 1000000000, 2)
FROM world
WHERE continent = 'South America';
```

10. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12
    zeros). Round this value to the nearest 1000. Show per-capita GDP for the trillion dollar countries to the nearest
    $1000.

```sql
SELECT name, ROUND((gdp / population), -3)
FROM world
WHERE gdp >= 1000000000000;
```

11. Greece has capital Athens. Each of the strings 'Greece', and 'Athens' has 6 characters. Show the name and capital
    where the name and the capital have the same number of characters.

    - You can use the LENGTH function to find the number of characters in a string

```sql
SELECT name, capital
FROM world
WHERE LENGTH(name) = LENGTH(capital);
```

12. The capital of Sweden is Stockholm. Both words start with the letter 'S'. Show the name and the capital where the
    first letters of each match. Don't include countries where the name and the capital are the same word.

    - You can use the function LEFT to isolate the first character.
    - You can use <> as the NOT EQUALS operator.

```sql
SELECT name, capital
FROM world
WHERE (LEFT(name,1) = LEFT (capital,1))
  AND (name <> capital);
```

13. Equatorial Guinea and Dominican Republic have all of the vowels (a e i o u) in the name. They don't count because
    they have more than one word in the name. Find the country that has all the vowels and no spaces in its name.

    - You can use the phrase name NOT LIKE '%a%' to exclude characters from your results.
    - The query shown misses countries like Bahamas and Belarus because they contain at least one 'a'

```sql
SELECT name
FROM world
WHERE name LIKE '%a%'
  AND name LIKE '%e%'
  AND name LIKE '%i%'
  AND name LIKE '%o%'
  AND name LIKE '%u%'
  AND name NOT LIKE '% %';
```

## SELECT from nobel

1. Change the query shown so that it displays Nobel prizes for 1950.

```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950;
```

2. Show who won the 1962 prize for Literature.

```sql
SELECT winner
FROM nobel
WHERE yr = 1962
  AND subject = 'Literature';
```

3. Show the year and subject that won 'Albert Einstein' his prize.

```sql
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein';
```

4. Give the name of the 'Peace' winners since the year 2000, including 2000.

```sql
SELECT winner
FROM nobel
WHERE subject = 'Peace'
  AND yr >= 2000;
```

5. Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.

```sql
SELECT winner
FROM nobel
WHERE subject = 'Peace'
  AND yr >= 2000;
```

6. Show all details of the presidential winners:

    - Theodore Roosevelt
    - Woodrow Wilson
    - Jimmy Carter
    - Barack Obama

```sql
SELECT *
FROM nobel
WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter', 'Barack Obama');
```

7. Show the winners with first name John

```sql
SELECT winner
FROM nobel
WHERE winner LIKE 'John%';
```

8. Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984.

```sql
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'Physics' AND yr = 1980
   OR subject = 'Chemistry' AND yr = 1984;
```

9. Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine

```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1980
  AND subject NOT IN ('Chemistry', 'Medicine');
```

10. Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910)
    together with winners of a 'Literature' prize in a later year (after 2004, including 2004)

```sql
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'Medicine' AND yr < 1910
   OR subject = 'Literature' AND yr >= 2004;
```

11. Find all details of the prize won by PETER GRÜNBERG

```sql
SELECT *
FROM nobel
WHERE winner = 'PETER GRÜNBERG';
```

12. Find all details of the prize won by EUGENE O'NEILL

```sql
SELECT *
FROM nobel
WHERE winner = 'EUGENE O''NEILL';
```

13. Knights in order. List the winners, year and subject where the winner starts with Sir. Show the the most recent
    first, then by name order.

```sql
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner;
```

14. The expression subject IN ('Chemistry','Physics') can be used as a value - it will be 0 or 1. Show the 1984 winners
    and subject ordered by subject and winner name; but list Chemistry and Physics last.

```sql
SELECT winner, subject
FROM nobel
WHERE yr = 1984
ORDER BY CASE WHEN subject IN ('Physics', 'Chemistry') THEN 1 ELSE 0 END, subject, winner;
```

## SELECT in SELECT

1. List each country name where the population is larger than that of 'Russia'.

```sql
SELECT name
FROM world
WHERE population >
      (SELECT population
       FROM world
       WHERE name = 'Russia');
```

2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.

```sql
SELECT name
FROM world
WHERE continent = 'Europe'
  AND gdp / population >
      (SELECT gdp / population
       FROM world
       WHERE name = 'United Kingdom');
```

3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of
   the country.

```sql
SELECT name, continent
FROM world
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))
ORDER BY name;
```

4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.

```sql
SELECT name, population
FROM world
WHERE population > (SELECT population FROM world WHERE name = 'Canada')
  AND population < (SELECT population FROM world WHERE name = 'Poland')
```

5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5
   million) has 11% of the population of Germany. Show the name and the population of each country in Europe. Show the
   population as a percentage of the population of Germany. The format should be Name, Percentage

```sql
SELECT name,
       CONCAT(CONVERT(DECIMAL, population / (SELECT population FROM world WHERE name = 'Germany') * 100),
              '%') AS percentage
FROM world
WHERE continent = 'Europe';
```

    COMMENT: ROUND(x, 0) gives a lot of '0' after the decimal point you can get rid of by using 
             additional CAST(x as INT) on it, but I think that the solution above is more transparent. 
             Even that, I also present the solution below:

```sql
SELECT name,
       CONCAT(CAST(ROUND((population / (SELECT population FROM world WHERE name = 'Germany') * 100), 0) AS INT),
              '%') AS percentage
FROM world
WHERE continent = 'Europe';
```

6. Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL
   gdp values)

```sql
SELECT name
FROM world
WHERE gdp > ALL (SELECT gdp FROM world WHERE gdp > 0 AND continent = 'Europe');
```

7. Find the largest country (by area) in each continent, show the continent, the name and the area:

```sql
SELECT continent, name, area
FROM world x
WHERE area >= ALL (SELECT area FROM world y WHERE y.continent = x.continent AND area > 0);
```

8. List each continent and the name of the country that comes first alphabetically.

```sql
SELECT continent, name
FROM world x
WHERE name <= ALL (SELECT name FROM world y WHERE x.continent = y.continent);
```

9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries
   associated with these continents. Show name, continent and population.

```sql
SELECT name, continent, population
FROM world x
WHERE 25000000 > ALL (SELECT population FROM world y WHERE x.continent = y.continent);
```

10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give
    the countries and continents.

```sql
SELECT name, continent
FROM world x
WHERE population > ALL (SELECT 3 * population FROM world y WHERE x.continent = y.continent AND x.name <> y.name);
```

## SUM and COUNT

1. Show the total population of the world.

```sql
SELECT SUM(population)
FROM world;
```

2. List all the continents - just once each.

```sql
SELECT DISTINCT continent
FROM world;
```

3. Give the total GDP of Africa

```sql
SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa';
```

4. How many countries have an area of at least 1000000

```sql
SELECT COUNT(*)
FROM world
WHERE area > 1000000;
```

5. What is the total population of ('Estonia', 'Latvia', 'Lithuania')

```sql
SELECT SUM(population)
FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania');
```

6. For each continent show the continent and number of countries.

```sql
SELECT continent, COUNT(*)
FROM world
GROUP BY continent;
```

7. For each continent show the continent and number of countries with populations of at least 10 million.

```sql
SELECT continent, COUNT(*)
FROM world
WHERE population >= 10000000
GROUP BY continent; 
```

8. List the continents that have a total population of at least 100 million.

```sql
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000;
```

## JOIN

1. The first example shows the goal scored by a player with the last name 'Bender'. The * says to list all the columns
   in the table - a shorter way of saying matchid, teamid, player, gtime. Modify it to show the matchid and player name
   for all goals scored by Germany. To identify German players, check for: teamid = 'GER'

```sql
SELECT matchid, player
FROM goal
WHERE teamid = 'GER';
```

2. From the previous query you can see that Lars Bender's scored a goal in game 1012. Now we want to know what teams
   were playing in that match. Notice that the matchid column in the goal table corresponds to the id column in the game
   table. We can look up information about game 1012 by finding that row in the game table. Show id, stadium, team1,
   team2 for just game 1012

```sql
SELECT id, stadium, team1, team2
FROM game
WHERE id = 1012;
```

3. You can combine the two steps into a single query with a JOIN.

        SELECT * FROM game JOIN goal ON (id = matchid)

   The FROM clause says to merge data from the goal table with that from the game table. The ON says how to figure out
   which rows in game go with which rows in goal - the matchid from goal must match id from game. (If we wanted to be
   more clear/specific we could say ON (game.id=goal.matchid). The code below shows the player (from the goal) and
   stadium name (from the game table) for every goal scored. Modify it to show the player, teamid, stadium and mdate for
   every German goal.

```sql
SELECT player, teamid, stadium, mdate
FROM game
         JOIN goal ON (id = matchid)
WHERE teamid = 'GER';
```

4. Use the same JOIN as in the previous question. Show the team1, team2 and player for every goal scored by a player
   called Mario player LIKE 'Mario%'

```sql
SELECT team1, team2, player
FROM game
         JOIN goal ON (id = matchid)
WHERE player LIKE 'Mario%';
```

5. The table eteam gives details of every national team including the coach. You can JOIN goal to eteam using the phrase
   goal JOIN eteam on teamid=id. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<
   =10

```sql
SELECT player, teamid, coach, gtime
FROM goal
         JOIN eteam ON (teamid = id)
WHERE gtime <= 10;
```

6. To JOIN game with eteam you could use either game JOIN eteam ON (team1=eteam.id) or game JOIN eteam ON (
   team2=eteam.id). Notice that because id is a column name in both game and eteam you must specify eteam.id instead of
   just id. List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.

```sql
SELECT mdate, teamname
FROM game
         JOIN eteam ON (team1 = eteam.id)
WHERE coach = 'Fernando Santos';
```

7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'

```sql
SELECT player
FROM goal
         JOIN game ON (id = matchid)
WHERE stadium = 'National Stadium, Warsaw';
```

8. The example query shows all goals scored in the Germany-Greece quarterfinal. Instead show the name of all players who
   scored a goal against Germany.

```sql
SELECT DISTINCT player
FROM game
         JOIN goal ON (id = matchid)
WHERE (team1 = 'GER' OR team2 = 'GER')
  AND teamid != 'GER';
```

9. Show teamname and the total number of goals scored.

```sql
SELECT teamname, COUNT(*)
FROM eteam
         JOIN goal ON (id = teamid)
GROUP BY teamname;
```

10. Show the stadium and the number of goals scored in each stadium.

```sql
SELECT stadium, COUNT(*)
FROM game
         JOIN goal ON (id = matchid)
GROUP BY stadium;
```

11. For every match involving 'POL', show the matchid, date and the number of goals scored.

```sql
SELECT matchid, mdate, COUNT(*)
FROM game
         JOIN goal ON (id = matchid)
WHERE team1 = 'POL'
   OR team2 = 'POL'
GROUP BY mdate, matchid;
```

12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'

```sql
SELECT matchid, mdate, COUNT(*)
FROM goal
         JOIN game ON (matchid = id)
WHERE teamid = 'GER'
GROUP BY matchid, mdate;
```

13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained
    in any previous exercises. Notice in the query given every goal is listed. If it was a team1 goal then a 1 appears
    in score1, otherwise there is a 0. You could SUM this column to get a count of the goals scored by team1. Sort your
    result by mdate, matchid, team1 and team2.

```sql
SELECT mdate,
       team1,
       SUM(CASE WHEN teamid = team1 THEN 1 ELSE 0 END) AS score1,
       team2,
       SUM(CASE WHEN teamid = team2 THEN 1 ELSE 0 END) AS score2
FROM game
         LEFT JOIN goal ON (id = matchid)
GROUP BY mdate, matchid, team1, team2
ORDER BY mdate, matchid, team1, team2;
```

## More JOIN

1. List the films where the yr is 1962 [Show id, title]

```sql
SELECT id, title
FROM movie
WHERE yr = 1962;
```

2. Give year of 'Citizen Kane'.

```sql
SELECT yr
FROM movie
WHERE title = 'Citizen Kane';
```

3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in
   the title). Order results by year.

```sql
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr;
```

4. What id number does the actor 'Glenn Close' have?

```sql
SELECT id
FROM actor
WHERE name = 'Glenn Close';
```

5. What is the id of the film 'Casablanca'

```sql
SELECT id
FROM movie
WHERE title = 'Casablanca';
```

6. Obtain the cast list for 'Casablanca'. Use movieid=11768, (or whatever value you got from the previous question)

```sql
SELECT name
FROM casting
         JOIN actor ON (actorid = id)
WHERE movieid = 27;
```

7. Obtain the cast list for the film 'Alien'

```sql
SELECT name
FROM casting
         JOIN actor ON (actorid = actor.id)
         JOIN movie ON (movieid = movie.id)
WHERE title = 'Alien';
```

8. List the films in which 'Harrison Ford' has appeared

```sql
SELECT title
FROM casting
         JOIN movie ON (movieid = movie.id)
         JOIN actor ON (actorid = actor.id)
WHERE name = 'Harrison Ford';
```

9. List the films where 'Harrison Ford' has appeared - but not in the starring
   role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]

```sql
SELECT title
FROM casting
         JOIN movie ON (movieid = movie.id)
         JOIN actor ON (actorid = actor.id)
WHERE name = 'Harrison Ford'
  AND ord != 1;
```

10. List the films together with the leading star for all 1962 films.

```sql
SELECT title, name
FROM casting
         JOIN movie ON (movieid = movie.id)
         JOIN actor ON (actorid = actor.id)
WHERE ord = 1
  AND yr = 1962;
```

11. Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any
    year in which he made more than 2 movies.

```sql
SELECT yr, COUNT(*)
FROM movie
         JOIN casting ON (movie.id = movieid)
         JOIN actor ON (actorid = actor.id)
WHERE name = 'Rock Hudson'
GROUP BY yr
HAVING COUNT(*) > 2;
```

12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.

```sql
SELECT title, name
FROM casting
         JOIN movie ON (movieid = movie.id)
         JOIN actor ON (actorid = actor.id)
WHERE ord = 1
  AND movie.id IN
      (SELECT movie.id
       FROM movie
                JOIN casting ON (movie.id = movieid)
                JOIN actor ON (actorid = actor.id)
       WHERE actor.name = 'Julie Andrews');
```

13. Obtain a list, in alphabetical order, of actors who've had at least 15 starring roles.

```sql
SELECT name
FROM casting
         JOIN actor ON (actorid = actor.id)
WHERE ord = 1
GROUP BY name
HAVING COUNT(*) >= 15
ORDER BY name;
```

14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.

```sql
SELECT title, COUNT(*)
FROM casting
         JOIN movie ON (movieid = movie.id)
WHERE yr = 1978
GROUP BY movieid, title
ORDER BY COUNT(*) DESC, title;
```

15. List all the people who have worked with 'Art Garfunkel'.

```sql
SELECT DISTINCT name
FROM actor
         JOIN casting ON (id = actorid)
WHERE name != 'Art Garfunkel'
  AND movieid IN (SELECT movieid
                  FROM casting
                           JOIN actor ON (actorid = id)
                  WHERE name = 'Art Garfunkel');
```

## Using NULL

1. List the teachers who have NULL for their department.

```sql
SELECT name
FROM teacher
WHERE dept IS NULL;
```

2. Note the INNER JOIN misses the teachers with no department and the departments with no teacher.

```sql
SELECT teacher.name, dept.name
FROM teacher
         INNER JOIN dept ON (dept = dept.id);
```

3. Use a different JOIN so that all teachers are listed.

```sql
SELECT teacher.name, dept.name
FROM teacher
         LEFT JOIN dept ON (dept = dept.id);
```

4. Use a different JOIN so that all departments are listed.

```sql
SELECT teacher.name, dept.name
FROM teacher
         RIGHT JOIN dept ON (dept = dept.id);
```

5. Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given. Show teacher
   name and mobile number or '07986 444 2266'

```sql
SELECT name, COALESCE(mobile, '07986 444 2266')
FROM teacher;
```

6. Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where
   there is no department.

```sql
SELECT teacher.name, COALESCE(dept.name, 'None')
FROM teacher
         LEFT JOIN dept ON (dept = dept.id);
```

7. Use COUNT to show the number of teachers and the number of mobile phones.

```sql
SELECT COUNT(name), COUNT(mobile)
FROM teacher;
```

8. Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the
   Engineering department is listed.

```sql
SELECT dept.name, COUNT(teacher.name)
FROM teacher
         RIGHT JOIN dept ON (dept = dept.id)
GROUP BY dept.name;
```

9. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2 and 'Art' otherwise.

```sql
SELECT name,
       CASE WHEN dept IN (1, 2) THEN 'Sci' ELSE 'Art' END
FROM teacher;
```

10. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the
    teacher's dept is 3 and 'None' otherwise.

```sql
SELECT name,
       CASE WHEN dept IN (1, 2) THEN 'Sci' WHEN dept = 3 THEN 'Art' ELSE 'None' END
FROM teacher;
```

## Self JOIN

1. How many stops are in the database.

```sql
SELECT COUNT(*)
FROM stops;
```

2. Find the id value for the stop 'Craiglockhart'

```sql
SELECT id
FROM stops
WHERE name = 'Craiglockhart';
```

3. Give the id and the name for the stops on the '4' 'LRT' service.

```sql
SELECT id, name
FROM stops
         JOIN route ON (id = stop)
WHERE num = '4'
  AND company = 'LRT';
```

4. The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query
   and notice the two services that link these stops have a count of 2. Add a HAVING clause to restrict the output to
   these two routes.

```sql
SELECT company, num, COUNT(*)
FROM route
WHERE stop = 149
   OR stop = 53
GROUP BY company, num
HAVING COUNT(*) = 2;
```

5. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without
   changing routes. Change the query so that it shows the services from Craiglockhart to London Road.

```sql
SELECT a.company, a.num, a.stop, b.stop
FROM route AS a
         JOIN route AS b ON (a.num = b.num)
WHERE a.stop = 53
  AND b.stop = 149;
```

6. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to
   stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road'
   are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'

```sql
SELECT a.company, a.num, stopa.name, stopb.name
FROM route AS a
         JOIN route AS b ON (a.num = b.num)
         JOIN stops AS stopa ON (a.stop = stopa.id)
         JOIN stops AS stopb ON (b.stop = stopb.id)
WHERE stopa.name = 'Craiglockhart'
  AND stopb.name = 'London Road';
```

7. Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')

```sql
SELECT DISTINCT a.company, a.num
FROM route AS a
         JOIN route AS b ON (a.num = b.num)
WHERE a.stop = 115
  AND b.stop = 137;
```

8. Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'

```sql
SELECT a.company, a.num
FROM route AS a
         JOIN route AS b ON (a.num = b.num)
         JOIN stops AS stopa ON (a.stop = stopa.id)
         JOIN stops AS stopb ON (b.stop = stopb.id)
WHERE stopa.name = 'Craiglockhart'
  AND stopb.name = 'Tollcross';
```

9. Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including '
   Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.

```sql
SELECT DISTINCT stopb.name, b.company, b.num
FROM route AS a
         JOIN route AS b ON (a.num = b.num)
         JOIN stops AS stopa ON (a.stop = stopa.id)
         JOIN stops AS stopb ON (b.stop = stopb.id)
WHERE a.company = b.company
  AND stopa.name = 'Craiglockhart';
```

10. Find the routes involving two buses that can go from Craiglockhart to Lochend. Show the bus no. and company for the
    first bus, the name of the stop for the transfer, and the bus no. and company for the second bus.

```sql
SELECT a.num, a.company, stopb.name, c.num, c.company
FROM route AS a
         JOIN route AS b ON (a.company = b.company)
         JOIN route AS c ON (b.stop = c.stop)
         JOIN route AS d ON (c.company = d.company)
         JOIN stops AS stopa ON (a.stop = stopa.id)
         JOIN stops AS stopb ON (b.stop = stopb.id)
         JOIN stops AS stopc ON (c.stop = stopc.id)
         JOIN stops AS stopd ON (d.stop = stopd.id)
WHERE a.num = b.num
  AND c.num = d.num
  AND stopb.id = stopc.id
  AND stopa.name = 'Craiglockhart'
  AND stopd.name = 'Lochend';
```

## SELECT names

1.Find the country that start with Y

```sql
SELECT name
FROM world
WHERE name LIKE 'Y%';
```

2. Find the countries that end with y

```sql
SELECT name
FROM world
WHERE name LIKE '%y';
```

3. Find the countries that contain the letter x

```sql
SELECT name
FROM world
WHERE name LIKE '%x%';
```

4. Find the countries that end with land

```sql
SELECT name
FROM world
WHERE name LIKE '%land';
```

5. Find the countries that start with C and end with ia

```sql
SELECT name
FROM world
WHERE name LIKE 'C%ia';
```

6. Find the country that has oo in the name

```sql
SELECT name
FROM world
WHERE name LIKE '%oo%'
```

7. Find the countries that have three or more a in the name

```sql
SELECT name
FROM world
WHERE name LIKE '%a%a%a%';
```

8. Find the countries that have "t" as the second character.

```sql
SELECT name
FROM world
WHERE name LIKE '_t%';
```

9. Find the countries that have two "o" characters separated by two others.

```sql
SELECT name
FROM world
WHERE name LIKE '%o__o%';
```

10. Find the countries that have exactly four characters.

```sql
SELECT name
FROM world
WHERE name LIKE '____';
```

11. Find the country where the name is the capital city.

```sql
SELECT name
FROM world
WHERE name = capital;
```

12. Find the country where the capital is the country plus "City".

```sql
SELECT name
FROM world
WHERE capital = CONCAT(name, ' City');
```

13. Find the capital and the name where the capital includes the name of the country.

```sql
SELECT capital, name
FROM world
WHERE capital LIKE CONCAT('%', name, '%');
```

14. Find the capital and the name where the capital is an extension of name of the country.

```sql
SELECT capital, name
FROM world
WHERE capital LIKE CONCAT(name, '%')
  AND capital <> name;
```

15. Show the name and the extension where the capital is an extension of name of the country.

```sql
SELECT name, REPLACE(capital, name, '')
FROM world
WHERE capital LIKE CONCAT(name, '%')
  AND capital <> name;
```

## Numeric examples

1. The example shows the number who responded for:
    - question 1
    - at 'Edinburgh Napier University'
    - studying '(8) Computer Science'

   Show the the percentage who STRONGLY AGREE

```sql
SELECT A_STRONGLY_AGREE
FROM nss
WHERE question = 'Q01'
  AND institution = 'Edinburgh Napier University'
  AND subject = '(8) Computer Science';
```

2. Show the institution and subject where the score is at least 100 for question 15.

```sql
SELECT institution, subject
FROM nss
WHERE score >= 100
  AND question = 'Q15';
```

3. Show the institution and score where the score for '(8) Computer Science' is less than 50 for question 'Q15'

```sql
SELECT institution, score
FROM nss
WHERE question = 'Q15'
  AND score < 50
  AND subject = '(8) Computer Science';
```

4. Show the subject and total number of students who responded to question 22 for each of the subjects '(8) Computer
   Science' and '(H) Creative Arts and Design'.

```sql
SELECT subject, SUM(response)
FROM nss
WHERE question = 'Q22'
  AND subject IN ('(8) Computer Science', '(H) Creative Arts and Design')
GROUP BY subject;
```

5. Show the subject and total number of students who A_STRONGLY_AGREE to question 22 for each of the subjects '(8)
   Computer Science' and '(H) Creative Arts and Design'.

```sql
SELECT subject,
       SUM(response * A_STRONGLY_AGREE / 100)
FROM nss
WHERE question = 'Q22'
  AND subject IN ('(8) Computer Science', '(H) Creative Arts and Design')
GROUP BY subject;
```

6. Show the percentage of students who A_STRONGLY_AGREE to question 22 for the subject '(8) Computer Science' show the
   same figure for the subject '(H) Creative Arts and Design'. Use the ROUND function to show the percentage without
   decimal places.

```sql
SELECT subject,
       ROUND(SUM(response * A_STRONGLY_AGREE) / SUM(response), 0)
FROM nss
WHERE question = 'Q22'
  AND subject IN ('(8) Computer Science', '(H) Creative Arts and Design')
GROUP BY subject;
```

7. Show the average scores for question 'Q22' for each institution that include 'Manchester' in the name. The column
   score is a percentage - you must use the method outlined above to multiply the percentage by the response and divide
   by the total response. Give your answer rounded to the nearest whole number.

```sql
SELECT institution,
       ROUND(SUM(response * score) / SUM(response), 0)
FROM nss
WHERE question = 'Q22'
  AND institution LIKE '%Manchester%'
GROUP BY institution;
```

8. Show the institution, the total sample size and the number of computing students for institutions in Manchester for '
   Q01'.

```sql
SELECT institution,
       SUM(sample),
       SUM(CASE WHEN subject = '(8) Computer Science' THEN sample ELSE 0 END)
FROM nss
WHERE question = 'Q01'
  AND institution LIKE '%Manchester%'
GROUP BY institution;
```

## Window functions

1. Show the lastName, party and votes for the constituency 'S14000024' in 2017.

```sql
SELECT lastName, party, votes
FROM ge
WHERE constituency = 'S14000024'
  AND yr = 2017
ORDER BY votes DESC;
```

2. Show the party and RANK for constituency S14000024 in 2017. List the output by party

```sql
SELECT party,
       votes,
       RANK() OVER (ORDER BY votes DESC) as posn
FROM ge
WHERE constituency = 'S14000024 '
  AND yr = 2017
ORDER BY party;
```

3. Use PARTITION to show the ranking of each party in S14000021 in each year. Include yr, party, votes and ranking (the
   party with the most votes is 1).

```sql
SELECT yr,
       party,
       votes,
       RANK() OVER (PARTITION BY yr ORDER BY votes DESC) as posn
FROM ge
WHERE constituency = 'S14000021'
ORDER BY party, yr;
```

4. Use PARTITION BY constituency to show the ranking of each party in Edinburgh in 2017. Order your results so the
   winners are shown first, then ordered by constituency.

```sql
SELECT constituency,
       party,
       votes,
       RANK() OVER (PARTITION BY constituency ORDER BY votes desc) as posn
FROM ge
WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
  AND yr = 2017
ORDER BY posn, constituency;
```

5. Show the parties that won for each Edinburgh constituency in 2017.

```sql
SELECT constituency, party
FROM (SELECT constituency,
             party,
             votes,
             RANK() OVER (PARTITION BY constituency ORDER BY votes DESC) as posn
      FROM ge
      WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
        AND yr = 2017) x
WHERE x.posn = 1;
```

    COMMENT: or without RANK OVER() below

```sql
SELECT constituency, party
FROM ge x
WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
  AND yr = 2017
  AND votes > ALL
      (SELECT votes
       FROM ge y
       WHERE y.constituency = x.constituency
         AND y.party <> x.party
         AND yr = 2017);
```

6. Show how many seats for each party in Scotland in 2017.

```sql
SELECT party, COUNT(*)
FROM (SELECT constituency,
             party,
             votes,
             RANK() OVER (PARTITION BY constituency ORDER BY votes DESC) as posn
      FROM ge
      WHERE constituency LIKE 'S%'
        AND yr = 2017) x
WHERE x.posn = 1
GROUP BY x.party;
```

## COVID 19

1. Modify the query to show data from Spain

```sql
SELECT name, DAY (whn),
    confirmed, deaths, recovered
FROM covid
WHERE name = 'Spain'
  AND MONTH (whn) = 3
ORDER BY whn;
```

2. Modify the query to show confirmed for the day before.

```sql
SELECT name, DAY (whn), confirmed,
    LAG(confirmed, 1) OVER (PARTITION by name ORDER BY whn)
FROM covid
WHERE name = 'Italy' AND MONTH (whn) = 3
ORDER BY whn;
```

3. Show the number of new cases for each day, for Italy, for March.

```sql
SELECT name,
    DAY (whn) AS day,
    confirmed - LAG(confirmed, 1) OVER (PARTITION BY name ORDER BY whn) AS newcases
FROM covid
WHERE name = 'Italy'
  AND MONTH (whn) = 3
ORDER BY whn;
```

4. Show the number of new cases in Italy for each week - show Monday only.

        BUG: 'DATE_FORMAT' is not a recognized built-in function name. 
              It doesn't even show what the correct answer is.


5. Show the number of new cases in Italy for each week - show Monday only.

        BUG: 'DATE_FORMAT' is not a recognized built-in function name. 
              It doesn't even show what the correct answer is.

6. The query shown shows the number of confirmed cases together with the world ranking for cases. United States has the
   highest number, Spain is number 2... Notice that while Spain has the second highest confirmed cases, Italy has the
   second highest number of deaths due to the virus. Include the ranking for the number of deaths in the table.

```sql
SELECT name,
       confirmed,
       RANK() OVER (ORDER BY confirmed DESC) AS rc, deaths,
       RANK() OVER (ORDER BY deaths DESC) AS rd
FROM covid
WHERE whn = '2020-04-20'
ORDER BY confirmed DESC;
```

7. The query shown includes a JOIN t the world table so we can access the total population of each country and calculate
   infection rates (in cases per 100,000). Show the infect rate ranking for each country. Only include countries with a
   population of at least 10 million.

        BUG: just error, no query works

8. For each country that has had at last 1000 new cases in a single day, show the date of the peak number of new cases.

        BUG: 'DATE_FORMAT' is not a recognized built-in function name.
              It doesn't even show what the correct answer is.