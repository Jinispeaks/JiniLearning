Row Context and Filter Context: The DAX Essentials

Row context is the context that is applied to each individual row of a table when a formula is evaluated. 
For example, if you have a table of sales data with columns for date, product, quantity, and price, and you want to calculate the total sales for each row, you can use a formula like this:

Total Sales = Quantity * Price

This formula will multiply the quantity and price values for each row and return the total sales. The row context is determined by the row that the formula is evaluated on.

Filter context is the context that is applied to a whole table or column when a formula is evaluated. 
Filter context is usually created by slicers, visuals, or filters that limit the data that is available for calculation.
 For example, if you have a slicer that lets you select a product category, and you want to calculate the average sales for each product category, you can use a formula like this:

Average Sales = AVERAGE(Total Sales)

This formula will calculate the average of the total sales column for each product category. The filter context is determined by the slicer that filters the sales table by product category.

One way to think about the difference between row context and filter context is row context operates on one row at a time, while filter context operates on a set of rows at once. Another way to think about it is that row context is defined by the structure of the table, while filter context is defined by the user's interaction with the data.

To illustrate the difference between row context and filter context with an example, let's say we have a table of sales data like this:
Sales Table
Image by author

If we want to calculate the total sales for each row, we can use a formula like this:

Total Sales = Sales[Quantity] * Sales[Price]

This formula will use the row context to multiply the quantity and price values for each row and return the total sales. The result will look like this:
Sales Table with Total Sales columns

If we want to calculate the average sales for each category, we can use a formula like this:

Average Sales = AVERAGE(Sales[Total Sales])

This formula will use the filter context to calculate the average of the total sales column for each category. The result will look like this:
No alt text provided for this image

Notice how the filter context changes depending on the category value. For category X, only the rows with category X are included in the calculation. Similarly, for category Y and Z

In summary, row context and filter context are two different ways of applying context to a DAX formula. 
Row context operates on one row at a time and is determined by the structure of the table.
Filter context operates on a set of rows at once and is determined by the user's interaction with the data
