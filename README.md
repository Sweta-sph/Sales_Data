# Sales_Data

 ---Give Rank and Dense_Rank to the total amount----
 ---paid by each customer----
 select Customer_Id, Amount_Paid, 
 		    rank() over(order by Amount_Paid) as rankes, 
        dense_rank() over(order by Amount_Paid) 
  from ProductSalesFact;


---Get the number of days a product is in inventory---
select Product_Id, Category_Id, datediff(current_date(),In_Inventory) as days, 
	      row_number() over(order by Category_Id, datediff(current_date(),In_Inventory)) as Row_No 
from ProductDim
order by Category_Id, datediff(current_date(),In_Inventory) desc;


---Get the total Amount customer paid---
---in each Usage_category each week----
select distinct t.Cust_Usage, t.* 
	from (
			select sum(Amount_Paid) over( partition by Cust_Usage, extract(week from dateofPurchase)) as SumOfWeek, 
 			        extract(week from DateofPurchase) as WeekNo, Cust_Usage
       from ProductSalesFact
       order by extract(week from DateofPurchase),Cust_Usage
       ) as t;

       

