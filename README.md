# Instagram User Analytics

This repository contains SQL queries and analyses for understanding user behavior on Instagram. The project covers various tasks aimed at marketing strategies and investor metrics, such as identifying the most loyal users, finding inactive users, determining the most liked photo, researching popular hashtags, and more.

## Table of Contents

- [Introduction](#introduction)
- [Tasks](#tasks)
  - [Marketing](#marketing)
  - [Investor Metrics](#investor-metrics)
- [Queries and Results](#queries-and-results)
  - [Marketing Queries](#marketing-queries)
  - [Investor Metrics Queries](#investor-metrics-queries)
- [Conclusion](#conclusion)
- [Acknowledgements](#acknowledgements)

## Introduction

Instagram User Analytics project involves running SQL queries to analyze user data from Instagram. The primary focus is on marketing strategies and investor metrics to provide insights into user engagement and platform usage.

## Tasks

### Marketing

1. **Rewarding the Most Loyal Users**
   - Identify the 5 oldest users of Instagram.

2. **Remind Inactive Users to Start Posting**
   - Find users who have never posted a single photo on Instagram.

3. **Declaring Contest Winner**
   - Find which user got the most likes on a single photo.

4. **Hashtag Researching**
   - Identify and suggest the top 5 most commonly used tags on the platform.

5. **Launch AD campaign**
   - Determine which day most users register on the platform.

### Investor Metrics

1. **User Engagement**
   - Calculate the average number of photos posted by users and show the total number of photos and users.

2. **Bots & Fake accounts**
   - Identify users who liked every photo.

## Queries and Results

### Marketing Queries

1. **Rewarding the Most Loyal Users**
   ```sql
   SELECT *
   FROM users
   ORDER BY created_at
   LIMIT 5;
   ```
   - **Conclusion**: Users with id 80, 67639538 are the 5 oldest users of Instagram.

2. **Remind Inactive Users to Start Posting**
   ```sql
   SELECT users.username AS Username, Photos.id
   FROM users
   LEFT JOIN photos ON users.id = photos.user_id
   WHERE photos.id IS NULL;
   ```
   - **Conclusion**: The mentioned users didn’t post any photos (photo_id is NULL).

3. **Declaring Contest Winner**
   ```sql
   SELECT
   username,
   photos.id AS Photo_ID, photos.image_url, COUNT(*) AS Total_likes
   FROM photos
   INNER JOIN likes ON likes.photo_id = photos.id
   INNER JOIN users ON photos.user_id = users.id
   GROUP BY photos.id
   ORDER BY Total_likes DESC
   LIMIT 1;
   ```
   - **Conclusion**: Photo_ID 145 by user Zack_Kemmer93 received the most likes.

4. **Hashtag Researching**
   ```sql
   SELECT tag_name, COUNT(tag_name) AS total_tag_usage
   FROM tags
   INNER JOIN photo_tags ON tags.id = photo_tags.tag_id
   GROUP BY tags.id
   ORDER BY total_tag_usage DESC
   LIMIT 5;
   ```
   - **Conclusion**: The most used hashtags are smile, beach, party, fun, and concert.

5. **Launch AD campaign**
   ```sql
   SELECT dayname(created_at) AS Day, count(*) AS total_registration
   FROM users
   GROUP BY Day
   ORDER BY total_registration DESC
   LIMIT 2;
   ```
   - **Conclusion**: Most user registrations occur on Thursday and Sunday.

### Investor Metrics Queries

1. **User Engagement**
   ```sql
   SELECT
   (SELECT COUNT(*) FROM photos) / (SELECT COUNT(*) FROM users) AS AVG_photo_upload;
   ```
   - **Conclusion**: The average number of photos uploaded by a user is 2.57.

2. **Bots & Fake accounts**
   ```sql
   SELECT users.id, username, COUNT(users.id) AS total_likes_given_by_user
   FROM users
   INNER JOIN likes ON users.id = likes.user_id
   GROUP BY users.id
   HAVING total_likes_given_by_user = (SELECT COUNT(*) FROM photos);
   ```
   - **Conclusion**: The mentioned users liked every single photo.

## Conclusion

This project provided valuable insights into Instagram user behavior, helping to develop marketing strategies and investor metrics. The SQL queries enabled efficient analysis of user data, highlighting key trends and patterns.

## Acknowledgements

Thank you for reviewing this project. It was a great learning experience in SQL and data analysis.

**Author**: Ayush Yadav

---

Feel free to reach out for any questions or further discussions on this project.
