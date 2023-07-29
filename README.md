# sql_zoo
SQL ZOO Project
## SELECT basics
### 1. Introducing the world table of countries
SELECT population FROM world
  WHERE name = 'Germany'
### 2. Scandinavia
SELECT name, population FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark');
### 3. Just the right size
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000

## SELECT FROM WORLD
### 1. Introduction
SELECT name, continent, population FROM world
### 2. Large countries
SELECT name FROM world
WHERE population >= 200000000
### 3. Per capita GDP
SELECT name, GDP/population
FROM world
WHERE population >= 200000000
### 4. South America in Millions
SELECT name, population/1000000
FROM world
WHERE continent = 'South America'
### 5. France, Germany, Italy
SELECT name, population 
FROM world
WHERE name IN ('France', 'Germany', 'Italy')
### 6. United
SELECT name
FROM world
WHERE name LIKE '%United%'
### 7. Two way too big 
SELECT name, population, area
FROM world
WHERE area >= 3000000 OR population >= 250000000
### 8. One but not the other
SELECT name, population, area
FROM world
WHERE area >= 3000000 XOR population >= 250000000
### 9. Rounding
SELECT name, population/1000000, gdp/10000000
FROM world
WHERE continent = 'South America'
### 10. Trillion dollar economies
SELECT name, ROUND((GDP/population),-3)
FROM world
WHERE GDP >= 1000000000000
### 11. Name and Capital have same length
SELECT name, capital
FROM world
WHERE LENGTH(name) = LENGTH(capital)
### 12. Matching name and capital
SELECT name, capital
FROM world
WHERE LEFT(name,1) = LEFT(capital,1)
AND name <> capital
### 13. All the vowels
SELECT name
   FROM world
WHERE name LIKE '%a%'
  AND name LIKE '%e%'
  AND name LIKE '%i%'
  AND name LIKE '%o%'
  AND name LIKE '%u%'
  AND name NOT LIKE '% %'

## SELECT from Nobel Tutorial
### 1. Winners for 1950
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
### 2. 1962 Literature
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'literature'
### 3. Albert Einstein
SELECT yr, subject
FROM nobel 
WHERE winner = 'Albert Einstein'
### 4. Reent Prizes
SELECT winner
FROM nobel 
WHERE subject = 'peace' AND yr >= 2000
### 5. Literature in 1980's
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'literature' AND yr BETWEEN 1980 AND 1989
### 6. Only presidents
SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Woodrow Wilson',
                  'Jimmy Carter',
'Barack Obama')
### 7. John
SELECT winner
FROM nobel
WHERE winner LIKE 'John_%'
### 8. Chemistry and Physics from different years
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1980 and subject = 'physics' OR yr = 1984 and subject = 'chemistry'
### 9. Exclude chemists and medics
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1980 AND subject NOT IN ('chemistry', 'medicine')
### 10. Early medicine and literature 
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'medicine' AND yr < 1910 OR subject = 'literature' AND yr >= 2004
### 11. Umlaut
SELECT yr, subject, winner
FROM nobel 
WHERE winner LIKE '%ünberg%'
### 12. Apostrophe
SELECT * 
FROM nobel 
WHERE winner LIKE '%O''NEILL%'
### 13. Knights of the realm
SELECT winner, yr, subject 
FROM nobel
WHERE winner LIKE 'Sir%' ORDER BY yr DESC
### 14. Chemistry and Physics last
SELECT winner, subject
  FROM nobel
 WHERE yr=1984
 ORDER BY subject IN ('physics','chemistry'), subject,winner

## SELECT from select
### 1. Bigger than Russia
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
### 2. Richer than UK
SELECT name 
FROM world
WHERE continent = 'Europe' AND gdp/population > (SELECT gdp/population
             FROM world
             WHERE name = 'United Kingdom')
### 3. Neighbors of Argentina and Australia
SELECT name, continent
FROM world
WHERE continent = 'Americas' OR continent = 'Oceania'
### 4. Between Canada and Poland
SELECT name, population 
FROM world
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom') AND population < (SELECT population FROM world WHERE name = 'Germany')
### 5. Percentages of Germany
SELECT name, CONCAT(ROUND((population / (SELECT population FROM world WHERE name = 'Germany'))*100,0),'%') AS percentage
FROM world
WHERE continent = 'Europe'
### 6. Bigger than every country in Europe
SELECT name
FROM world
WHERE gdp > ALL(SELECT gdp
                                           FROM world
                                           WHERE continent = 'Europe'
                                           AND gdp IS NOT NULL)  
### 7. Largest in each continent
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
### 8. First Country of each continent
SELECT continent, MIN(name) FROM world x
GROUP BY continent
ORDER BY name
### 9. Difficult Questions
SELECT name, continent, population 
FROM world x
WHERE NOT EXISTS( SELECT population 
                   FROM world y 
                   WHERE x.continent = y.continent
                   AND population >= 25000000)
### 10. Three times bigger
SELECT name, continent
FROM world x
WHERE population >= ALL(SELECT population * 3
                        FROM world y
                        WHERE x.continent = y.continent
                        AND x.name <> y.name)

## SUM and COUNT
### 1. Total world population
SELECT SUM(population)
FROM world
### 2. List of continents
SELECT DISTINCT continent
FROM world
### 3. GDP of Africa
SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa'
### 4. Count the big countries
SELECT COUNT(name)
FROM world
WHERE area >= 1000000
### 5. Baltic states population 
SELECT SUM(population)
FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
### 6. Using GROUP BY and HAVING
SELECT continent, COUNT(name)
FROM world
GROUP BY continent
### 7. Counting big countries in each continent
SELECT continent, COUNT(name)
FROM world
WHERE population >= 10000000
GROUP BY continent
### 8. Counting big continents
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) > 100000000

## JOIN
### 1.
SELECT matchid, player FROM goal 
  WHERE teamid = 'GER'
### 2.
SELECT id,stadium,team1,team2
  FROM game

WHERE game.id = 1012
### 3.
SELECT player, teamid, stadium, mdate
  FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'
### 4.
SELECT team1, team2, player
FROM game
INNER JOIN goal
ON game.id = goal.matchid
WHERE player LIKE 'Mario%'
### 5.
SELECT player, teamid, coach, gtime
  FROM goal
INNER JOIN eteam
ON goal.teamid = eteam.id 
 WHERE gtime<=10
### 6. 
SELECT mdate, teamname
FROM game
INNER JOIN eteam 
ON game.team1 = eteam.id
WHERE coach = 'Fernando Santos'
### 7. 
SELECT player
FROM game
INNER JOIN goal
ON game.id = goal.matchid
WHERE stadium = 'National Stadium, Warsaw'
### 8.
SELECT DISTINCT player
  FROM game JOIN goal ON matchid = id 
    WHERE (team1='GER' OR team2='GER')
    AND goal.teamid <> 'GER'
### 9. 
SELECT teamname, COUNT(goal.teamid)
  FROM eteam JOIN goal ON id=teamid
GROUP BY teamname
 ORDER BY teamname
### 10. 
SELECT stadium, COUNT(goal.matchid)
FROM game
INNER JOIN goal ON game.id = goal.matchid
GROUP BY stadium
### 11. 
SELECT matchid, mdate, COUNT(goal.matchid)
  FROM game JOIN goal ON matchid = id 
 WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid, mdate, goal.matchid
### 12. 
SELECT matchid, mdate, COUNT(goal.matchid)
FROM game
INNER JOIN goal ON game.id = goal.matchid
WHERE goal.teamid = 'GER'
GROUP BY matchid, mdate
### 13. 
SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
  FROM game LEFT JOIN goal ON matchid = id 
GROUP BY mdate,matchid,team1,team2

## MORE JOIN
### 1. 
SELECT id, title
 FROM movie
 WHERE yr=1962
### 2. 
SELECT yr
FROM movie
WHERE title = 'Citizen Kane'
### 3. 
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr
### 4. 
SELECT id
FROM actor
WHERE name = 'Glenn Close'
### 5. 
SELECT id
FROM movie
WHERE title = 'Casablanca' 
### 6. 
SELECT actor.name 
FROM casting

JOIN actor ON actor.id = casting.actorid
WHERE movieid = 11768
### 7. 
SELECT actor.name
FROM casting
JOIN movie ON movie.id = casting.movieid
JOIN actor ON actor.id = casting.actorid
WHERE movie.title = 'Alien'
### 8. 
SELECT movie.title
FROM movie
JOIN casting ON movie.id = casting.movieid
JOIN actor ON actor.id = casting.actorid
WHERE actor.name = 'Harrison Ford'
### 9. 
SELECT movie.title
FROM movie
JOIN casting ON movie.id = casting.movieid
JOIN actor ON actor.id = casting.actorid
WHERE actor.name = 'Harrison Ford' AND ord <> 1
### 10. 
SELECT movie.title, actor.name
FROM movie
JOIN casting ON movie.id = casting.movieid
JOIN actor ON actor.id = casting.actorid
WHERE movie.yr = 1962 AND ord = 1
### 11. 
SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
        JOIN actor   ON actorid=actor.id
WHERE name='Rock Hudson'
GROUP BY yr
HAVING COUNT(title) > 2
### 12.
SELECT movie.title, actor.name 
FROM movie
JOIN casting ON casting.movieid = movie.id
JOIN actor ON actor.id = casting.actorid
WHERE movie.id IN(
SELECT DISTINCT movieid FROM casting
WHERE actorid IN (
  SELECT id FROM actor
  WHERE name='Julie Andrews'))
AND ord = 1
### 13. 
SELECT actor.name
FROM actor
JOIN casting ON actor.id = casting.actorid
JOIN movie ON movie.id = casting.movieid
WHERE casting.ord = 1 
GROUP BY actor.name
HAVING COUNT(casting.ord) >= 15
ORDER BY actor.name
### 14. 
SELECT movie.title, COUNT(casting.actorid)
FROM movie
JOIN casting ON movie.id = casting.movieid
JOIN actor ON actor.id = casting.actorid
WHERE movie.yr = 1978
GROUP BY movie.title
ORDER BY COUNT(casting.actorid) DESC, movie.title 
### 15. 
SELECT DISTINCT actor.name
FROM actor
JOIN casting ON actor.id = casting.actorid
JOIN movie ON movie.id = casting.movieid
WHERE actor.name <> 'Art Garfunkel' 
AND
casting.movieid IN (SELECT movie.id 
                          FROM movie
                          JOIN casting ON movie.id = casting.movieid
                          JOIN actor ON actor.id = casting.actorid 
                          WHERE actor.name = 'Art Garfunkel')

