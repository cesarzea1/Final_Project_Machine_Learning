# Final_Project_Machine_Learning
Final_Project_Machine_Learning

The Data for this analysis was found in Kaggle: https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents 

# Step 1: Cleaning the data for EDA
- The Jupyter Notebook file that was used to clean the data is 'Final_Project_Machine_Learning1.ipynb'
- The original file is 7728394 rows × 46 columns.  This is too large for Tableau Public.
- A random sample of the original dataset was selected, ensuring that the new file maintains a proper representation by city.
- To ensure proper representation is maintained, 2 data frames were created with the city counts and city proportions.  The comparison between them shows that the proportions difference is very minimal (The maximum City Proportion difference is for Miami: 0.000013).  Considering this, we can use the new sampled dataset.
- The files containing the data to compare the original sample and the sampled data set are:
    - 'city_counts.csv':  city count and proportions of the original data set
    - 'sampled_city_counts.csv': city count and proportions of the sampled data set
    - 'US_Accidents_March23.csv': is the original data set
    - 'sampled_US_Accidents.csv': is the sampled data set (50% of the original)    

# Step 2: Tableau Exploratory Data Analysis (EDA)
- 'sampled_US_Accidents': is the sampled data set (50% of the original) and the one used for the EDA.
- When plotting the accidents by month, it becomes apparent that the data for the years 2016 and 2023 are unreliable, as they seem incomplete. For 2016, the data starts at zero, indicating the likely commencement of data collection. In contrast, the data for 2023 only extends up to March. For the remainder of the analysis, I will exclude these years and focus on the fully reliable data spanning from 2017 to 2022.
- When focusing on 2017 to 2022, we observe a significant increase in the number of accidents in 2019. This is followed by a decline for a couple of months, possibly due to the pandemic, and then a spike again at the end of 2020, with high numbers persisting afterwards.
- Using Tableau dynamic filtering and dashboard, we can see that when combining all years from 2017 to 2022, November and December consistently had the most accidents. This trend, however, varies pre- and post-pandemic. From 2017 to 2019, October was the most active month, while from 2020 to 2022, December became the most active, followed by November. Assuming consistent data collection across these years, these shifts may reflect behavioral changes post-pandemic, such as increased remote or hybrid work.
- There are also noticeable changes in the peak hours for accidents. Analyzing data from 2017 to 2022, the peak hours are between 7 to 8 AM and 4 to 5 PM, with both periods showing similar activity levels. In the pre-pandemic years (2017 to 2019), the 7 to 8 AM slot was significantly more active than the evening peak. Conversely, during the pandemic and post-pandemic years (2020 to 2022), the evening peak became more prominent.
- Based on these observations, we can conclude that behavioral changes influenced the periods and timing of peak accident activity in the US.

- Severity of accidents has declined over the years. 'Severity' here is rated on a scale from 1 to 4, with 1 representing minimal impact on traffic (such as short delays) and 4 representing significant impact. The average severity in 2017 was 2.4, which decreased to 2.1 in 2022.
- The most significant decline in severity was observed in 2020, and this downward trend continues. This could be due to a combination of factors, ranging from changes in driving habits, resulting in fewer accidents as fewer people commute daily, to improved efficiency and effectiveness in handling accidents across the US.  
- Although Saturday and Sunday have the lowest number of accidents, the severity on these days is higher. Weekends consistently show higher accident severity. However, this severity has also been declining over the years.
- Finally, the time of day with the highest severity is between 10 PM and 3 AM. However, even this trend is showing improvement.

- There appears to be a correlation between geolocation and the frequency of accidents, with the East Coast and Southwest Coast regions being particularly active.
- The severity of accidents also appears to have a geographical correlation, with the Southwest Coast and the Southeast Coast exhibiting higher severity averages.

# Step 3: Cleaning the data for Machine Learning Models:
- Additional data cleaning is needed.  The process will continue in file 'Final_Project_Machine_Learning1.ipynb':
    - Eliminate columns with missing data that should not/cannot be used.
    - Eliminate columns End_Lat, End_Lng, Street, City, Airport_Code, Weather_Timestamp, Wind_Chill(F), Wind_Direction, Sunrise_Sunset, Civil_Twilight, Nautical_Twilight, Description, Timezone, and Zipcode.
- Work on the Features that need cleaning:
    - missing data in columns Temperature(F), Humidity(%), Pressure(in), Visibility(mi), Wind_Speed(mph), Precipitation(in): fill with the avg. of the other scores in the same column.
    - Weather_Condition: Fill with "NA"
- saved a new clean dataset to continue processing and avoid system crashes.  The new file is called 'clean_sampled_US_Accidents.csv'
- Split the data in Start_Time to be able to evaluate the elements on their own.  Split Year, Month, Day of the week, and Time.
- Drop columns that will not be used for the analysis:  Source, End_Time, Country.
- Based on the EDA, we'll drop all the rows for years 2016 and 2023.
- Clean the 'Weather_Condition' column grouping the different options
- saved a new clean dataset to create the machine learning models.  The new file is called 'clean_sampled_US_Accidents_for_model.csv'

# Step 4: Final preparation for the models:
- Final cleaning and the models were run in file 'Final_Project_Machine_Learning2.ipynb':
    - drop the 'ID' and 'County' columns.
    - Encode the Object variables: 'State', 'Weather_Condition', 'Astronomical_Twilight'.
    - Found correlations between Features to see if there were redundant ones.  Droped 'Astronomical_Twilight_Day' based on results.
- Split the data in features and target.
- Split the data into training and testing sets.
- Scaled the data.

# Step 5: Model Selection and Execution:
The file with the models is: 'Final_Project_Machine_Learning2.ipynb'
Run the 3 different models first with 50% of the sample, and then extracted a random 10% of that (5% of the total)and the results were very similar.  In the end, the final code uses 5% of the total. 

## Model 1: Logistic Regression model:
The results between the model run with 50% of the total sample and the one run with 5% of the total sample are quite similar.

The model shows strong precision and recall for Class 2, and decent scores for Class 3.  However, the results are poor for Classes 1 and 4.  This is likely due to a sample imbalanced, based on the support scores.  Overall, while the model might seem accurate (80%), its ability to distinguish between different severity levels is poor, especially for classes 1 and 4.

## Model 2: Decision Tree model:
The results between the model run with 50% of the total sample and the one run with 5% of the total sample show a larger difference vs. the logistic regression model.  That said, the results are still quite similar.

The Decision Tree model shows an improvement in classes 1 and 3 when compared with the logistic regression model but still shows poor results for class 4.  Class 2 results are the strongest ones, due to its large representation in the dataset.  The model is better balanced than the logistic regression model, but class 4 is still not well-predicted.

## Model 3: Deep Neural Model:
The results between the model run with 50% of the total sample and the one run with 5% of the total sample show a similar Accuracy score of 82% and 83%.  However, the model run with 5% of the sample shows a better prediction for the different classes.

The model is most accurate with severity level 2 predictions, showing a high recall of 0.94 but lower precision for other severity levels.
Severity levels 1 and 4 have particularly low recall, indicating many instances were missed by the model.
The model struggles with distinguishing between severity level 3 and 4, evident by the low precision and recall for these classes.
The accuracy seems high at 0.83, but this might be misleading due to class imbalance, as severity level 1 has a much higher number of instances compared to others.

# Step 6: Model optimization:
The optimization was run in file: 'Final_Project_Machine_Learning2- For Optimization.ipynb'

Considering the results of the 3 models, some methods were tried to optimize the models:
- Revising the Features:
    - eliminated the States.
    - eliminated some columns that could have low impact on severity: 'Amenity', 'No_Exit', 'Railway', 'Station'
    - applied SMOTE method to deal with the imbalance of the sample.
None of these actions improved significantly the models.

For the Deep Neural Model the fitting elements of the model were changed:
    - went from 'epochs=10, batch_size=50' to 'epochs=20, batch_size=50'

Comparing the results between pre and post optimization:
- The optimizations made to the model have resulted in better identification of the more severe cases (level 4). There's a slight decrease in performance for severity level 1 in terms of recall. However, the overall accuracy of the model has remained stable.
- The improvement in recall for severity levels 3 and 4 is particularly important if the cost of missing these cases is high, even if it means accepting more false positives (lower precision).
- In summary, the optimization has led to the model being more sensitive to higher severity levels, which can be critical.







