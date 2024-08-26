# DataRequests ETL and Visualization Project
Data Requests were originally taken from a Sharepoint form but were recently tranferred to an Asana form. These forms are both dumped into an excel sheet, though with different formats, thus needing similar ETL structures but different processes. 
A SQL Server Integration Services package facilitates these processes. 
First, a staging table is truncated and Second, data is copied from the excel file into the empty staging table.
Next, an Upsert stored procedure is executed to load data into a production table for historical recording. The Upsert query merges the staging table [source] with the production table [target]. Records that exist in staging but not production, are inserted. Records that exist in both, are checked at each field for any differences, if no differences are found, the record is ignored, if found, the record is updated in production. 
Two triggers exist that will send the updated or inserted records into an AUDIT table. 
The staging table is truncated and the records that exist in the audit table are copied back into the staging, so that staging only includes 'new' data. The audit table is truncated.

A stored procedure is executed to Transform the data in staging and load it into its respective tables in the Data Warehouse.
A source table was made tracking which form the requests came from. Sharepoint was marked with a 1 and Asana with a 2. A compound key was implemnted to ensure referential integrity.
Dim tables are equipped with the logic to update themselves if new options are added to the original data request form and are able to use the updated information to load data into the Fact and Associative dim tables later in the same stored procedure.
Due to complicated, dirty data in often unpredictable formats, series of CTE's and TEMP tables are used to parse, lookup, and store manipulated data for later inserts.

I created an ERD for an existing Data Warehouse structure and added and deleted tables as necessary for more cohesive relationships while including all the data asked for.

Visualizations were created to quickly show key takeaways in the data such as: When people were requesting data, Who was requesting data, Which departments were requesting data and What were they requesting, Which dates were people requesting data for, Percentage of requests were Grama Requests, etc. The first visualization, the detail filtering, is quite compicated, and contains a lot of data in and of itself. So I added two different drill through page navigation sections taking you either to pages showing which filtering options were chosen depending on the filter category, or to a table view breaking down the choices by individual request along with longer descriptions and requirements.

Lastly, I was tasked with creating Reports for the data with SQL Server Reporting Services. Three reports were created. A matrix displaying all of the chosen filters for each request, a table with information regarding the employee that requested the data, as well as a table that shows important time data included in the request. The matrix was made by joining multiple unrelated Associative tables to their respective dim tables and back to the fact table in CTE's. Each table was Union'd together and a final query combined these all into one table by coalescing the RequestID's and joining with a FULL OUTER JOIN to include the NULLS which were important in seeing which filter options were frequently being chosen.

I learned a lot during this project. My SQL skills are much more advanced than when I initally started. I've become skilled in parsing text with CHARINDEX and LEFT AND RIGHT FUNCTIONS and using these parsed values to lookup additional values. I've learned the value of using CTE's to manipulate data. Instead of trying to fit everything into one query through a mess of functions, I took the extra space and wrote additional queries to give everything approiate aliases and tranformations to make the final query as simple as possible. This also led me to learn how to use temp tables, which became crucial to the integrity of my large query. These tools will save me much time in future projects, and I'll be able to resort to them before problems arise rather than try them on the nth try of fixing a problem. 

I've also learned the importance of working with a clear head. Many times I would spend hours working on problems to no avail, when I would decide to take lunch or trt again in the morning. Countless times, the answer would come to me as I was mindlessly going about my day. I always came right back into work and would immediately solve the problem after a little bit of trouble shooting.

I'm excited to work on more ETL processes with the use of SQL stored procedures.
