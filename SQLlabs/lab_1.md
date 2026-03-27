# LAB : 1 
 SQL injection vulnerablity in Where clause allowing reterival of hidden data :

End Goals : 
(i) Disclose the unrelased product .

Vulnerablity :
SQL injection vulnerablity in product category.

Given Query : SELECT * FROM products WHERE category = 'Gifts' AND released = 1

Solution :
(i) Capture the request in Repeater.
(ii) In this request of category where we find a parameter of category.
        category=Food+%26+Drink
    
Analysis : According to the given query if product is relased it show 1 (relased = 1) and if not relased (relased = 0) in the query.

(iii) So we finding unrelased we want to true this query for all product. 
        modified query : SELECT * FROM products WHERE category = 'Gift' OR 1=1-- ' AND released = 1
        
        in this query we want first query is true either other is true and comment the rest query.

        so our payload is  :  ' OR 1=1 -- 
        and this payload is category parameter in request captured and also put directly in url.
