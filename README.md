# ProblemSet4
Problem Set 4 for Zoo6927: Creating a database


# 1. Metadata document found here: 

# 2. I would split this data up into two tables. The first table, labeled Rates, would include columns for the utility company name, the commercial rate, the individual rate, and the residential rate charges. This table would only contain unique utility company names, as this information seems to be very redundant throughout the dataset. The primary key for this table would be the utility company name. The second table, labeled location, would include columns for the zip code, state, utility company name, and ownership type. The foreign key in this table will be the utility company name, relating to the Rates table utility company name. Table below explains further: 
| Table name | Column names | Type of data | Keys | 
| --- | --- | --- | --- |
| Rates | utility_name | String | Primary Key | 
| | comm_rate | String | na |
| | ind_rate | String | na |
| | res_rate | String | na | 
| Location | zip | Integer | na |
| | state | String | na | 
| | utility_name | string | Foreign Key (Rates.utility_name) | 
| | ownership | string | na | 

