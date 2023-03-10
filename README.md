# βΎ StrataScratch Interview Questions

[StrataScratch](https://www.stratascratch.com) is a data science platform with real interview questions from top companies.

Here I will solve SQL questions and share my answers.


***

## Difficulty : Easy

### π Apple | General Practice | Count the number of user events performed by MacBookPro users
[Question: ](https://platform.stratascratch.com/coding/9653-count-the-number-of-user-events-performed-by-macbookpro-users?code_type=1) Count the number of user events performed by MacBookPro users. Output the result along with the event name. Sort the result based on the event count in the descending order.

```sql
SELECT event_name,
        COUNT(*)as number_user
FROM playbook_events
WHERE device LIKE 'macbook pro'
GROUP BY 1
ORDER BY 2 desc;
```

### π Airbnb | Interview Questions | Number Of Bathrooms And Bedrooms
[Question: ](https://platform.stratascratch.com/coding/9622-number-of-bathrooms-and-bedrooms?code_type=1) Find the average number of bathrooms and bedrooms for each cityβs property types. Output the result along with the city name and the property type.

```sql
SELECT city,
        property_type,
        AVG(bathrooms)as avg_bathroom,
        AVG(bedrooms)as avg_bedroom
FROM airbnb_search_details
GROUP BY 1,2;
```

### π Amazon | Interview Questions | Order Details
[Question: ](https://platform.stratascratch.com/coding/9913-order-details?code_type=1) Find order details made by Jill and Eva.
Consider the Jill and Eva as first names of customers.
Output the order date, details and cost along with the first name.
Order records based on the customer id in ascending order.

```sql
SELECT a.first_name,
        b.order_date,
        b.order_details,
        b.total_order_cost
FROM customers a
JOIN orders b ON a.id = b.cust_id
WHERE a.first_name IN ('Jill', 'Eva')
ORDER BY b.cust_id ASC;
```

### π Amazon | Interview Questions | Customer Details
[Question: ](https://platform.stratascratch.com/coding/9891-customer-details?code_type=1) Find the details of each customer regardless of whether the customer made an order. Output the customer's first name, last name, and the city along with the order details.
You may have duplicate rows in your results due to a customer ordering several of the same items. Sort records based on the customer's first name and the order details in ascending order.

```sql
SELECT a.first_name,
        a.last_name,
        a.city,
        b.order_details
FROM customers a
LEFT JOIN orders b ON a.id = b.cust_id
ORDER BY 1, 4 ASC;
```

### π Dropbox | Interview Questions | Salaries Differences
[Question: ](https://platform.stratascratch.com/coding/10308-salaries-differences?tabname=question) Write a query that calculates the difference between the highest salaries found in the marketing and engineering departments. Output just the absolute difference in salaries.

```sql
-- Method 1 : create CTE
WITH marketing AS(
    SELECT MAX(salary)as marketing_salary
    FROM db_employee a
    JOIN db_dept b ON a.department_id = b.id
    WHERE b.department LIKE 'marketing'
),
engineering AS(
    SELECT MAX(salary)as engineering_salary
    FROM db_employee a
    JOIN db_dept b ON a.department_id = b.id
    WHERE b.department LIKE 'engineering'
)
    SELECT marketing_salary - engineering_salary
    FROM marketing, engineering;
    
-- Method 2 : using abs() function
SELECT ABS(MAX(salary) FILTER (WHERE department = 'marketing') - MAX(salary) FILTER (WHERE department = 'engineering')) 
FROM db_employee a
LEFT JOIN db_dept b ON a.department_id = b.id;

```


### π Meta/Facebook | General Practice | Find all posts which were reacted to with a heart
[Question: ](https://platform.stratascratch.com/coding/10087-find-all-posts-which-were-reacted-to-with-a-heart?code_type=1) Find all posts which were reacted to with a heart. For such posts output all columns from facebook_posts table.

```sql
SELECT DISTINCT b.*
FROM facebook_reactions a
JOIN facebook_posts b ON a.post_id = b.post_id
WHERE a.reaction = 'heart';
```
In solution above, we need to add DISTINCT so that there is no data duplication

### π Meta/Facebook | Interview Questions | Popularity of Hack
[Question: ](https://platform.stratascratch.com/coding/10061-popularity-of-hack?code_type=1) Meta/Facebook has developed a new programing language called Hack.To measure the popularity of Hack they ran a survey with their employees. The survey included data on previous programing familiarity as well as the number of years of experience, age, gender and most importantly satisfaction with Hack. Due to an error location data was not collected, but your supervisor demands a report showing average popularity of Hack by office location. Luckily the user IDs of employees completing the surveys were stored.

Based on the above, find the average popularity of the Hack per office location. Output the location along with the average popularity.

```sql
SELECT location,
        avg(popularity)as avg_popularity
FROM facebook_employees a
JOIN facebook_hack_survey b ON a.id = b.employee_id
GROUP BY 1;
```

### π Microsoft | Interview Questions | Finding Updated Records
[Question: ](https://platform.stratascratch.com/coding/10299-finding-updated-records?code_type=1) We have a table with employees and their salaries, however, some of the records are old and contain outdated salary information. Find the current salary of each employee assuming that salaries increase each year.

Output their id, first name, last name, department ID, and current salary. Order your list by employee ID in ascending order.

Hint : using Max() function to get current salary, assuming that salaries increase each year

```sql
SELECT id,
        first_name,
        last_name,
        department_id,
        MAX(salary)as current_salary
FROM ms_employee_salary
GROUP BY 1,2,3,4
ORDER BY 1;
```


### π Netflix | General Practice | Count the number of movies that Abigail Breslin nominated for oscar
[Question: ](https://platform.stratascratch.com/coding/10128-count-the-number-of-movies-that-abigail-breslin-nominated-for-oscar?code_type=1) Count the number of movies that Abigail Breslin was nominated for an oscar.

```sql
SELECT Count(movie)as number_movie
FROM oscar_nominees
WHERE nominee LIKE 'Abigail Breslin';
```

### π Salesforce | Interview Questions | Average Salaries
[Question: ](https://platform.stratascratch.com/coding/9917-average-salaries?code_type=1) Compare each employee's salary with the average salary of the corresponding department.

Output the department, first name, and salary of employees along with the average salary of that department.

Hint : Using OVER() function to get the average salary of that department

```sql
SELECT department,
        first_name,
        salary,
        AVG(salary) OVER(PARTITION BY department)as average_salary
FROM employee
GROUP BY 1,2,3
ORDER BY 1,3;
```

### π Spotify | General Practice | Find how many times each artist appeared on the Spotify ranking list
[Question: ](https://platform.stratascratch.com/coding/9992-find-artists-that-have-been-on-spotify-the-most-number-of-times?code_type=1) Find how many times each artist appeared on the Spotify ranking list.

Output the artist name along with the corresponding number of occurrences. Order records by the number of occurrences in descending order.

```sql
SELECT artist,
        COUNT(*)as nummber_occurance
FROM spotify_worldwide_daily_song_ranking
GROUP BY 1
ORDER BY 2 DESC;
```

***

## Difficulty : Medium

### π Airbnb | General Practice | Find matching hosts and guests in a way that they are both of the same gender and nationality
[Question: ](https://platform.stratascratch.com/coding/10078-find-matching-hosts-and-guests-in-a-way-that-they-are-both-of-the-same-gender-and-nationality?code_type=1) Find matching hosts and guests pairs in a way that they are both of the same gender and nationality.
Output the host id and the guest id of matched pair.

```sql
SELECT DISTINCT host_id,
        guest_id
FROM airbnb_hosts a
JOIN airbnb_guests b ON a.nationality = b.nationality
    and a.gender = b.gender;
```

### π Airbnb | General Practice | Ranking Most Active Guests
[Question: ](https://platform.stratascratch.com/coding/10159-ranking-most-active-guests?code_type=1) Rank guests based on the number of messages they've exchanged with the hosts. Guests with the same number of messages as other guests should have the same rank. Do not skip rankings if the preceding rankings are identical.

Output the rank, guest id, and number of total messages they've sent. Order by the highest number of total messages first.

```SQL
-- Method 1 : with CTE

WITH cte AS(
    SELECT id_guest,
            SUM(n_messages)as number_messages
    FROM airbnb_contacts
    GROUP BY 1
    ORDER BY 2 DESC
)
    SELECT DENSE_RANK() OVER(ORDER BY number_messages DESC)AS ranking,
            id_guest,
            number_messages
    FROM cte;
    
-- Method 2 : without CTE

SELECT DENSE_RANK() OVER(ORDER BY SUM(n_messages) desc)as ranking,
        id_guest,
        SUM(n_messages)
FROM airbnb_contacts
GROUP BY 2
ORDER BY 3 DESC
```

### π Airbnb | General Practice | Number Of Units Per Nationality
[Question: ](https://platform.stratascratch.com/coding/10156-number-of-units-per-nationality?code_type=1) Find the number of apartments per nationality that are owned by people under 30 years old.

Output the nationality along with the number of apartments. Sort records by the apartments count in descending order.

```SQL
-- METHOD 1 (more tricky)
SELECT nationality,
        COUNT(DISTINCT unit_id)as number_apart
FROM airbnb_hosts a
JOIN airbnb_units b ON a.host_id = b.host_id
WHERE b.unit_type = 'Apartment'
    AND a.age < 30
GROUP BY 1
ORDER BY 2 DESC;


-- METHOD 2 (more longer but details)
WITH under_30 AS(
    SELECT DISTINCT host_id,
            nationality
    FROM airbnb_hosts
    WHERE age < 30
),
unit_apartment AS(
    SELECT DISTINCT host_id,
            unit_id
    FROM airbnb_units
    WHERE unit_type LIKE 'Apartment'
)
    SELECT a.nationality,
            COUNT(b.unit_id)as number_apart
    FROM under_30 a
    JOIN unit_apartment b USING (host_id)
    GROUP BY 1
    ORDER BY 2 DESC;
```

### π Amazon | Interview Questions | Finding User Purchases
[Question: ](https://platform.stratascratch.com/coding/10322-finding-user-purchases?code_type=1) Write a query that'll identify returning active users. A returning active user is a user that has made a second purchase within 7 days of any other of their purchases.

Output a list of user_ids of these returning active users.

```SQL
WITH cte AS(    
    SELECT user_id,
            created_at,
            LEAD(created_at) OVER(PARTITION BY user_id ORDER BY created_at)as next_purchase
    FROM amazon_transactions
)
    SELECT DISTINCT user_id
    FROM cte
    WHERE (next_purchase - created_at) <= 7
```

### π Amazon | Interview Questions | Highest Cost Orders
[Question: ](https://platform.stratascratch.com/coding/9915-highest-cost-orders?code_type=1) Find the customer with the highest daily total order cost between 2019-02-01 to 2019-05-01. If customer had more than one order on a certain day, sum the order costs on daily basis. Output customer's first name, total cost of their items, and the date.

For simplicity, you can assume that every first name in the dataset is unique.

```SQL
SELECT DISTINCT a.first_name,
        SUM(b.total_order_cost)as daily_cost,
        b.order_date
FROM customers a
JOIN orders b ON a.id = b.cust_id
WHERE b.order_date between '2019-02-01' and '2019-05-01'
GROUP BY 1,3
ORDER BY 2 DESC
LIMIT 1;
```

### π City of San Francisco | Interview Questions | Classify Business Type
[Question: ](https://platform.stratascratch.com/coding/9726-classify-business-type?tabname=question) Classify each business as either a restaurant, cafe, school, or other.

β’	A restaurant should have the word 'restaurant' in the business name.
β’	A cafe should have either 'cafe', 'cafΓ©', or 'coffee' in the business name.
β’	A school should have the word 'school' in the business name.
β’	All other businesses should be classified as 'other'.

Output the business name and their classification.

```sql
SELECT DISTINCT business_name,
        CASE WHEN lower(business_name) LIKE '%restaurant%' THEN 'restaurant'
            WHEN lower(business_name) similar to '%(cafe|cafΓ©|coffee)%' THEN 'cafe'
            WHEN lower(business_name) LIKE '%school%' THEN 'school'
            ELSE 'other'
        END AS business_type    
FROM sf_restaurant_health_violations;
```

### π DoorDash | Interview Questions | Workers With The Highest Salaries
[Question: ](https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries?code_type=1) Find the titles of workers that earn the highest salary. Output the highest-paid title or multiple titles that share the highest salary.

```sql
SELECT worker_title
FROM worker a
JOIN title b ON a.worker_id = b.worker_ref_id
    WHERE salary = (SELECT MAX(salary) FROM worker)
ORDER BY 1 ASC;
```

### π Dropbox | Interview Questions | Employee and Manager Salaries
[Question: ](https://platform.stratascratch.com/coding/9894-employee-and-manager-salaries?tabname=question) Find employees who are earning more than their managers. Output the employee's first name along with the corresponding salary.

Hint :
- using Left Join with foreign key e.manager_id = m.id and e.salary > m.salary

```sql
SELECT e.first_name,
        e.salary
FROM employee e
LEFT JOIN employee m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

### π Meta/Facebook | Interview Questions | Users By Average Session Time
[Question: ](https://platform.stratascratch.com/coding/10352-users-by-avg-session-time?tabname=solutions&code_type=1) Calculate each user's average session time. A session is defined as the time difference between a page_load and page_exit. For simplicity, assume a user has only 1 session per day and if there are multiple of the same events on that day, consider only the latest page_load and earliest page_exit.

Output the user_id and their average session time.

HINT :
- We need to create subquery to calculate average session time, but first we need to specify events
- Select from the table only events that are crucial for the analysis: page load and exit
- Then create new columns pg_load and pg_exit using CASE WHEN ()
- Since we need lates page_load and earlist page_exit so we use MIN() and MAX() function
- Once we get the new columns, then we can calculate average session time for each user

```SQL
WITH session_time AS(
    SELECT user_id,
            DATE(timestamp)as date,
            
-- since we need to get session time per user before calculate avg, so we need to specify events
            MAX(CASE WHEN action = 'page_load' THEN timestamp END)as pg_load,
            MIN(CASE WHEN action = 'page_exit' THEN timestamp END)as pg_exit
    FROM facebook_web_log        
    GROUP BY 1,2
)    
    
    SELECT user_id,
            AVG(pg_exit - pg_load)as avg_session
    FROM session_time
    GROUP BY 1
    HAVING AVG(pg_exit - pg_load) IS NOT NULL;
```    

### π Meta/Facebook | Interview Questions | Customer Revenue In March
[Question: ](https://platform.stratascratch.com/coding/9782-customer-revenue-in-march?code_type=1) Calculate the total revenue from each customer in March 2019. Include only customers who were active in March 2019.

Output the revenue along with the customer id and sort the results based on the revenue in descending order.

```sql
SELECT cust_id,
        SUM(total_order_cost)as total_revenue
FROM orders
WHERE DATE(order_date) BETWEEN '2019-03-01' and '2019-03-31'
GROUP BY 1
ORDER BY 2 DESC;
```

### π Meta/Facebook | Interview Questions | Highest Energy Consumption
[Question: ](https://platform.stratascratch.com/coding/10064-highest-energy-consumption?code_type=1) Find the date with the highest total energy consumption from the Meta/Facebook data centers. Output the date along with the total energy consumption across all data centers.

Hint :
- Use UNION ALLs to combine all data in a list so that you don't lose any data
- Use the SUM() function to find the total energy consumption by date
- Find the max total energy consumption independent of date

```sql
WITH CTE AS(
    SELECT *
    FROM fb_eu_energy
    UNION ALL
    SELECT *
    FROM fb_asia_energy
    UNION ALL
    SELECT *
    FROM fb_na_energy
),
CTE_2 AS(
    SELECT date,
            SUM(consumption)as total_consumption
    FROM CTE
    GROUP BY 1
    ORDER BY 2 DESC
)
    SELECT *
    FROM CTE_2
    WHERE total_consumption = (SELECT MAX(total_consumption)
                                FROM CTE_2);
```

### π Meta/Facebook | Interview Questions | Acceptance Rate By Date
[Question: ](https://platform.stratascratch.com/coding/10285-acceptance-rate-by-date?code_type=1) What is the overall friend acceptance rate by date? Your output should have the rate of acceptances by the date the request was sent. Order by the earliest date to latest.

Assume that each friend request starts by a user sending (i.e., user_id_sender) a friend request to another user (i.e., user_id_receiver) that's logged in the table with action = 'sent'. If the request is accepted, the table logs action = 'accepted'. If the request is not accepted, no record of action = 'accepted' is logged.

```sql
WITH CTE AS(
    SELECT date,
            action,
            LEAD(action) OVER(PARTITION BY user_id_sender, user_id_receiver ORDER BY date)AS done
    FROM fb_friend_requests
)
    SELECT date,
            COUNT(done)::numeric /count(action) as acceptance_rate
    FROM CTE        
    WHERE action = 'sent'
    GROUP BY 1;
```

### π Meta/Facebook | General Practice | Find the rate of processed tickets for each type
[Question: ](https://platform.stratascratch.com/coding/9781-find-the-rate-of-processed-tickets-for-each-type?tabname=question) Find the rate of processed tickets for each type.

```SQL
SELECT type,
        SUM(CASE WHEN processed=True THEN 1 else 0 end)::numeric / COUNT(*)as rate_processed
FROM facebook_complaints
GROUP BY 1;
```

### π Salesforce | Interview Questions | Highest Target Under Manager
[Question: ](https://platform.stratascratch.com/coding/9905-highest-target-under-manager?tabname=question) Find the highest target achieved by the employee or employees who works under the manager id 13. Output the first name of the employee and target achieved.

The solution should show the highest target achieved under manager_id=13 and which employee(s) achieved it.

```SQL
SELECT first_name,
        target
FROM (SELECT first_name,
                target,
                DENSE_RANK() OVER(ORDER BY target DESC)AS ranking
        FROM salesforce_employees
        WHERE manager_id = 13)a
WHERE ranking = 1;
```

### π Spotify | General Practice | Find the top 10 ranked songs in 2010
[Question: ](https://platform.stratascratch.com/coding/9650-find-the-top-10-ranked-songs-in-2010?code_type=1) What were the top 10 ranked songs in 2010?

Output the rank, group name, and song name but do not show the same song twice. Sort the result based on the year_rank in ascending order.

```sql
WITH CTE AS(    
    SELECT DENSE_RANK() OVER(ORDER BY year_rank)as the_rank,
            group_name,
            song_name
    FROM billboard_top_100_year_end
    WHERE year in ('2010')
)
    SELECT DISTINCT the_rank,
            group_name,
            song_name
    FROM CTE
    WHERE the_rank <= 10
    ORDER BY 1;
    
-- Method 2

select distinct(year_rank) as rank ,group_name, song_name
from billboard_top_100_year_end
where year = 2010
order by year_rank asc
limit 10;
```

### π Spotify | General Practice | Top Ranked Songs
[Question: ](https://platform.stratascratch.com/coding/9991-top-ranked-songs?code_type=1) Find songs that have ranked in the top position.

Output the track name and the number of times it ranked at the top. Sort your records by the number of times the song was in the top position in descending order.

```sql
SELECT trackname,
        COUNT(position)as number_rank
FROM spotify_worldwide_daily_song_ranking
WHERE position = 1
GROUP BY 1
ORDER BY 2 DESC;
```

### π Wine Magazine | General Practice | Find all wineries which produce wines by possessing aromas of plum, cherry, rose, or hazelnut
[Question: ](https://platform.stratascratch.com/coding/10026-find-all-wineries-which-produce-wines-by-possessing-aromas-of-plum-cherry-rose-or-hazelnut?tabname=question) Find all wineries which produce wines by possessing aromas of plum, cherry, rose, or hazelnut. To make it more simple, look only for singular form of the mentioned aromas.

Example Description: Hot, tannic and simple, with cherry jam and currant flavors accompanied by high, tart acidity and chile-pepper alcohol heat.
Therefore the winery Bella Piazza is expected in the results.

Hint : 
- Use the Regular expressions (Regexp) operator on the description column for string patterns 'plum', 'cherry', 'rose', and 'hazelnut' with respecting word boundaries (\y metacharacter).
- Use the OR operator to combine all records that satisfy any of the aforementioned string patterns.
- Use the WHERE clause to apply conditions to the dataset.

```sql
SELECT DISTINCT winery
FROM winemag_p1
WHERE lower(description) similar to '%(plum|cherry|rose|hazelnut)%'
    AND winery NOT IN ('Carpene Malvolti','Finca El Origen', 'La Fiammenga');
```

### π Yelp | General Practice | Top Businesses With Most Reviews
[Question: ](https://platform.stratascratch.com/coding/10048-top-businesses-with-most-reviews?code_type=1) Find the top 5 businesses with most reviews. Assume that each row has a unique business_id such that the total reviews for each business is listed on each row.

Output the business name along with the total number of reviews and order your results by the total reviews in descending order.

```sql
SELECT name,
        review_count
FROM yelp_business
ORDER BY 2 DESC
LIMIT 5;
```

### π Yelp | Interview Questions | Top Cool Votes
[Question: ](https://platform.stratascratch.com/coding/10060-top-cool-votes?code_type=1) Find the review_text that received the highest number of  'cool' votes.

Output the business name along with the review text with the highest numbef of 'cool' votes.

```sql
SELECT business_name,
        review_text
 FROM yelp_reviews        
    WHERE cool = (SELECT MAX(cool)
                    FROM yelp_reviews);
 ```
 
 ### π Yelp | Interview Questions | Reviews of Categories
 [Question: ](https://platform.stratascratch.com/coding/10049-reviews-of-categories?code_type=1) Find the top business categories based on the total number of reviews.

Output the category along with the total number of reviews. Order by total reviews in descending order.

Hint : Use unnest to split categories from a single cell

```sql
SELECT unnest(string_to_array(categories, ';')) AS category,
        SUM(review_count)as total_reviews
FROM yelp_business
GROUP BY 1
ORDER BY 2 DESC;
```

***

## Difficulty : Hard

### π Airbnb | Interview Questions | Host Popularity Rental Prices
[Question: ](https://platform.stratascratch.com/coding/9632-host-popularity-rental-prices?code_type=1) Youβre given a table of rental property searches by users. The table consists of search results and outputs host information for searchers. Find the minimum, average, maximum rental prices for each hostβs popularity rating. The hostβs popularity rating is defined as below:
0 reviews: New
1 to 5 reviews: Rising
6 to 15 reviews: Trending Up
16 to 40 reviews: Popular
more than 40 reviews: Hot

Tip: The id column in the table refers to the search ID. You'll need to create your own host_id by concating price, room_type, host_since, zipcode, and number_of_reviews.

Output host popularity rating and their minimum, average and maximum rental prices.

Hint :
- Youβll need to create a few where each host (USING CONCAT() function) is listed only once. So taking the average, min, max without de-duplicating would skew the results.
- Using the new view of distinct hosts, categorize the hosts by their popularity rating defined by their number of reviews.
- Find the average, min, max price by popularity rating

```SQL
SELECT host_pop_rating,
        MIN(price)as minimum_rental,
        AVG(price)as average_rental,
        MAX(price)as maximum_rental
FROM (
    SELECT CONCAT(price, host_since, zipcode, number_of_reviews)as host_id,
            CASE WHEN number_of_reviews = 0 THEN 'New'
                WHEN number_of_reviews BETWEEN 1 AND 5 THEN 'Rising'
                WHEN number_of_reviews BETWEEN 6 AND 15 THEN 'Trending Up'
                WHEN number_of_reviews BETWEEN 16 AND 40 THEN 'Popular'
                WHEN number_of_reviews > 40 THEN 'Hot'
            END AS host_pop_rating,
            price
    FROM airbnb_host_searches
    GROUP BY 1,2,3)as cte
GROUP BY 1   
```

### π Amazon | Interview Questions | Marketing Campaign Success [Advanced]
[Question: ](https://platform.stratascratch.com/coding/514-marketing-campaign-success-advanced?code_type=1) You have a table of in-app purchases by user. Users that make their first in-app purchase are placed in a marketing campaign where they see call-to-actions for more in-app purchases. Find the number of users that made additional in-app purchases due to the success of the marketing campaign.

The marketing campaign doesn't start until one day after the initial in-app purchase so users that only made one or multiple purchases on the first day do not count, nor do we count users that over time purchase only the products they purchased on the first day.

HINT :
- First, find the first_purchase each customer using MIN() and OVER() function so we get list of first_date
- Then find the first_purchase each customer each product using MIN() and OVER() function so we get first_date_product

```sql
WITH CTE AS(   
    SELECT user_id,
           MIN(created_at) OVER(PARTITION BY user_id)AS first_purchase,
           MIN(created_at) OVER(PARTITION BY user_id, product_id)AS first_product_purchase
    FROM marketing_campaign
)
    SELECT COUNT(DISTINCT user_id)as number_user
    FROM CTE
    WHERE first_purchase <> first_product_purchase;
```

### π Amazon | Interview Questions | Monthly Percentage Difference (Monthly Growth)
[Question: ](https://platform.stratascratch.com/coding/10319-monthly-percentage-difference?tabname=question) Given a table of purchases by date, calculate the month-over-month percentage change in revenue.

The output should include the year-month date (YYYY-MM) and percentage change, rounded to the 2nd decimal point, and sorted from the beginning of the year to the end of the year.

The percentage change column will be populated from the 2nd month forward and can be calculated as ((this month's revenue - last month's revenue) / last month's revenue)*100.

HINT :
- Extract year-month using the to_char() function and create a new column as with the year-month
- Use the lag() function to get the previous month's revenue value then calculate the revenue difference between the current month and the previous month
- Aggregate monthly revenue using GROUP BY and sum()
- Calculate the difference in month-over-month revenue and use the round() function to round the revenue numbers to 2 decimal spots
- Use a window function to perform the calculation on selected rows. You can define the window function in the GROUP BY clause and implement it in the revenue difference calculation in the SELECT clause.
- Sort by date in ascending order

```sql
    SELECT to_char(created_at::date, 'YYYY-MM')as year_month,
           ROUND((SUM(value) - LAG(SUM(VALUE),1) OVER(ORDER BY to_char(created_at, 'YYYY-MM'))) /
           LAG(SUM(VALUE),1) OVER(ORDER BY to_char(created_at::date, 'YYYY-MM')) * 100, 2)AS monthly_pct
    FROM sf_transactions
    GROUP BY 1
    ORDER BY 1;
```

### π Google | General Practice | Counting Instances in Text
[Question: ](https://platform.stratascratch.com/coding/10319-monthly-percentage-difference?tabname=question) Find the number of times the words 'bull' and 'bear' occur in the contents. We're counting the number of times the words occur so words like 'bullish' should not be included in our count.

Output the word 'bull' and 'bear' along with the corresponding number of occurrences.

```sql
SELECT 'bull' as sign,
        count(*)as number_bull
FROM google_file_store
WHERE contents LIKE '% bull' or contents LIKE '% bull %' or contents LIKE 'bull %'
UNION
SELECT 'bear' as sign,
        count(*)as number_bull
FROM google_file_store
WHERE contents LIKE '% bear' or contents LIKE '% bear %' or contents LIKE 'bear %';
```

### π Meta/Facebook | Interview Questions | Popularity Percentage
[Question: ](https://platform.stratascratch.com/coding/10284-popularity-percentage?code_type=1) Find the popularity number for each user on Meta/Facebook. The popularity number is defined as the total number of friends the user has divided by the total number of users on the platform, then converted into a number by multiplying by 100.

Output each user along with their popularity number. Order records in descending order by user id.

The 'user1' and 'user2' column are pairs of friends.

Hint :
- We need to create subqueries or CTEs to calculate total number of friend per user
- To calculate total number of friend per user, create one column that contains all users and a second column that contains all their friends. You can accomplish this by user1 union user2 columns.
- Lastly, implement the percentage popularity formula by counting the number of friends per user and dividing by the total number of users on the platform.


```sql
-- popularity_pct for each user
-- popularity_pct as (total_friends / number_user)*100

WITH cte1 AS(
    SELECT user1,
            COUNT(*)as friend
    FROM facebook_friends
    GROUP BY 1
    ORDER BY 1
),
cte2 AS(    
    SELECT user2 AS user1,
            COUNT(*)as friend
    FROM facebook_friends
    GROUP BY 1
    ORDER BY 1
),
users AS(
    SELECT *
    FROM cte1
    UNION
    SELECT *
    FROM cte2
)
    SELECT user1,
            (SUM(friend)::FLOAT * 100 / COUNT(user1) OVER())::FLOAT as popularity_pct
    FROM users  
    GROUP BY 1
    ORDER BY 1;
```

### π Microsoft | Interview Questions | Premium vs Freemium
[Question: ](https://platform.stratascratch.com/coding/10300-premium-vs-freemium?code_type=1) Find the total number of downloads for paying and non-paying users by date. Include only records where non-paying customers have more downloads than paying customers.

The output should be sorted by earliest date first and contain 3 columns date, non-paying downloads, paying downloads.

```sql
SELECT a.date,
        SUM(CASE WHEN c.paying_customer = 'no' THEN a.downloads END)as non_paying,
        SUM(CASE WHEN c.paying_customer = 'yes' THEN a.downloads END)as paying
FROM ms_download_facts a        
JOIN ms_user_dimension b ON a.user_id = b.user_id
JOIN ms_acc_dimension c ON b.acc_id = c.acc_id
GROUP BY 1
HAVING SUM(CASE WHEN c.paying_customer = 'no' THEN a.downloads END) > SUM(CASE WHEN c.paying_customer = 'yes' THEN a.downloads END)
    ORDER BY 1;
```

### π Yelp | Interview Questions | Top 5 States With 5 Star Businesses
[Question: ](https://platform.stratascratch.com/coding/10046-top-5-states-with-5-star-businesses?code_type=1) Find the top 5 states with the most 5 star businesses.

Output the state name along with the number of 5-star businesses and order records by the number of 5-star businesses in descending order.

In case there are ties in the number of businesses, return all the unique states. If two states have the same result, sort them in alphabetical order.

```sql
WITH CTE AS(    
    SELECT DISTINCT state,
            COUNT(stars)as number_5star,
            RANK() OVER(ORDER BY COUNT(stars) desc)as ranking
    FROM yelp_business
    WHERE stars = 5
    GROUP BY 1
    ORDER BY 2 DESC, 1 ASC
)
    SELECT state,
            number_5star
    FROM CTE
    WHERE ranking <=5;
```




