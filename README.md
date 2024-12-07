# Big Query Costs


__Q:  How would you recommend structuring the project to keep costs down?__

To optimize project structure and reduce costs, I recommend creating new tables for data that may be queried frequently.

For static data in Google Cloud, I suggest establishing tables for data that will be accessed often. For instance, if Query 4 is a query we plan to use frequently, we could create a table such as 'missoula_url_html.' This would be a one-time process, costing approximately $0.33, and would significantly reduce the data size in the new table. As a result, subsequent queries on this table would be faster and more cost-effective.
As data flows through the pipeline, I suggest you filter and clean it on your local hardware before uploading it to the cloud. Again, create tables only for data anticipated to be queried often, while still storing raw, unfiltered data in a separate table.

I advise against querying unfiltered data unless you're using simple functions like 'COUNT' and returning basic data types such as integers or floats. Functions like 'SUM', 'MIN', 'MAX', and 'AVG' are also cost-effective, as arithmetic operations are generally inexpensive. Operators like 'GROUP BY', 'WHERE', and 'LIMIT' are efficient but can become more expensive if large string datatypes are involved. Any time computers parse through large bodies of text, it increases processing.

Additionaly, we can leverage [string compression](html_compress_test.ipynb) techniques to reduce storage and transmission costs, particularly since HTML documents often contain a significant number of repetitive tokens, however, this will add some time to the project. I haven't calculated and tested how long it would take to compress thousands of htmls.

In summary, the goal is to optimize queries by focusing on frequently accessed data, processing it in smaller, targeted tables to reduce query costs and improve performance. By leveraging inexpensive functions and operators and avoiding unnecessary processing of large, unfiltered datasets, you can keep costs low while maintaining efficiency, leading to successful data management and optimization in Google Cloud.

__[query_costs](query_costs.ipynb)__ - This Jupyter notebook analyzes and demonstrates the cost implications of different BigQuery operations on a car listing dataset ("carbitrage"). It runs four increasingly complex queries - from a simple count to retrieving full HTML content - and calculates both the immediate processing costs and projected annual costs if the queries were run every 6 hours. The notebook concludes with recommendations for optimizing query costs, particularly highlighting how queries involving large text fields (like HTML content) significantly increase processing costs compared to simpler queries on basic data types.

__[html_compress_test](html_compress_test.ipynb)__ - Compresses HTML content using Python's zlib library, reducing the file size by around 85% while ensuring no data is lost. It reads HTML from a text file, compresses it, and then verifies the compression worked by decompressing it and comparing it to the original, while also showing how much smaller the compressed version is as a percentage.
