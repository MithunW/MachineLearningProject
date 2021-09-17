# CS4622 - Machine Learning Final Project Pump it Up: Data Mining the Water Table
github link: https://github.com/MithunW/MachineLearningProject
![image](https://user-images.githubusercontent.com/46267056/133775047-8fcfe825-ca00-4b72-bb43-8c7f85a97b7a.png)

Using data from Taarifa and the Tanzanian Ministry of Water, can you predict which pumps are functional, which need some repairs, and which don't work at all? This is an intermediate-level practice competition. Predict one of these three classes based on a number of variables about what kind of pump is operating, when it was installed, and how it is managed. A smart understanding of which waterpoints will fail can improve maintenance operations and ensure that clean, potable water is available to communities across Tanzania.

## Preprocessing
### Handling missing values
* Columns with missing values
  *   funder - Who funded the well
Missing Values = 3635
count                      55765
unique                      1897
top       Government Of Tanzania
freq                        9084


* funder, installer, public meeting, scheme_management, scheme_name, permit are the columns with null values.

* for the column funder, null values are replaced with mean values.

* for the column installer, null values are replaced with unknown.

* for the column public_meeting, True and false are the values for that column. As there abouth more True values compared to False values, null values are replaced with True

* As these scheme_management reperesents who operates the water point and the 'management' represents how the water point is managed. As the scheme_management column has 4846 null values, drop the scheme_management column from the dataset.

* scheme_name column is dropped due to it has large unique values and null values

* Missing values of permit column is replaced with True as it has many True compared to False.

* For the population, logitude columns, the zero values are replaced with the mean value of corresponding column.

* For the construction_year, replaces the zero values with the round off mean value.

### Categorical Encoding
* There are about 30 object type features. After doing feature enginnering processes and dropping columns,
for categorical encoding for these features used factorize method.

* Did not used one hot encoding, as it increases number of columns the increase of the unique values in the column which cause larger computational power to train the model.

### Standard Normalization
* Finally standardize the features by bremoving the mean and scaling to unit variance.

### Other
* For the column installer, the names of the values are different due to spelling mistakes, use of upper case letters in different places in the word etc..

* for a example 'District Water Department', 'District water depar','Distric Water Department'

* 'COUN', 'District COUNCIL', 'DISTRICT COUNCIL','District Counci', 'District Council','Council','Counc','District  Council','Distri'

* So replace those words with one unique value.


## Feature Engineering

### Create new features
* For the column construction_year, as ther are about many unique values, create a new feature called decade to add the information about in which decade the construction has started.

### feature Selection
* For the columns,  quatity and quatity_group has contains same information, decided to drop the quatity_group because redundancy would reduce thebaccuracy of the model.

* source, source_type, sorce_class three columns keep the same information, decided to keep only the sorce column as it has more details.

* water_quality and quality_group columns has same information and water_quality has more unique values, decided to keep water_quality column only.

* extraction_type_group or extraction_type columns has the same information. Although extraction_type has more unique values than extraction_type_group,  decided to keep extraction_type_group as the other one has some values with extremely low amount

* waterpoint_type and waterpoint_type_group columns contain same information, decided to keep the waterpoint_type which has more details

* column recorded_by has only one unique value for all the rows. So decided to remove that column.

* wpt_name, scheme_name and region_code 3 columns have dropped due to having large unique values and null values.

* for the subvillage column it has many non unique values. As this column has the location value of water point region but from the column region it already keep the location value of water point. handling difficulty of non unique object values, decided to drop the subvillage column.


## Proof of Submission
![proof_pump_it_up_2](https://user-images.githubusercontent.com/47697151/133602630-b87bee8f-f702-4d0a-893c-56d5e7471d81.PNG)


## Final Rank
![rank_pump_it_up_2](https://user-images.githubusercontent.com/47697151/133602680-e116f044-b452-45ab-9cf6-d2c3c38217ad.PNG)
