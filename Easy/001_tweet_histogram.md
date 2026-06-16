# Tweet Histogram

## Problem

Assume you're given a table Twitter tweet data, write a query to obtain a histogram of tweets posted per user in 2022.

---

## Table Schema

### tweets

| Column Name | Type |
|------------|------|
| tweet_id | integer |
| user_id | integer |
| msg | string |
| tweet_date | timestamp |

`tweets` Example Input:
| tweet_id  | user_id | msg                                                              | tweet_date |
|----------|---------|-------------------------------------------------------------------|------------|
|214252    | 111     | Am considering taking Tesla private at $420. Funding secured.     | 12/30/2021 |
|739252    | 111     | Despite the constant negative press covfefe                       | 01/01/2022 |
|846402    | 111     | Following @NickSinghTech on Twitter changed my life!              | 02/14/2022 |
|241425    | 254     | If the salary is so competitive why won’t you tell me what it is? | 03/01/2022 |
|231574    | 148     | I no longer have a manager. I can't be managed                    | 03/23/2022 |

Example Output:
| tweet_bucket | users_num |
|--------------|-----------|
| 1	| 2 |
| 2 | 1 |


---

## Sample Data

```sql
CREATE TABLE tweets (
    tweet_id INT,
    user_id INT,
    msg VARCHAR(100),
    tweet_date DATETIME
);

INSERT INTO tweets VALUES
(214252,111,'Am considering taking Tesla private at $420. Funding secured.','2021-12-30 00:00:00'),
(739252,111,'Despite the constant negative press covfefe','2022-01-01 00:00:00'),
(846402,111,'Following @NickSinghTech on Twitter changed my life!','2022-02-14 00:00:00'),
(241425,254,'If the salary is so competitive why won''t you tell me what it is?','2022-03-01 00:00:00'),
(231574,148,'I no longer have a manager. I can''t be managed','2022-03-23 00:00:00');
```

---

## My Thought Process

1. Filter tweets from 2022.
2. Count tweets per user.
3. Create a histogram from tweet counts.
4. Group by tweet_count and count users.

---

## Final Solution

```sql
SELECT
    tweet_count AS tweet_bucket,
    COUNT(*) AS users_num
FROM (
    SELECT
        user_id,
        COUNT(*) AS tweet_count
    FROM tweets
    WHERE YEAR(tweet_date) = 2022
    GROUP BY user_id
) t
GROUP BY tweet_count;
```

---

## Key Learnings

- Nested aggregation
- Subqueries
- COUNT(*)
- GROUP BY
- Histogram-style problems
