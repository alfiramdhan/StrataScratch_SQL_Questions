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

### 📌 Meta/Facebook | Interview Questions | Users By Average Session Time
[Question: ](https://platform.stratascratch.com/coding/10352-users-by-avg-session-time?tabname=solutions&code_type=1) Calculate each user's average session time. A session is defined as the time difference between a page_load and page_exit. For simplicity, assume a user has only 1 session per day and if there are multiple of the same events on that day, consider only the latest page_load and earliest page_exit.

Output the user_id and their average session time.

```SQL
WITH CTE AS(
    SELECT user_id,
            DATE(timestamp)as date,
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














