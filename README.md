# DA
Data Analytics projects and presentations

Using the fueleconomy database: 



--1. Write a query to inspect the schema of the vehicles table 



--2. Return a list of the 100 vehicles with the lowest highway mileage. 
—What make/model is # 100? (Just look at the list to answer)



--3. Limit this results of the query above to only those vehicles with hwy mileage of 100 mpg or more.
--How many are there?


—4.Return a list of models wiih an manual 5-spd transmission and either a 4 or 6 cylc engine, 
—order by cyclinders largest to smallest. 



—5.How many  distinct models manufactured each year between 1985 and 2000


—6.Which make had the highest average highway mileage between 1990 and 1995, 
--group the results by class, sorted by avg mileage descending, rounded to 2 decimal places. 



—7 Which makes and models have an average highway mileage equal to or greater than 35 between 1990 and 1995


--8. Which makes and models has the largest difference between their hwy and city mpg? (across all years)
 

--9 Which makes and models have a difference less than 5 mpg?






--1. Write a query to inspect the schema of the vehicles table 

SELECT *
FROM vehicles

SELECT column_name, data_type
FROM information_schema.columns
WHERE table_name = ‘vehicles’;

--2. Return a list of the 100 vehicles with the lowest highway mileage. 
—What make/model is # 100? (Just look at the list to answer)


SELECT hwy, make, model
FROM vehicles
ORDER BY hwy DESC
LIMIT 100;

--Acura Legend

--3. Limit this results of the query above to only those vehicles with hwy mileage of 100 mpg or more.
--How many are there?

SELECT hwy, make, model
FROM vehicles
WHERE hwy >= 100
ORDER BY hwy DESC
LIMIT 100;

-- 9

--4.Return a list of models wiih an manual 5-spd transmission and either a 4 or 6 cylc engine, 
--order by cyclinders largest to smallest. 

SELECT model, trans, cyl
FROM vehicles
WHERE cyl = '4' AND trans = 'Manual 5 spd' OR cyl = '6' AND trans = 'Manual 5-spd'
ORDER BY cyl DESC;

--2025

--5.How many  distinct models manufactured each year between 1985 and 2000

SELECT DISTINCT model, year
FROM vehicles
WHERE year BETWEEN 1985 AND 2000
ORDER BY year DESC;

--6.Which make had the highest average highway mileage between 1990 and 1995, 
--group the results by class, sorted by avg mileage descending, rounded to 2 decimal places. 

SELECT DISTINCT make, year, ROUND(AVG(hwy),2) AS avg_hwy
FROM vehicles 
WHERE year BETWEEN 1990 AND 1995
GROUP BY year, make
ORDER BY avg_hwy DESC;

--Suzuki

--7. Which makes and models have an average highway mileage equal to 
--or greater than 35 between 1990 and 1995

SELECT make, model, AVG(hwy) AS avg_hwy
FROM vehicles
WHERE year BETWEEN 1990 AND 1995
GROUP BY make, model
HAVING AVG(hwy) > 35
ORDER BY avg_hwy DESC;

--Geo Metro XFI

--8. Which makes and models has the largest difference between their 
--hwy and city mpg? (across all years)

SELECT make, model, year, SUM(hwy-cty) AS hwy_ctydiff
FROM vehicles
GROUP BY year,make, model
ORDER BY hwy_ctydiff DESC;


--9 Which makes and models have a difference less than 5 mpg?

SELECT DISTINCT model, make, year, SUM(hwy-cty) AS hwy_ctydiff
FROM vehicles
GROUP BY model, make, year
HAVING SUM(hwy-cty)< 5
ORDER BY hwy_ctydiff DESC;
