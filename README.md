# CS4622 - Machine Learning Final Project Pump it Up: Data Mining the Water Table
github link: https://github.com/MithunW/MachineLearningProject
![image](https://user-images.githubusercontent.com/46267056/133775047-8fcfe825-ca00-4b72-bb43-8c7f85a97b7a.png)

Using data from Taarifa and the Tanzanian Ministry of Water, can you predict which pumps are functional, which need some repairs, and which don't work at all? This is an intermediate-level practice competition. Predict one of these three classes based on a number of variables about what kind of pump is operating, when it was installed, and how it is managed. A smart understanding of which waterpoints will fail can improve maintenance operations and ensure that clean, potable water is available to communities across Tanzania.

## Preprocessing
### Handling missing values
* Columns with missing values
  *   funder - Who funded the well
      *   Missing Values = 3635 count
      * 55765 
      * unique                      1897 
      * top       Government Of Tanzania 
      * freq                        9084
  * installer - Organization that installed the well
	  * Missing Values 3655
	  * count     55745
	  * unique     2145
	  * top         DWE
	  * freq      17402
  * public_meeting - True/False
	  * Missing Values 3334
	  * count     56066
	  * unique        2
	  * top        True
	  * freq      51011
  * recorded_by - Group entering this row of data
	  * Unique Values =1
	  * Not important because of the uniqueness of each and every row.
  * scheme_management - Who operates the waterpoint
	  * Missing values 3877
	  * count     55523
	  * unique       12
	  * top         VWC
	  * freq      36793
  * scheme_name - Who operates the waterpoint
	  * Missing Values 28166
	  * Unique Values 2696
  * permit - If the waterpoint is permitted
	 * Missing values 3056
	 * count     56344
	 * unique        2
	 * top        True
	 * freq      38852

* schema_name , sub_village, recorded_by , date_recorded columns were dropped due to the high missing value count and they have high number of unique values.

* funder, installer, public_meeting, schema_management, permit columns were filled their missing values with the most frequent value which we observed from the dataset.

### Duplicate Values in Train Dataset
*  There were 118 duplicate rows in train dataset and they were dropped from the dataset.

### Correlation Matrix
![image](https://user-images.githubusercontent.com/46267056/133789592-06b34056-48ee-43d2-9285-fd0590f79459.png)


*	Correlation Data
	*	gps_height x construction_year 0.66
	*	installer x funder 0.54
	*	region_code x district-code 0.68
	*	management x scheme_management 0.66
	*	extraction_type x extraction_type_group 0.95
	*	extraction_type_class x extraction_type 0.7
	*	extraction_type_class x extraction_type_group 0.78
	*	management_group x management 0.6
	*	payment_type x payment 0.69
	*	quantity_group x quantity 1
	*	source_type x source 0.94
	*	waterpoint_type x waterpoint_type_group 0.98 (waterpoint_type contains more details)

*	Drop one column from correlations more than 0.9
	*	extraction_type_group
	*	extraction_type
	*	quantity_group
	*	source_type
	*	waterpoint_type_group
*	Drop more.
	*	management x scheme_management 0.66 and schema_management had relatively high count of missing values, Dropping it.
	*	payment_type x payment 0.69 lets drop one of them.
	*	installer x funder 0.54 has a relatively high correlation. So we are going to remove funder and keep installer.
	*	region_code x district-code 0.68 dropping region_code

### More Feature Engineering
*	Analyzing quantity and quantity_group columns
![image](https://user-images.githubusercontent.com/46267056/133789748-419698a2-2d4d-4402-aab4-3abcc47850e8.png)
![image](https://user-images.githubusercontent.com/46267056/133789798-981a6583-fd35-4094-bcc3-3f91f77ab97c.png)
Conclusing is these two columns have same information

* Analyzing source_class and source columns
!![image](https://user-images.githubusercontent.com/46267056/133789865-118d2ea0-2d91-4199-bb54-653d0cb9f692.png)
![image](https://user-images.githubusercontent.com/46267056/133789892-52f3ee6d-0d68-4891-befb-0101da0e8a40.png)
As you can clearly see, source_class is a generalized version of source column. Lets keep source column which includes more details and drop source_class columns
*	Analyzing water_quality and quality_group
	*	water_quality column unique values
		*	soft                  50716
		*	salty                  4851
		*	unknown                1866
		*	milky                   804
		*	coloured                490
		*	salty abandoned         338
		*	fluoride                200
		*	fluoride abandoned       17
	*	quality_group column unique values
		*	good        50716
		*	salty        5189
		*	unknown      1866
		*	milky         804
		*	colored       490
		*	fluoride      217

Both columns have same values and water_quality column has more details. drop quality_group column.

##### Dropping Columns
*	extraction_type_group
*	quantity_group
*	funder
*	extraction_type
*	source_type
*	waterpoint_type_group
*	scheme_management
*	source_class
*	quality_group
*	payment_type

##### construction year
*	After training the model once, It is observed that the construction year is contributing to the model at large. So going to do some feature engineering to construction_year
*	Since the construction_year column has high cardinality, going to make it more generalize by changing the year to decades.
*	After the generalization
	*	0s 20613 (There were outlier year values equal to 0 and they were replaced with '0'
	*	00s 15328 
	*	90s 7677 
	*	80s 5571 
	*	10s 5160 
	*	70s 4398 
	*	60s 535

![image](https://user-images.githubusercontent.com/46267056/133789958-14255969-7d2f-4c8c-8f03-c953ba54c9be.png)

### Preprocessing Outlier Values in Dataframe
* Population
Population column has values of 0 which is possible to be an outlier. So replaced them with the mean value of the population column in full dataset.
*Longitude
Longitude also has zero values which should be possible outliers. We are going to replace them with the mean value.
![image](https://user-images.githubusercontent.com/46267056/133790004-f66fa477-86a5-46de-958c-958bebd73c0b.png)
*	Installer
Observed that there are lots of unique values in this column, most of them are outliers which are possible spelling mistakes or errors in data gathering. So, fixed them.
* Removed High Cardinality Columns from Dataset
	* wpt_name
	* ward
	* lga


## Encode Categorical Columns
*	Train data and test data feature set were concatanated in to one datafram and them encoded categorical columns using a label encoder. 
*	Did not use one hot encoding which will increase the feature country drastically and it will badly affect the complexity of the model.

### Feature Selection
* As discussed above, handled the missing values, outliers and created new features from the given dataset.
* Dropped the redundant columns from the dataset and rest was used as the features for the below models.

## 	Model Selection
* Gradient Boosting Classifier
* Random Forest Regressor
* XG Boost

Used these models to train the dataset and get the predictions.

## Proof of Submission
![proof_pump_it_up_2](https://user-images.githubusercontent.com/47697151/133602630-b87bee8f-f702-4d0a-893c-56d5e7471d81.PNG)


## Final Rank
![rank_pump_it_up_2](https://user-images.githubusercontent.com/47697151/133602680-e116f044-b452-45ab-9cf6-d2c3c38217ad.PNG)
