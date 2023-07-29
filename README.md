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
 
