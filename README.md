# Companies that Suffered and were Resilient to the Pandemic -Fortune-1000-companies-from-2018-2021
Fortune 1000 data was collected (source given in readme), cleaned in Excel, Grouped in SQL and visualized in PowerBI

# EXCEL - 
Used 
https://www.someka.net/excel-template/fortune-1000-excel-list/ (given in File as 2020 Fortune 1000 Companies_by Industry)
as Source for the years 2018-2021 and downloaded these as excel sheets before compiling them onto a single excel sheet and cleaning them
Additional details such as type of industries were integrated into the compiled excel sheet as Type of Industry coloumn - https://fueled.com/blog/2020-fortune-1000-companies/ (given in File as Fortune 1000_2018-2021companies data)
Unique ID coloumn was created as "Company Name""Type of Industry" so as to be able to join later in SQL


# SQL Server - 

use Fortune1000
-- To find the count of the different type of industries for each year, around 233 rows are present in all, certain mismatch
-- due to certain companies in 2018,2019 being acquired and dropped and certain new ones added in 2021
select count(distinct(type_of_industry)) from dbo.Year_2021
select count(distinct(type_of_industry)) from dbo.Year_2020
select count(distinct(type_of_industry)) from dbo.Year_2019
select count(distinct(type_of_industry)) from dbo.Year_2018


-- Creating View for all years with headcount, profit, assets in one single dataset

create view vw_all as (select a.Company_Name,a.Type_of_Industry,a.Number_of_Employees Headcount_2021,b.Number_of_Employees Headcount_2020 ,
c.Number_of_Employees Headcount_2019,d.Number_of_Employees Headcount_2018,a.Profits Profits_2021,b.Profits Profits_2020 ,
c.Profits Profits_2019,d.Profits Profits_2018, a.Assets Assets_2021,b.Assets Assets_2020,c.Assets Assets_2019,d.Assets Assets_2018 
from dbo.Year_2021 a,dbo.Year_2020 b,dbo.Year_2019 c,dbo.Year_2018 d
where a.Unique_ID=b.Unique_ID and b.Unique_ID=c.Unique_ID and b.Unique_ID=d.Unique_ID)

-- count queries for both these show that there are 821 records showing 1 coloumn, this means that from 2018-2021 there are 821 records which are recurring
-- out of 1000, ~179 companies have discrepancy as some could have been acquired and merged since 2018,2019 and some could have formed new in 2021 we will query these
-- later with left and right join

select Type_of_industry,count(type_of_industry) as Industry from vw_all
group by Type_of_Industry
having count(type_of_industry) = '1'

-- grouping the different companies by type of industry and how many companies fit under these
-- Also removed industry only having a single company to avoid skewed results

select Type_of_industry,count(type_of_industry) as Industry from vw_all
group by Type_of_Industry
having count(type_of_industry) > '1'


# POWER BI - 
To view the visualization dashboard go to Companies that Suffered and Prospered and open raw file. You must have PowerBI downloaded to view this

# PDF -
A pdf version of the different visualizations is also put to easily read (however this is not in interactive version)
