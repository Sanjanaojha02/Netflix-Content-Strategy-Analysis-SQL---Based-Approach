# Netflix-Content-Strategy-Analysis-SQL---Based-Approach
CREATE DATABASE netflix_db;
USE netflix_db;
CREATE TABLE netflix (
    show_id VARCHAR(20),
    type VARCHAR(20),
    title VARCHAR(255),
    director VARCHAR(150),
    cast TEXT,
    country VARCHAR(100),
    date_added DATE,
    release_year INT,
    rating VARCHAR(20),
    duration VARCHAR(20),
    listed_in TEXT,
    description TEXT
);
---------------DATA EXPLORATION------------------
select * from netflix;
                 -------------- DATA ANALYSIS----------------------
1.	Count the number of movies and tv shows
select
      type,
      count(*) as total_content
      from netflix
      group by type;
  	
2.	Find the most common ratings for movies and tv shows
select
      type,
      max(rating)
      from netflix
      group by 1;
3.	Find the most common ratings for movies and tv shows
select
      type,
      max(rating)
      from netflix
      group by 1;
  	
4.Find the top countries with most content on Netflix
SELECT country, COUNT(*) AS total_content
FROM netflix
WHERE country IS NOT NULL
GROUP BY country
ORDER BY total_content DESC
LIMIT 5;
SELECT *
FROM netflix
where 
     type = 'Movie'
     and
     duration = (select max(duration) from netflix);
     
5. Identify the longest TV show or Movie duration
SELECT *
FROM netflix
where 
     type = 'Movie'
     and
     duration = (select max(duration) from netflix);

6. Find all the Movies/TV shows by director ‘Toshiya Shinohara’
select * from netflix
where director = 'Toshiya Shinohara';

7. list all tv shows with more than 2 seasons
SELECT 
    *,
    CAST(SUBSTRING_INDEX(duration, ' ', 1) AS UNSIGNED) AS seasons
FROM netflix
WHERE type = 'TV Show'
AND CAST(SUBSTRING_INDEX(duration, ' ', 1) AS UNSIGNED) > 5;

8. Count the number of content items in each genre
SELECT TRIM(SUBSTRING_INDEX(listed_in, ',', 1)) AS genre,
       COUNT(*) AS total
FROM netflix
GROUP BY genre
ORDER BY total DESC;

9. Find each year and the Average  number of content released by US on Netflix and return top 5 year with highest Average content release.
SELECT 
    YEAR(STR_TO_DATE(date_added, '%M %d, %Y')) AS year_added,
    COUNT(*) AS total_content,
    ROUND(COUNT(*) / 12.0, 2) AS avg_content
FROM netflix
WHERE country = 'United States'
GROUP BY year_added
ORDER BY avg_content DESC
LIMIT 5;

10. Categorize the content based on the presence of the keywords ‘kill’ and ‘violence’ in the description  field. Label  content contains these keywors as ‘bad’ and ‘good’ . count how many items fall in each category.
SELECT 
    CASE
        WHEN description LIKE '%kill%'
          OR description LIKE '%violence%'
        THEN 'Bad'
        ELSE 'Good'
    END AS category,
    
    COUNT(*) AS total_content
FROM netflix
GROUP BY category;




