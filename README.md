# ♾ StrataScratch Interview Questions

[StrataScratch](https://www.stratascratch.com) is a data science platform with real interview questions from top companies.

Here I will solve SQL questions and share my answers.


***

## Difficulty : Easy

### 📌 Apple | General Practice | Count the number of user events performed by MacBookPro users
[Question: ](https://platform.stratascratch.com/coding/9653-count-the-number-of-user-events-performed-by-macbookpro-users?code_type=1) Count the number of user events performed by MacBookPro users. Output the result along with the event name. Sort the result based on the event count in the descending order.

```sql
SELECT event_name,
        COUNT(user_id)as number_user
FROM playbook_events
WHERE device = 'macbook pro'
GROUP BY 1
ORDER BY 2 desc;
```

### 📌 Airbnb | Interview Questions | Number Of Bathrooms And Bedrooms
[Question: ](https://platform.stratascratch.com/coding/9622-number-of-bathrooms-and-bedrooms?code_type=1) Find the average number of bathrooms and bedrooms for each city’s property types. Output the result along with the city name and the property type.

```sql
SELECT city,
        property_type,
        AVG(bathrooms)as avg_bathroom,
        AVG(bedrooms)as avg_bedroom
FROM airbnb_search_details
GROUP BY 1,2;
```

### 📌 Amazon | Interview Questions | Order Details
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

### 📌 Amazon | Interview Questions | Customer Details
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

### 📌 Meta/Facebook | General Practice | Find all posts which were reacted to with a heart
[Question: ](https://platform.stratascratch.com/coding/10087-find-all-posts-which-were-reacted-to-with-a-heart?code_type=1) Find all posts which were reacted to with a heart. For such posts output all columns from facebook_posts table.

```sql
SELECT DISTINCT b.*
FROM facebook_reactions a
JOIN facebook_posts b ON a.post_id = b.post_id
WHERE a.reaction = 'heart';
```
In solution above, we need to add DISTINCT so that there is no data duplication

### 📌 Meta/Facebook | Interview Questions | Popularity of Hack
[Question: ](https://platform.stratascratch.com/coding/10061-popularity-of-hack?code_type=1) Meta/Facebook has developed a new programing language called Hack.To measure the popularity of Hack they ran a survey with their employees. The survey included data on previous programing familiarity as well as the number of years of experience, age, gender and most importantly satisfaction with Hack. Due to an error location data was not collected, but your supervisor demands a report showing average popularity of Hack by office location. Luckily the user IDs of employees completing the surveys were stored.

Based on the above, find the average popularity of the Hack per office location. Output the location along with the average popularity.

```sql
SELECT location,
        avg(popularity)as avg_popularity
FROM facebook_employees a
JOIN facebook_hack_survey b ON a.id = b.employee_id
GROUP BY 1;
```

### 📌 Netflix | General Practice | Count the number of movies that Abigail Breslin nominated for oscar
[Question: ](https://platform.stratascratch.com/coding/10128-count-the-number-of-movies-that-abigail-breslin-nominated-for-oscar?code_type=1) Count the number of movies that Abigail Breslin was nominated for an oscar.

```sql
select nominee,
       Count(movie)as number_movie
from oscar_nominees
where nominee LIKE 'Abigail Breslin'
group by 1;
```

### 📌 Spotify | General Practice | Find how many times each artist appeared on the Spotify ranking list
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

### 📌 Airbnb | General Practice | Find matching hosts and guests in a way that they are both of the same gender and nationality
[Question: ](https://platform.stratascratch.com/coding/10078-find-matching-hosts-and-guests-in-a-way-that-they-are-both-of-the-same-gender-and-nationality?code_type=1) Find matching hosts and guests pairs in a way that they are both of the same gender and nationality.
Output the host id and the guest id of matched pair.

```sql
SELECT DISTINCT host_id,
        guest_id
FROM airbnb_hosts a
JOIN airbnb_guests b ON a.nationality = b.nationality
    and a.gender = b.gender;
```

### 📌 Airbnb | General Practice | Ranking Most Active Guests
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

### 📌 Airbnb | General Practice | Number Of Units Per Nationality
[Question: ](https://platform.stratascratch.com/coding/10156-number-of-units-per-nationality?code_type=1) Find the number of apartments per nationality that are owned by people under 30 years old.

Output the nationality along with the number of apartments. Sort records by the apartments count in descending order.

```SQL
SELECT nationality,
        COUNT(DISTINCT unit_id)as number_apart
FROM airbnb_hosts a
JOIN airbnb_units b ON a.host_id = b.host_id
WHERE b.unit_type = 'Apartment'
    AND a.age < 30
GROUP BY 1
ORDER BY 2 DESC;
```

### 📌 Amazon | Interview Questions | Finding User Purchases
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

### 📌 Amazon | Interview Questions | Highest Cost Orders
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

### 📌 DoorDash | Interview Questions | Workers With The Highest Salaries
[Question: ](https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries?code_type=1) Find the titles of workers that earn the highest salary. Output the highest-paid title or multiple titles that share the highest salary.

```sql
SELECT worker_title
FROM worker a
JOIN title b ON a.worker_id = b.worker_ref_id
    WHERE salary = (SELECT MAX(salary) FROM worker);
```

### 📌 Meta/Facebook | Interview Questions | Users By Average Session Time
[Question: ](https://platform.stratascratch.com/coding/10352-users-by-avg-session-time?tabname=solutions&code_type=1) Calculate each user's average session time. A session is defined as the time difference between a page_load and page_exit. For simplicity, assume a user has only 1 session per day and if there are multiple of the same events on that day, consider only the latest page_load and earliest page_exit.

Output the user_id and their average session time.

```SQL
WITH CTE AS(
    SELECT user_id,
            DATE(timestamp)as date,
            
-- since we need lates page_load and earlist page_exit so we use MIN() and MAX() function
            MAX(CASE WHEN action = 'page_load' then timestamp end)as pg_load,
            MIN(CASE WHEN action = 'page_exit' then timestamp end)as pg_exit
    FROM facebook_web_log
    GROUP BY 1,2
)
    SELECT user_id,
            AVG(pg_exit - pg_load)as avg_session
    FROM CTE
    GROUP BY 1
    HAVING AVG(pg_exit - pg_load) IS NOT NULL;
```    

### 📌 Meta/Facebook | Interview Questions | Customer Revenue In March
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

### 📌 Meta/Facebook | Interview Questions | Highest Energy Consumption
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

### 📌 Meta/Facebook | Interview Questions | Acceptance Rate By Date
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

### 📌 Meta/Facebook | General Practice | Find the rate of processed tickets for each type
[Question: ](https://platform.stratascratch.com/coding/9781-find-the-rate-of-processed-tickets-for-each-type?tabname=question) Find the rate of processed tickets for each type.

```SQL
SELECT type,
        SUM(CASE WHEN processed=True THEN 1 else 0 end)::numeric / COUNT(*)as rate_processed
FROM facebook_complaints
GROUP BY 1;
```

### 📌 Spotify | General Practice | Find the top 10 ranked songs in 2010
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

### 📌 Spotify | General Practice | Top Ranked Songs
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

### 📌 Yelp | General Practice | Top Businesses With Most Reviews
[Question: ](https://platform.stratascratch.com/coding/10048-top-businesses-with-most-reviews?code_type=1) Find the top 5 businesses with most reviews. Assume that each row has a unique business_id such that the total reviews for each business is listed on each row.

Output the business name along with the total number of reviews and order your results by the total reviews in descending order.

```sql
SELECT name,
        review_count
FROM yelp_business
ORDER BY 2 DESC
LIMIT 5;
```

### 📌 Yelp | Interview Questions | Top Cool Votes
[Question: ](https://platform.stratascratch.com/coding/10060-top-cool-votes?code_type=1) Find the review_text that received the highest number of  'cool' votes.

Output the business name along with the review text with the highest numbef of 'cool' votes.

```sql
SELECT business_name,
        review_text
 FROM yelp_reviews        
    WHERE cool = (SELECT MAX(cool)
                    FROM yelp_reviews);
 ```
 
 ### 📌 Yelp | Interview Questions | Reviews of Categories
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

### 📌 Airbnb | Interview Questions | Host Popularity Rental Prices
[Question: ](https://platform.stratascratch.com/coding/9632-host-popularity-rental-prices?code_type=1) You’re given a table of rental property searches by users. The table consists of search results and outputs host information for searchers. Find the minimum, average, maximum rental prices for each host’s popularity rating. The host’s popularity rating is defined as below:
0 reviews: New
1 to 5 reviews: Rising
6 to 15 reviews: Trending Up
16 to 40 reviews: Popular
more than 40 reviews: Hot

Tip: The id column in the table refers to the search ID. You'll need to create your own host_id by concating price, room_type, host_since, zipcode, and number_of_reviews.

Output host popularity rating and their minimum, average and maximum rental prices.

Hint :
- You’ll need to create a few where each host (USING CONCAT() function) is listed only once. So taking the average, min, max without de-duplicating would skew the results.
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

### 📌 Amazon | Interview Questions | Marketing Campaign Success [Advanced]
[Question: ](https://platform.stratascratch.com/coding/514-marketing-campaign-success-advanced?code_type=1) You have a table of in-app purchases by user. Users that make their first in-app purchase are placed in a marketing campaign where they see call-to-actions for more in-app purchases. Find the number of users that made additional in-app purchases due to the success of the marketing campaign.

The marketing campaign doesn't start until one day after the initial in-app purchase so users that only made one or multiple purchases on the first day do not count, nor do we count users that over time purchase only the products they purchased on the first day.

```sql

```











