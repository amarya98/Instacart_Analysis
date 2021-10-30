# Instacart Customer Analytics 
<img width="371" alt="Logotype" src="https://user-images.githubusercontent.com/84459190/139473109-ad72aadc-ac82-4ec2-8363-09c616ea17ea.png"> 

I was curious about looking at what products Instacart users purchase, how frequently they order, and how some of my favorite products rank. To do this analysis, I used a dataset available on [Kaggle](https://www.kaggle.com/c/instacart-market-basket-analysis/data). 

P.S. take a look at my interactive Tableau dashboard which visualizes all of my findings from my analysis. Check it out [here](https://public.tableau.com/app/profile/anisha.marya/viz/InstacartAnalysis_16350198341140/InstacartDashboard)!


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

![Screen Shot 2021-10-29 at 1 01 01 PM](https://user-images.githubusercontent.com/84459190/139511856-508f8795-f1d9-4e35-a3c8-e8d4e49fa0d5.png)

## Ordering Patterns

I was interested in knowing more about whether more people place orders on a weekly or a monthly basis; to find this out, I wrote the following query: 

```
SELECT days_since_prior_order, count(user_id)
FROM "amarya9872/BristleClicker"."orders.csv"
GROUP BY days_since_prior_order;
```

I made this visual to show that most reorders tend to happen on either a weekly or monthly basis.

<img width="531" alt="Screen Shot 2021-10-29 at 7 50 34 PM" src="https://user-images.githubusercontent.com/84459190/139512274-f158e8dc-2981-43e6-ab4c-02628194cd40.png">

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


