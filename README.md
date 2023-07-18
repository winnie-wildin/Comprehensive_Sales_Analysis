# Comprehensive_Sales_Analysis
Analyzing a sales dataset to answer a few business questions.
- Jupyter notebook: **Sales Insights_A Comprehensive Sales Analysis.ipynb**
# Table of contents
1. [Introduction](#introduction)

2. [Description of the data set](#section2)
    1. [Initial steps](#sec2p1)
    2. [Data Preprocessing I](#sec2p2)
    3. [Data Preprocessing II](#sec2p3)
    
3. [Questions Answered](#section3)
    1. [What are the top-selling products in terms of quantity_ordered and sales?](#sec3p1)
    2. [Which month had the highest sales and which had the lowest sales?](#sec3p2)
    3. [What are the 3 top-selling products in each of the cities?](#sec3p3)
    4. [Is there a noticeable difference in product prices during peak hours or specific days?](#sec3p4)
    5. [What is the correlation between quantity ordered and sales revenue?](#sec3p5)
    6. [How does the Price of Each Item and the Quantity Ordered Influence Sales?](#sec3p6)
    7. [Price Elasticity](#sec3p7)
    
4. [Conclusion](#conclusion)

5. [References](#references)

## 1. Introduction <a name="introduction"></a>
- This README describes work done on the sales data sets. Resources used include Python and associated packages Jupyter, Pandas, Numpy, matplotlib, Seaborn, statsmodels.
- The analysis takes the form of a single Jupyter notebook of filename given above. To view this file, download it from this repository and start Jupyter notebook in the folder containing the file. Use the command **Jupyter notebook** on the command line. 
- Alternatively, view a static version of the notebook (by providing its GitHub url) using Jupyter Nbviewer. 
- The sales data sets was included in one of the videos on Keith Galli's Youtube chanel. I downloaded it to my local machine.
- All images intended for inclusion in this README are located in the **images** subdirectory of this repository.
- I have tried to structure the Jupyter notebook and this README so that they have corresponding sections. However, I do not wish to merely repeat here what has been stated in the notebook. I will endeavour to have this README summarize the work of the notebook and, hopefully, complement the analyses done there.

##  2. Description of the data set <a name="section2"></a>
The data in the sales data sets was gathered over a period of 12 months in 2019. It contains 186850 rows of data combined relating to purchases of electronic items. I'm pretty certain that this is an American data set so I will assume that the currency is $. Information within includes the Order ID, Product, Quantity Ordered, Price of Each item, Order Date, Purchase Address. 

### 2.1 Initial steps <a name="sec2p1"></a>
The original data was spread out in twelve csv files for each month. Therefore, I had to consolidate all of them into a single comprehensive csv file for further analysis or processing. Through a loop, each CSV file was read using pandas and appended to the "all_months_data" DataFrame. Conversely, if the output file already existed, the process was skipped and a message was printed accordingly.
The very first step is always to check if the data needs cleaning by looking for duplicate rows, zero values or NaNs where they shouldn't be, etc. The head of the data set looks like:

![head](https://github.com/winnie-wildin/Comprehensive_Sales_Analysis/assets/110402296/4db9b700-f21a-4fee-b59c-9af9c155b37a)


### 2.2 Data preprocessing: Cleaning and Filtering <a name="sec2p2"></a>
Pandas **info()** can provide a concise summary of a DataFrame. It displays information about the DataFrame's structure, including the number of rows and columns, column names, data types of columns, and the amount of non-null values in each column. It also provides an estimate of the memory usage of the DataFrame. The output of pandas **info()**before and after the cleaning is shown below. Here, all columns of the DataFrame are included in the analysis.

![info_before](https://github.com/winnie-wildin/Comprehensive_Sales_Analysis/assets/110402296/ff01cf8a-2a02-4a0f-8e27-9b85bcedcf0b)

![info_after](https://github.com/winnie-wildin/Comprehensive_Sales_Analysis/assets/110402296/a0727842-e08e-4a12-99f5-8b6108f232e7)


### 2.3 Data preprocessing: Transformation and Restructuring <a name="sec2p3"></a>
Several transformations were performed on the DataFrame, resulting in the creation of a modified DataFrame named df_modified. The following actions were carried out to derive a more informative and structured dataset:

- A new column named 'month' was added, derived from the 'order_date' column. This column captured the specific month corresponding to each order.

- Another column called 'sales' was introduced, calculated by multiplying the 'price_each' and 'quantity_ordered' columns. This provided the total sales amount for each order.

- A 'city' column was created based on the information extracted from the 'purchase_address' column. This column represented the city where each purchase was made.

- Additionally, a 'time' column was generated from the 'order_date' column to capture the specific time of each order.

- To eliminate redundancy and reduce unnecessary information, the 'purchase_address' and 'order_date' columns were removed since the relevant details were already extracted and stored in separate columns.

- Finally, the index of the DataFrame was reset to ensure a consistent and sequential numbering of rows.
![df_before_transformation](https://github.com/winnie-wildin/Comprehensive_Sales_Analysis/assets/110402296/f5e3c05e-5aa0-48b2-9854-383fd50a143d)

![df_after_transformation](https://github.com/winnie-wildin/Comprehensive_Sales_Analysis/assets/110402296/72c32840-5992-44a2-b977-173949106b1a)

## 3. The Code explained <a name="section3"></a>
I have tried to explain the intuition behind the code and visualizations. To learn about the final answers to these questions, kindly refer to the notebook.

### 3.1 What are the top-selling products in terms of quantity_ordered and sales? <a name="sec3p1"></a>
- This question tries to identify the top-selling products by their quantity_ordered and sales, allowing us to determine the most popular and profitable items based on the provided data. 

- I have used the **groupby()** function to group the data in the df_modified DataFrame by the 'product' column. Then, applied the **agg()** function to calculate the sum of 'quantity_ordered' and 'sales' for each unique product.

- The resulting DataFrame contains the aggregated values for quantity_ordered and sales for each product. The **nlargest()** function is applied to retrieve the **top 5** products based on the quantity_ordered, sorting them in descending order.

- The horizontal bar chart is created by using the **plt.barh()** function from Matplotlib. The chart's horizontal bars represent the products, while their lengths correspond to the respective quantity_ordered values



### 3.2 Which month had the highest sales and which had the lowest sales? <a name="sec3p2"></a>

- This one calculates the total sales for each month in the 'df_modified' DataFrame and identifies the month with the highest and lowest sales.

- First, I grouped the data in the 'df_modified' DataFrame by the 'month' column using the **groupby()** function. Then I applied the **agg()** function to calculate the sum of the 'sales' column for each month resulting in the dataframe called sales_by_month. 
To identify the month with the highest sales and lowest sales, I used the **idxmax()** and **idxmin()** functions respectively.

### 3.3 What are the 3 top-selling products in each of the cities? <a name="sec3p3"></a>

- Here I visualized and compared the top three selling products in each city, providing insights into the popular products within different regions.

- First I defined a function named **plot_top_products()**. This function accepts two essential parameters: **city_data**, which represents the dataset specific to a particular city, and **city_name**, denoting the name of the city under consideration.

- Within the **plot_top_products()** function, I employed grouping, which entails categorizing the city_data by product and calculating the sum of quantity_ordered for each product. This aggregation process was achieved by leveraging the **groupby()** function coupled with the **sum()** method. By doing so, I obtained an insightful DataFrame named **product_counts**.

- To further narrow down our focus to the top performers, I utilized the **nlargest()** function. This enabled the selection of the three products exhibiting the highest quantity_ordered values from the product_counts DataFrame, resulting in a refined DataFrame known as **top_products_city**.

### 3.4 Is there a noticeable difference in product prices during peak hours or specific days? <a name="sec3p4"></a>
- This part visualizes the variations in average prices throughout different hours of the day, allowing for a clear understanding of when prices tend to peak. The annotations further emphasize the two hours with the highest average prices.
- To achieve this, I created a new column called 'hour_of_day' in the 'df_modified' DataFrame. Utilizing a **lambda function**, I extracted the hour component from each timestamp present in the 'time' column.

- Next, I generated the **price_grouped** DataFrame by grouping the data in 'df_modified' based on the 'hour_of_day' column and calculating the average price ('price_each') for each hour.

- To visualize the average prices effectively, I utilized the matplotlib library to create a line plot.

- To identify the two maximum points, indicating the hours with the highest average prices, I employed the **nlargest()** function on the 'price_grouped' DataFrame.

- To emphasize these maximum points on the plot, I incorporated annotations using the **plt.annotate()** function.

### 3.5 What is the correlation between quantity ordered and sales revenue? <a name="sec3p5"></a>

- The correlation coefficient provides insights into the strength and direction of the linear relationship between these two variables.
- I calculated the correlation between the quantity_ordered and sales revenue using the **corr()** function.
- Using the seaborn library, the 'sns.scatterplot()' function generated the scatter plot.It shows the correlation between two continuous variables because it visually displays the individual data points, helps identify patterns or trends, and assesses linearity or outliers in the relationship.


### 3.6 How does the Price of Each Item and the Quantity Ordered Influence Sales? <a name="sec3p6"></a>
- I conducted a multiple linear regression analysis to examine the relationship between the predictors, namely 'price_each' and 'quantity_ordered', and the target variable 'sales'. I defined the 'X' variable as a DataFrame consisting of these predictors and set 'y' as the 'sales' variable.

- To ensure an intercept is included in the regression model, I added a constant term using the **sm.add_constant(X)** function.

- By utilizing the **sm.OLS()** function, I created an ordinary least squares (OLS) regression model. This model took the dependent variable 'y' (sales) and the independent variable(s) 'X' (price_each and quantity_ordered).

- To obtain the fitted model's results, I called the **model.fit()** function and stored the results in the 'results' variable.

- To provide a comprehensive summary of the regression analysis, I utilized the **print(results.summary())** statement. This summary encompassed crucial statistical measures, such as coefficients, standard errors, p-values, and the R-squared value, among others.

### 3.7 Price Elasticity <a name="sec3p7"></a>
- I performed an analysis to calculate the price elasticity of the quantity_ordered variable based on the 'price_each' and 'quantity_ordered' columns in the 'df_modified' DataFrame.

- Using the regression model, I obtained the **intercept** and **slope coefficients**.

- I then calculated the price elasticity by multiplying the slope coefficient by the ratio of 'price_each' to 'quantity_ordered' for each observation.

- The results were stored in a DataFrame called **elasticity_df**.


## 4. Conclusion <a name="conclusion"></a>
The main findings of this analysis are:
1. Top-selling Products
2. Sales by Month
3. Price Variations by Time of Day
4. Quantity-Sales Relationship
5. Price Elasticity Analysis
   
## 5. References <a name="references"></a>
- [1] Python Software Foundation
https://www.python.org/

- [2] Project Jupyter
https://jupyter.org/

- [3] seaborn: statistical data visualization
https://seaborn.pydata.org/index.html#

- [4] matplotlib: Python plotting library
https://matplotlib.org/

- [5] statsmodels: Statistics in Python
https://www.statsmodels.org/stable/index.html

- [6] Ordinary Least Squares in statsmodels
https://www.statsmodels.org/dev/examples/notebooks/generated/ols.html

- [7] Keith Galli's YT Channel
https://www.youtube.com/channel/UCq6XkhO5SZ66N04IcPbqNcw


<details><summary>👇click me</summary>
![mika-noah](https://github.com/winnie-wildin/Comprehensive_Sales_Analysis/assets/110402296/e6a6c859-ad46-4d4c-b442-01c7a08bade6)

</details>
