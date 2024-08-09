esentation Notes Final

Wednesday, August 7, 2024
10:10 AM

My ETL Script:
	- Data Ingestion:
		○ We loaded the datasets that we wanted to use for our project in an s3 bucket because s3 offers reliable, scalable and secure storage for large amounts of data
		○ We also used pyspark in a databricks environment to load data from s3 to PySpark dataframes to leverage Spark's comprehensive data manipulation operations efficiently 
	- Data Transformations:
		○ I will go through the transformations that we performed in more depth when we give a small demo of the work we have done in databricks
		○ *We started off by loading the raw source taxi datasets from the s3 bucket into PySpark dataframes
		○ Performed data filtering to  train the model on the most recent data about taxi rides in new york city to allows for the model to make more accurate predictions about the immediate future
		○ To ensure data consistency and reliability, we dropped duplicate records
		○ Feature engineering entails deriving new columns from other columns in the dataset in an attempt to enrich it and make it suitable for training machine learning models - we were able to derive the year, month, day of the week and weekday_weekend columns from the pickup time column
		○ This is another example of feature engineering that we performed and we were able to extract additional information from the pickup and dropoff columns such as pickup hour and dropoff hour
		○ We even were able to derive information about the time block - Morning, Afternoon, Evening and Night - during which a passenger was picked up for example
		○ So here, we performed feature engineering on the weather dataset so that it can be combined with the taxi dataset … and then join the two datasets together
	- Data Loading:
		○  Prior to loading the final transformed dataset into the s3 bucket containing transformed data, we ensured that the data types for the columns were of appropriate types(ie. The pickup time column is of the data type timestamp).  We also dropped columns that are not relevant to trip duration

Model 1 was the first model that we built and trained, this model was trained on the taxi dataset after we performed data transformations in the databricks environment. To clarify, taxi data refers to data on taxi rides to different boroughs of new york city during the year 2024. 

*** Analyze Tab, Zoom out! 
 Essentially the model was trained on 80% of the taxi dataset. During the training process, it learned how to use values for different attributes of the dataset such as Weekday_Weekend and Pickup_Time_Block to predict trip_duration. After it was trained, it made predictions about trip duration for the remaining 20% of the samples that make up the taxi dataset, samples that it has never seen before during the training process. 

The predicted vs actual graph here shows predicted values of trip duration by the model against actual values for trip duration. So this visual helps us quickly assess the accuracy of the model when it came to predicting trip duration.  There is direct positive correlation between RMSE and the width of the purple region. As you can see, the purple region is quite wide, which means that the prediction errors the model made are quite high. 

If I hover over this point, you can see that the  value that was output by the model for trip duration is ____ but the actual trip duration is ______. 


Model 4:
This is another model that we created. This model was trained on the dataset containing taxi data and historical weather data in new york city. Prior to training the model on this dataset, we removed outliers or samples that had values for trip duration and distance that did not fall within 3 standard deviations. We did this because we did not want to train the model on samples that are very rare or uncommon. 

Show Columns Tab: 
This model  includes additional columns related to weather information such as Average_Dew_Point, Average_Temperature, Precipation in Inches and Average Wind Speed. The reason for creating this model is we wanted to see if parameters relating to the weather were strong indicators of trip duration. 

Analyze Overview Tab
 The visual on the left shows how heavily the parameters of the model influence the predictions that it makes. The higher the percentage, the greater the impact of the parameter.  As you can see, the Average_Temperature and Average_Dew_Point parameters of the model barely have an impact on the predictions that the model makes about trip duration. 
Analyze Scoring Tab:
This model has an RMSE of 5.055 and while there has been a significant decrease in RMSE, this model is terribly complex - it has 18 columns, some which as we saw, barely influence model outcomes. As a result of this model being complex model, it is able to predict trip duration with high precision at the cost of requiring  more computational resources and being more challenging to interpret. This influenced us to create a much simpler model. 


Model 5: 
Go to Columns Tab: 
When creating our final model, we were very deliberate about the 5 parameters that we have selected for this model and all of these parameters: Trip_Distance, Dropoff_Zone, pickup_time_block, Pickup_Zone and Weekday_Weekend - have a large influence on the performance of the model. 
Show Purple Graph:
By looking at the Predicted vs Actual Graph for this model, you can see that the points on the graph are very close to the dark blue line which is good because the closer the points lie to the diagonal line, the more accurate the predictions about trip duration. 

While this model yielded a slightly higher RMSE than the models with more parameters did, the RMSE of 5.428 that it yielded goes to show that the model still did a good job at accurately capturing patterns in the complex training dataset and making accurate predictions on trip duration when presented with new, unseen data. Out of all the models that we built and trained, this model is the model that we wanted to deploy

Now I will hand it over to Samee who will talk about the importance of the data analysis work that we done as well as the machine learning models that we have created
