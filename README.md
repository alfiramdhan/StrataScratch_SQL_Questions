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










