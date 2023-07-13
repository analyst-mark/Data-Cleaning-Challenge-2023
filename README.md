# Data Cleaning Project

![](Data%20cleaning%20challenge/source/background.PNG)

## Introduction

Data cleaning is the process of identifying and correcting corrupt or inaccurate data in a dataset. It involves revising, editing, and organizing raw data to make it uniform and ready for analysis. Data errors can occur during entry, storage, or transmission, but cleaning the data can improve its accuracy, dependability, and validity.

This project was undertaken for the #DataCleaningChallenge, an event organized by data experts in the data tech space. The challenge aimed to encourage participants to utilize their preferred data cleaning tools, such as spreadsheets, Power Query, Python, R, or SQL, to clean the data. Specifically, I utilized Power Query to clean my data.

## About the Data

The dataset used for this project was obtained from **[Kaggle](https://www.kaggle.com/datasets/aayushmishra1512/fifa-2021-complete-player-data)**, consisting of 18,979 rows and 77 columns. It includes information about players, such as their IDs, photo URLs, player URLs, names, ratings, ages, nationalities, positions, growth potentials in the game, and more.

## Data Exploration

To begin the data cleaning process, I conducted a data exploration of the dataset in Microsoft Excel using Power Query. This allowed me to gain a better understanding of the dataset. During the review, I observed that the dataset contained special characters, unnecessary columns, incorrect data types, and inconsistencies in the units used for recording, such as "M", "K", "Kg and lbs", "cm and ft'in". Here is a screenshot of the original dataset:

| Original Dataset | ![](Data%20cleaning%20challenge/source/original%20dirty%20data.PNG) |
| ---------------- | ------------------------------------------------------------------- |

I decided to split the dataset into two parts: "Player Info" and "Player Stats". The majority of the cleaning work will be done on the "Player Info" table, as it contains several issues such as special characters, unnecessary columns, and inconsistencies in the units used for recording. On the other hand, the "Player Stats" table is mostly clean and requires minimal cleaning. The only necessary change is to replace the data type from string to number for some columns.

| Player Info table  | ![](Data%20cleaning%20challenge/source/player%20info%20table.PNG)  |
| ------------------ | ------------------------------------------------------------------ |
| Player Stats table | ![](Data%20cleaning%20challenge/source/player%20stats%20table.PNG) |

## Whitespaces

---

| Before                           | After                                            |
| -------------------------------- | ------------------------------------------------ |
| ![](https://github.com/analyst-mark/Data-Cleaning-Challenge-2023/blob/main/Data%20cleaning%20challenge/source/white%20space.PNG) | ![](https://github.com/analyst-mark/Data-Cleaning-Challenge-2023/blob/main/Data%20cleaning%20challenge/source/white%20space%20clean.PNG) |
|                                  |                                                  |

During the initial inspection of the data, we noticed that there were large amounts of white space in the dataset. In order to clean and manipulate the data more easily, we needed to remove the white space. To achieve this, we used the functions CLEAN and TRIM.

## Name and LongName Column

| Issues | ![](Data%20cleaning%20challenge/source/name%20dirty%20data.PNG) |
| ------ | --------------------------------------------------------------- |

The "Name" and "Long Name" columns in the dataset contained several issues. Firstly, some entries contained abbreviated first names instead of the full name. Secondly, there were diacritic names, such as Spanish names with accents, which could cause issues when analyzing the data. Additionally, some entries only contained either the first name or surname, making it difficult to identify the player accurately.

### Cleaner version

| Before                                                   | After                                                               |
| -------------------------------------------------------- | ------------------------------------------------------------------- |
| ![](Data%20cleaning%20challenge/source/player%20url.PNG) | ![](Data%20cleaning%20challenge/source/Cleaned%20player%20name.PNG) |
|                                                          |                                                                     |

I decided to delete the Name and LongName Column, which contained inconsistent and sometimes incomplete data. The "Player URL" column provided a reliable source of information for extracting the full name of each player.Furthermore, by removing any diacritic marks from the names and applying the Proper function to capitalize the first letter of each word, we were able to ensure that the names were consistent and presented in a standardized format across the dataset.

## Age

| dirty data          | ![](Data%20cleaning%20challenge/source/age.PNG)                     |
| ------------------- | ------------------------------------------------------------------- |
| Year parameter      | ![](Data%20cleaning%20challenge/source/current%20age.PNG)           |
| power query formula | ![](Data%20cleaning%20challenge/source/dynamic%20age%20formula.PNG) |
| Cleaned data        | ![](Data%20cleaning%20challenge/source/dynamic%20age.PNG)           |

Since the dataset is two years old, we wanted to ensure that the age of each player reflected the most current age possible. To achieve this, we created a parameter in Power Query that calculated the difference between the current year and 2021. We then added this value to the age column, which resulted in a more recent and dynamic age for each player.

This approach ensures that the age of each player is up-to-date and accurate, regardless of when we access the dataset in the future. For example, if we were to open the dataset five years from now, the age of each player would still be calculated based on the current year, rather than the year in which the dataset was created.

## OVA, POT, and BOV

| Before                                           | After                                                    |
| ------------------------------------------------ | -------------------------------------------------------- |
| ![](Data%20cleaning%20challenge/source/rate.PNG) | ![](Data%20cleaning%20challenge/source/clean%20rate.PNG) |
|                                                  |                                                          |

The "OVA", "POT", and "BOV" columns in the dataset were recorded as strings, but we needed to convert them to percentages. To do this, we used Power Query to first change their data type to number. We then divided each value by 100 to get them in decimal form, and finally converted them to percentage format.

## Height, Weight and Hits

| Column issues | ![](Data%20cleaning%20challenge/source/height%20weight%20%2C%20hits.PNG) |
| ------------- | ------------------------------------------------------------------------ |

The "Height", "Weight", and "Hits" columns had several issues that needed to be addressed during the cleaning process. Firstly, all three columns were recorded as strings, which made it difficult to perform numerical calculations or comparisons. Secondly, the "Height" column was recorded in two different units - centimeters and feet/inches - which made it difficult to compare heights across players. The "Weight" column was similarly recorded in two different units - kilograms and pounds - which made it difficult to compare weights across players. Finally, the "Hits" column was recorded in thousands (K) units, which needed to be expanded to get the actual number of hits.

### Solution

---

| hits   | ![](Data%20cleaning%20challenge/source/hits%20fixed.PNG) |
| ------ | -------------------------------------------------------- |
| weight | ![](Data%20cleaning%20challenge/source/wight%20fix.PNG)  |
| Height | ![](Data%20cleaning%20challenge/source/height%20fix.PNG) |

For the "Hits" column, we checked if the value ended with the letter "k" indicating a thousand-unit value. If it did, we replaced the "k" with an empty space, converted the remaining value to a number, and then multiplied by 1000. If it did not end with "k", we simply converted the value to a number.

To convert the "Weight" column, we can use a similar approach. We can first check whether the value ends with "lbs" or "kg". If it ends with "lbs", we replace "lbs" with an empty string and convert the remaining string to a number. If it ends with "kg", we replace "kg" with an empty string and convert the remaining string to a number. We can then multiply the weight in kg by 2.20462 to get the weight in pounds.

To fix the issues with the "Height" column, we can split the column into two separate columns for feet and inches. If the original data is in centimeters, we can mark the inches column as null. Then, using a custom column in Power Query, we can check whether the inches column is null. If it is null, we simply get the value in the feet column. If it is not null, we can convert the feet and inches values to centimeters by multiplying feet by 30.48 and adding the inches value multiplied by 2.54.Lastly, rename the column as height

## IR,SM and W/F 

| Before                                           | After                                                  |
| ------------------------------------------------ | ------------------------------------------------------ |
| ![](Data%20cleaning%20challenge/source/star.PNG) | ![](Data%20cleaning%20challenge/source/star%20fic.PNG) |
|                                                  |                                                        |

To clean the "IR", "SM", and "W/F" columns, we need to remove the star symbol from the values and convert the resulting strings into whole numbers using Power Query. We can use the "Replace Values" function to replace the star symbol with an empty string and then use the "Change Type" function to convert the column to a whole number data type. This will leave us with the numerical value of each attribute without the star symbol.

## Position

| Before                                                | After                                                         |
| ----------------------------------------------------- | ------------------------------------------------------------- |
| ![](Data%20cleaning%20challenge/source/positions.PNG) | ![](Data%20cleaning%20challenge/source/positions%20fixed.PNG) |

To clean the "Positions" column, we first split the column by delimiter (",") in Power Query. This will create separate columns for each position listed. We then renamed these columns to "Position 1", "Position 2", and "Position 3" respectively.

This approach allowed us to separate out the multiple positions listed in the original column, and have them organized into their own respective columns. We can now analyze and manipulate the data based on each player's primary, secondary, and tertiary positions.

## Value, Wage, Release Clause

| dirty data | ![](Data%20cleaning%20challenge/source/value%2Cwage%20release%20cost.PNG) | power query formula | ![](Data%20cleaning%20challenge/source/value%20formula.PNG)                         |
| ---------- | ------------------------------------------------------------------------- | ------------------- | ----------------------------------------------------------------------------------- |
|            |                                                                           |
| parameter  | ![](Data%20cleaning%20challenge/source/parameter%20formula.PNG)           | Cleaned data        | ![](Data%20cleaning%20challenge/source/final%20wage%2Cvalue%2Crelease%20clause.PNG) |

The "Value", "Wage", and "Release Clause" columns in the dataset are all recorded in euros and as strings. Additionally, the units of these values are either "M" (for millions) or "K" (for thousands), which makes it difficult to compare and analyze them. Therefore, we need to clean and convert these columns to a proper numerical format.

To do this, we first need to remove the euro symbol and any commas from the string values. We can then check whether the value ends with "M" or "K" to determine the unit, and convert the string to a number accordingly. Once we have converted the values to numeric form, we can standardize the units by multiplying values in thousands (K) by 1000, and values in millions (M) by 1,000,000.

We then created a parameter for the euro to dollar conversion rate, which allowed us to easily change the conversion rate and reflect a more accurate value in dollars.After creating the parameter, we multiplied it by the euro values to get the corresponding dollar values.

## Contract

---

| contract data issues | ![](Data%20cleaning%20challenge/source/contract%20issues.PNG) |
| -------------------- | ------------------------------------------------------------- |

The "Contract" column in the dataset contains information about the start and end dates of each player's contract with their respective club. However, there are several issues with this column that need to be addressed.

Firstly, the start and end dates are both recorded as a single string, which makes it difficult to perform any analysis or filtering based on these dates. Additionally, some rows do not have a contract start or end date and instead indicate a loan date, while others simply state that the player is free. These inconsistencies need to be fixed in order to make the "Contract" column more useful for analysis.

### Split table and filter each

| contract table | ![](Data%20cleaning%20challenge/source/free%20table.PNG)    |
| -------------- | ----------------------------------------------------------- |
| condition      | ![](Data%20cleaning%20challenge/source/contract%20type.PNG) |

To make data cleaning easier, you decided to duplicate the table three times and create an additional column that describes the player's contract type as either "contract", "on loan", or "free". This would allow for easier filtering and sorting of the data based on the player's contract status.

| final contract table | ![](Data%20cleaning%20challenge/source/final%20contract%20table.PNG) |
| -------------------- | -------------------------------------------------------------------- |

By cleaning up the contract column, including splitting it into contract start and end and creating additional columns for loan start and end, make it's easier to analyze and manipulate the contract data. Additionally, marking null for empty records ensures data consistency and accurate calculations. Finally, appending the three tables into a single final contract table streamlines the data and facilitates further analysis.

## Player Stats Table

| Player Stats | ![](Data%20cleaning%20challenge/source/player%20tats.PNG) |
| ------------ | --------------------------------------------------------- |

The player stats table is already quite clean, so all we need to do is convert the data from string to whole numbers for easier analysis.

# Final Tables

| Player Info   | ![](Data%20cleaning%20challenge/source/player%20info%20fin.PNG) |
| ------------- | --------------------------------------------------------------- |
| Player Stats  | ![](Data%20cleaning%20challenge/source/player%20tats.PNG)       |
| Contract Info | ![](Data%20cleaning%20challenge/source/contract%20info.PNG)     |
| Relationship  | ![](Data%20cleaning%20challenge/source/rel.PNG)                 |

To summarize my project, I have split a single dataset into three separate tables: player info, player stats, and contract info. By doing so, I have established a one-to-one relationship between the tables, as each record has a unique attribute ID and no duplicate values. This approach has made the data more organized and easier to work with for further analysis.

# Conclusion

The project involved applying and improving data-cleaning skills, which is a crucial step in any data analysis project. Various data quality issues such as inconsistencies, wrong data types, and other anomalies were identified and addressed to ensure the accuracy of the data.

Thoroughly cleaning the FIFA'21 dataset resulted in a complete, accurate, and consistent final data set, suitable for data visualization and further analysis.

Different techniques were applied throughout the project, such as splitting columns, removing unnecessary characters, converting data types, marking null values, and creating new columns to address data quality issues. These techniques are essential for data cleaning and can be used in real-world data analysis projects.

Overall, this project provided a valuable learning experience in data cleaning and improved data-wrangling skills. It also demonstrated practical knowledge applicable in various data analysis projects.

Thank you for taking the time to read through my FIFA'21 data cleaning project


