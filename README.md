# Creating a Relational Database System in MySQL.

# Index:
This repository holds Relational Database System project.
1. [Images](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/tree/main/Images) Images used in readme file.
2. [accessibility](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/blob/main/accessibility.csv) Generated table using python code.
3. [building](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/blob/main/building.csv) Generated table using python code.
4. [building_new](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/blob/main/building_new.csv) Generated table using python code.
5. [buildinglocation](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/blob/main/buildinglocation.csv) Generated table using python code.
6. [buildings](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/blob/main/buildings.csv) Main CSV file that is used to generated different tables.
7. [censusyear](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/blob/main/censusyear.csv) Generated table using python code.
8. [censusyearbuildingrecords](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/blob/main/censusyearbuildingrecords.csv) Generated table using python code.
9. [features](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/blob/main/features.csv) Generated table using python code.
10. [Msql database](https://github.com/Justin-Mathew/Creating-a-Relational-Database-System-in-MySQL/blob/main/mysql%20database.ipynb) PYTHON CODE to generate tables.

# Objective:
The objective is to create a relational database system in MySQL using python pandas for data preparation.

# Relational database systems:
A database is a collection of data stored in a structured manner that makes it easy to access it. In relational database system the dataset is divided into multiple tables that are connected to each other with relations. Tables consists of rows and columns.

# About the data set:
 The dataset can be downloaded from the link:
https://data.melbourne.vic.gov.au/Property/Buildings-with-name-age-size-accessibility-and-bic/pmhb-s6pn

The dataset consists of 19 columns. It represents different building attributes including location, construction year, refurbished year, number of floors, predominant space use, bicycle/shower facilities and building accessibility data.

# E-R diagram:
 
![](Images/ER%20diagram.png)

## Dividing the dataset into 6 different tables:
1.	Building: This table consists of all the data related to a particular building like property id, base property id, building name, construction year. Creating a new primary key building id for this table.
2.	Census year: Creating a new primary key census year id for this table. This table consists of primary key and census year.
3.	Census year building record: This table consists of all the data of a building in a given census year that includes refurbished year, no of floors and predominant space use. Creating a new primary key for this table cybr id.
4.	Building location:  This table consists of all the data about the location of a particular building. All the data in this table are related to each other. Each clue small area(suburb) has a single block id.
5.	Accessibility: This table consists of all the data that are related to accessibility of a building for the given census year.
6.	Features: This table consists of all the data that are related to features like bicycle space and shower facilities of a building for the given census year.

# Data preparation:
Python pandas is used to divide the dataset to smaller dataset. Each smaller dataset represents a table. New fields have been added wherever needed. These table will be imported to MySQL to create a relational database system.

# Importing data to MySQL:
1.	Install MySQL workbench from the following link:
https://www.mysql.com/products/workbench/
2.	Open MySQL workbench and enter password.
![](Images/mysql%20workbench.png)
3.	Create a new schema
![](Images/create%20schema.png)
4.	Run queries to create tables
![](Images/create%20table%20query.png)
5.	Right click on the table that you created and select table data import wizard. 
![](Images/data%20import%20wizard.png)
6.	Choose the appropriate file that you want to import and click next to import data  
![](Images/import%20tables.png)
7.	Create and import all the tables into MySQL.
8.	Run query to access data. 
![](Images/data%20access%20query.png)

# Queries:

## 1.	For creating building table in MySQL:

create table building (<br />
building_id int,<br />
property_id int,<br />
base_property_id int,<br />
building_name varchar(100),<br />
construction_year numeric,<br />
PRIMARY KEY (building_id)<br />
);<br />

## 2.	For creating building location table in MySQL:

create table buildinglocation (<br />
building_id int,<br />
block_id int,<br />
street_address varchar(50),<br />
clue_small_area varchar(50),<br />
x_coordinate float,<br />
y_coordinate float,<br />
PRIMARY KEY (building_id),<br />
FOREIGN KEY (building_id) REFERENCES building (building_id) ON DELETE CASCADE<br />
);<br />
## 3.	For creating census year table in MySQL:

create table censusyear (<br />
censusyear_id int,<br />
census_year int,<br />
PRIMARY KEY (censusyear_id)<br />
);<br />

## 4.	For creating census year building records table in MySQL:

create table censusyearbuildingrecords (<br />
cybr_id int,<br />
building_id int,<br />
censusyear_id int,<br />
refurbished_year numeric,<br />
number_of_floors int,<br />
predominant_space_use varchar(100),<br />
PRIMARY KEY (cybr_id),<br />
FOREIGN KEY (censusyear_id) REFERENCES censusyear (censusyear_id) ON DELETE CASCADE,<br />
FOREIGN KEY (building_id) REFERENCES building (building_id) ON DELETE CASCADE<br />
);<br />

## 5.	For creating accessibility table in MySQL:

create table accessibility (<br />
cybr_id int,<br />
accessibility_type varchar(50),<br />
accessibility_type_description varchar(100),<br />
accessibility_rating numeric,<br />
PRIMARY KEY (cybr_id),<br />
FOREIGN KEY (cybr_id) REFERENCES censusyearbuildingrecords (cybr_id) ON DELETE CASCADE<br />
);<br />

## 6.	For creating features table in MySQL:

create table features (<br />
cybr_id int,<br />
bicycle_spaces varchar(50),<br />
has_showers numeric,<br />
PRIMARY KEY (cybr_id),<br />
FOREIGN KEY (cybr_id) REFERENCES censusyearbuildingrecords (cybr_id) ON DELETE CASCADE<br />
);<br />

## 7.	Queries for accessing data from MySQL database:

### Query1:

select census_year as censusyear,<br />
building_name as buildingName,<br />
accessibility_rating as rating,<br />
accessibility_type as accessibilityType,<br />
accessibility_type_description as accessibilityDescription,<br />
construction_year as constructionYear<br />
from   building b,<br />
censusyearbuildingrecords cy,<br />
accessibility a,<br />
censusyear c<br />
where  b.building_id=cy.building_id<br />
and cy. cybr_id =a. cybr_id<br />
and cy.censusyear_id=c.censusyear_id<br />
and building_name= 'RMIT'<br />
and accessibility_rating = 3;<br />

### Query 2:

select  census_year as censusYear,<br />
building_name as buildingName,<br />
street_address as streetAddress,<br />
clue_small_area as suburb,<br />
construction_year as constructionYear,<br />
refurbished_year as refurbishedYear,<br />
accessibility_rating as accessibilityRating<br />
from    building b,<br />
buildinglocation bl,<br />
censusyearbuildingrecords cy,<br />
accessibility a,<br />
censusyear c<br />
where   b.building_id=cy.building_id<br />
and b.building_id=bl.building_id<br />
and cy. cybr_id =a. cybr_id<br />
and cy.censusyear_id=c.censusyear_id<br />
and accessibility_rating IS NOT NULL<br />
and census_year=2018<br />
and construction_year>=1950<br />
and refurbished_year >=2018;<br />

### Query3:

select  building_name as buildingName,<br />
street_address as streetAddress,<br />
clue_small_area as suburb,<br />
bicycle_spaces as bicycleSpace,<br />
has_showers as showers<br />
from    building b,<br />
buildinglocation bl,<br />
censusyearbuildingrecords cy,<br />
accessibility a,<br />
censusyear c,<br />
features f<br />
where   b.building_id=cy.building_id<br />
and b.building_id=bl.building_id<br />
and cy. cybr_id =a. cybr_id<br />
and cy.censusyear_id=c.censusyear_id<br />
and f.cybr_id=cy.cybr_id<br />
and accessibility_rating = 3<br />
and census_year=2018<br />
and construction_year>=2015<br />
and bicycle_spaces IS NOT NULL<br />
and has_showers >0;<br />

# Problems faced:
1.	Table header should be removed before importing the data into MySQL. This was achieved by setting header to false using python pandas.
2.	Null numeric values: Some database systems like apache derby supports null numeric values. MySQL does not support null numeric values this problem was resolved by replacing all the null numeric values with -1.

# Conclusion:

Building dataset was divided into smaller tables and new columns were added with the help of python pandas. Tables were imported into MySQL. Finally, queries were written to access data.
