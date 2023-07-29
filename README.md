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
