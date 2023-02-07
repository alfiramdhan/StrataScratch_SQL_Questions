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

### 📌 📌 Meta/Facebook | Interview Questions | Popularity of Hack
[Question: ](https://platform.stratascratch.com/coding/10061-popularity-of-hack?code_type=1) Meta/Facebook has developed a new programing language called Hack.To measure the popularity of Hack they ran a survey with their employees. The survey included data on previous programing familiarity as well as the number of years of experience, age, gender and most importantly satisfaction with Hack. Due to an error location data was not collected, but your supervisor demands a report showing average popularity of Hack by office location. Luckily the user IDs of employees completing the surveys were stored.
Based on the above, find the average popularity of the Hack per office location.
Output the location along with the average popularity.

```sql





