1. order by 

   * FIELD

2. like

   * 用\进行转义

     ```mysql
     SELECT 
         productCode, 
         productName
     FROM
         products
     WHERE
         productCode LIKE '%\_20%';
     ```

3. IS NULL

   * MySQL会利用索引对IS NULL的查询做优化。

4. CROSS JOIN

   