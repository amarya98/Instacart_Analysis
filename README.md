# Instacart Customer Analytics 
<img width="371" alt="Logotype" src="https://user-images.githubusercontent.com/84459190/139473109-ad72aadc-ac82-4ec2-8363-09c616ea17ea.png"> 

I was curious about looking at what over 200,000 Instacart users purchase, how frequently they order based off of over 3 million orders placed, and how some of my favorite products rank against over the 49,000 products available to purchase. To do this analysis, I used a dataset available on [Kaggle](https://www.kaggle.com/c/instacart-market-basket-analysis/data). 

P.S. take a look at my interactive Tableau dashboard which visualizes all of my findings from my analysis. Check it out [here](https://public.tableau.com/app/profile/anisha.marya/viz/InstacartAnalysis_16350198341140/InstacartDashboard)!


## Ordering Patterns

I was interested in knowing more about whether more people place orders on a weekly or a monthly basis; to find this out, I wrote the following query: 

```
SELECT days_since_prior_order, count(user_id)
FROM "amarya9872/BristleClicker"."orders.csv"
GROUP BY days_since_prior_order;
```

I made this visual to show that most reorders tend to happen on either a weekly or monthly basis.

<img width="531" alt="Screen Shot 2021-10-29 at 7 50 34 PM" src="https://user-images.githubusercontent.com/84459190/139512274-f158e8dc-2981-43e6-ab4c-02628194cd40.png">

## Number of Orders Per Day and Per Hour

Building on my curiosity about the ordering patterns, I wanted to write a query that would show me how many orders were placed each hour for every day of the week:

```
SELECT order_hour_of_day, order_dow, count(*) 
FROM "amarya9872/BristleClicker"."orders.csv"
GROUP BY order_hour_of_day, order_dow;
```

I visualized my findings (which is interactive on my [dashboard](https://public.tableau.com/app/profile/anisha.marya/viz/InstacartAnalysis_16350198341140/InstacartDashboard)) to show that Saturday and Sunday had the highest amount of orders placed at over 50,000, while the weekdays were all under 40,000 orders per day and orders for all days of the week were placed; between 10AM and 2PM.

<img width="699" alt="Screen Shot 2021-11-01 at 12 49 00 AM" src="https://user-images.githubusercontent.com/84459190/139623144-f5a72c0e-db3c-4413-bac4-d353a9fa1344.png">


## Top Cookies Purchased 

One of my favorite things to buy are cookies, so I wanted to see what the top purchased cookie was:

```
SELECT Products."product_name", count(*) 
FROM "amarya9872/BristleClicker"."order_products__train.csv" AS Orders
LEFT JOIN "amarya9872/BristleClicker"."products.csv" AS Products
ON Orders."product_id" = Products."product_id"
WHERE Products."product_name" LIKE '%Cookies%' OR Products."product_name" LIKE '%Cookie%'
GROUP BY Products."product_name"
ORDER BY count DESC;
```

The bar graph below shows that the Classic Chocolate Chip Cookie was the most purchased while my favorite cookie, Oreos, were the fourth most popular. If you go to my [dashboard](https://public.tableau.com/app/profile/anisha.marya/viz/InstacartAnalysis_16350198341140/InstacartDashboard) be able to scroll through and see if your favorite cookie made the top 20!

<img width="350" alt="Screen Shot 2021-10-29 at 8 06 49 PM" src="https://user-images.githubusercontent.com/84459190/139512904-08201fcd-059b-46a6-bff5-635202c6be83.png">

## Top Products by Department

Instacart has over 20 departments and over 49,000 products listed, I was interested in seeing what the top 20 products were for each department and how many of each product were bought:

```
SELECT product_name, department, num_purchases, product_rank FROM(
  SELECT * FROM(
    SELECT p."product_name", p."department_id", purchases."num_purchases", 
    row_number() over(partition by department_id order by num_purchases DESC) AS product_rank
    FROM(
        SELECT Products."product_id", count(*) AS num_purchases
      FROM "amarya9872/BristleClicker"."order_products__train.csv" AS Train
      LEFT JOIN "amarya9872/BristleClicker"."products.csv" AS Products
      ON Train."product_id" = Products."product_id"
      GROUP BY Products."product_id"
    ) purchases JOIN "amarya9872/BristleClicker"."products.csv" p ON p."product_id" = purchases."product_id"
  ) department_ranks
  WHERE department_ranks."product_rank" <= 20) top_in_department 
  JOIN "amarya9872/BristleClicker"."departments.csv" 
  ON top_in_department."department_id" = "amarya9872/BristleClicker"."departments.csv"."department_id";
  ```
  
  I created this treemap to show how each departments top products rank, the department with the snacks is shown here since it's a personal favorite. This is an interactive [dashboard](https://public.tableau.com/app/profile/anisha.marya/viz/InstacartAnalysis_16350198341140/InstacartDashboard) and other departments products been seen by visiting it. 
  
  <img width="820" alt="Screen Shot 2021-11-01 at 1 54 36 AM" src="https://user-images.githubusercontent.com/84459190/139627830-3385fa44-0098-4e31-9d9f-76b55949ddf8.png">

## Coke vs Pepsi

I wanted to know what the more popular soda was between Coca-Cola and Pepsi. 

```
SELECT Products."product_name", count(*)
FROM "amarya9872/BristleClicker"."order_products__train.csv" AS Train
LEFT JOIN "amarya9872/BristleClicker"."products.csv" AS Products
ON Train."product_id" = Products."product_id"
LEFT JOIN "amarya9872/BristleClicker"."orders.csv" AS Orders
ON Train."order_id" = Orders."order_id"
WHERE Products."product_name" LIKE '%Coke%' OR Products."product_name" LIKE '%Pepsi%'
GROUP BY Products."product_name"
ORDER BY count DESC;
```
The bar graph shows that Classic Coke and other Coke varieties overall were purchased more than Pepsi.

<img width="385" alt="Screen Shot 2021-11-01 at 2 27 05 AM" src="https://user-images.githubusercontent.com/84459190/139630612-128428db-72f3-49a3-a821-0fec18b7df3c.png">
