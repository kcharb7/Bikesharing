# Citi Bike Bikesharing Program in New York City
## Overview
### *Purpose*
Kate would like to start a bike-sharing business in her hometown of Des Moines, Iowa, similar to the one she used when visiting New York City. Kate has a potential angel investor that is willing to provide seed money. Kate would like to learn more about how this business was run in New York City and then create a proposal on how it might work in Des Moines. For the proposal, one of the key stakeholders would like to see a bike trip analysis with the following visualizations:
-	Show the length of time that bikes are checked out for all riders and genders
-	Show the number of bike trips for all riders and genders for each hour of each day of the week
-	Show the number of bike trips for each type of user and gender for each day of the week

# Results
## Download Data
For this project, I used data from the Citi Bike program in New York City for August 2019. 

## Determine the Number of Trips
To start, I wanted to determine how many bike trips were recorded for August 2019. I created a new worksheet in Tableau Public called “Number of Rides”. Then, I dragged the measure “201908-citibike-tripdata.csv(Count)” into the Text box of the Marks section to determine that the total number of trips in August 2019 was 2,344,224. 

![Number_of_Rides.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Number_of_Rides.png)

## Find the Proportion of Short-Term Customers to Annual Subscribers
To determine the proportion of short-term customers of the bike service to the annual subscribers, I created a pie chart. I created a new worksheet called “Customers” and then added the “Usertype” dimension to the Color mark and selected the “Pie” chart option. Then, I added the “201908-citibike-tripdata.csv(Count)” measure into the Size mark and the Angle mark. The pie chart looked as follows:

![Customers_Pie.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Customers_Pie.png)

“Subscribers” were users who were annual subscribers and consisted of 1,900,359 individuals. “Customers” were those who were short-term riders and consisted of 443,865 individuals. 

## Find the Number of Rides by Gender
To further understand the customer-base, I aimed to determine the gender breakdown of Citi Bike riders using a pie chart. I dragged the “Gender” measure to the Color mark and selected the Pie chart option. Once dragged, I changed the “Gender” measure to a dimension. Then, I dragged the “201908-citibike-tripdata.csv(Count)” measure to the Angle mark. To display each gender, I created a calculated field by first navigating to my “Data Source” tab and changing the “Gender” variable to a string. Then I clicked the arrow in the “Gender” dimension, selected “Create Calculated Field”, and renamed it to “Number to String” in the pop-up window. Within this window, I used the following code to convert the numbers in the “Gender” dimension to unknown, male, or female:
```
IF [Gender] = '0' THEN 'Unknown'
ELSEIF [Gender] = '1' THEN 'Male'
ELSEIF [Gender] = '2' THEN 'Female' 
END
```
Then, I returned to my worksheet and dragged the “Number to String” dimension to the Color mark. 

![Gender_Breakdown.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Gender_Breakdown.png)

The pie chart indicated that a majority of customers were males at 1,530,272, followed by females at 588,431, and unknown at 225,521.

## Change Trip Duration to a Datetime Format
Before I could create my visualizations for the number of trips for each hour, I needed to convert the “tripduration” column from an integer to a datetime datatype to get the time in hours, minutes and seconds. To do this, I used Python and Pandas functions to create a DataFrame from the “201908-citibike-tripdata.csv” file:
```
import pandas as pd

# Create a DataFrame for the 201908-citibike-tripdata data. 
citibike_df = pd.read_csv('201908-citibike-tripdata.csv')
citibike_df.head(10)
```
Then, I checked the datatype of each column in the DataFrame:
```
# Check the datatypes of your columns. 
citibike_df.dtypes
```
I passed the “tripduration” column and units inside the “to_datetime()” function to convert it to a datetime datatype and created a new column called “tripduration_datetime” to contain the conversion:
```
# Convert the 'tripduration' column to datetime datatype.
citibike_df['tripduration_datetime'] = pd.to_datetime(citibike_df['tripduration'], unit='s')
citibike_df.head()
```
I checked the datatype of each column to ensure the conversion worked:
```
# Check the datatypes of your columns. 
citibike_df.dtypes
```
Finally, I exported the new DataFrame as a CSV file without the index column:
```
# Export the Dataframe as a new CSV file without the index.
citibike_df.to_csv('201908-citibike-tripduration-datetime.csv', index=False)
```
## Checkout Time for Users
I created a visualization of how long bikes are checked out by riders by adding the “201908-citibike-tripdata.csv(Count)” measure to the Rows section. I added the converted “tripduration_datetime” column to the Columns section and filtered the “More” option by “Hour”. I added the “tripduration_datetime” column again to the Columns section, filtered the “More” option by “Minute”, and changed the values from “discrete” to “continuous”. The “tripduration_datetime” column that showed the “Hour” was then added to the Filters field and I selected “Show Filter”. I edited the label of the x-axis to “Minutes” and the label of the y-axis to “Number of Bikes”. The final graph looked as follows:

![Checkout_Times_All.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Checkout_Times_All.png)

This graph indicates that most bikes were checked out for approximately 5 minutes and very few bikes were checked out for more than an hour. 

## Checkout Times by Gender
To create the graph of the length of time that bikes are checked out by each gender, I repeated the same steps as above. I additionally added the converted column for “Gender” to the Color mark and the Filter field. Then, I selected “Show Filter”. The final graph looked as follows:

![Checkout_Time_Gender.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Checkout_Time_Gender.png)

This graph indicates that more male customers than female or unknown customers checked out their bikes for 5 minutes. There is minimal difference between genders for customers who took the bikes for an hour. 

### *Trips by Weekday for Each Hour*
For the next visualization, I graphed the number of bike trips by weekday for each hour of the day as a heatmap. I added the “Starttime” column to the Rows section and filtered the “More” option by “Hour”. Then, I added the “Stoptime” column to the Columns section and filtered the “More” option by “Weekday”. I added the “201908-citibike-tripdata.csv(Count)” measure to the Color Mark and selected “Automatic” for the graph type. I formatted the y-axis to show the hours in a 12-hour format and formatted the x-axis to show the weekdays as abbreviations. Finally, I changed the colour palette to “red-gold”. The heatmap looked as follows:

![Heatmap.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Heatmap.png)

Most bikes are used between 5pm and 6pm on Mondays, Tuesdays, and Thursdays, with Thursdays having the highest count. Bikes were also frequently used Mondays to Friday at 8am, with Thursdays having a higher bike count than the other weekdays. Bikes are used less on Mondays, Tuesdays, and Wednesdays between 2am and 4am. Tuesdays at 3am is when bikes are used the least, followed by Mondays at 3am. 

### *Trips by Gender*
I created a similar heatmap using the same process as above to graph the number of bike trips by gender for each hour of each day of the week. To the Columns section, I added the converted column for "Gender". I additionally added the “Gender” column to the Filters field and selected "Show Filter". The final heatmap looked as follows:

![Heatmap_Gender.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Heatmap_Gender.png)

Male and female customers had similar usage trends, with the busiest time being Thursdays between 5pm and 6pm. Though, more males used the bikes than females. Mondays and Tuesdays between 5pm and 6pm were the next highest usage times for males, while Thursdays at 8am was the next highest usage time for females. Unknown genders most often used the bikes on Saturdays between 11am and 6pm. All genders used bikes the least between 2am and 4am Monday to Friday.

### *User Trips by Gender by Weekday*
I created a third heatmap to graph the number of bike trips by both user type and gender. I added the converted column for "Gender" to the Columns and Filters sections and selected “Show Filter”.  I did the same with the “Usertype” column. I added the “Starttime” column to the Rows section and filtered the “More” option by “Weekday”. Finally, I added the “201908-citibike-tripdata.csv(Count)” measure to the Color Mark and selected “Automatic” for the graph type. The heatmap looked as follows:

![Heatmap_Users_Gender.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Heatmap_Users_Gender.png)

More bike trips were taken by male and female annual subscribers, while bikes trips by short-term customers were more often done by unknown genders. All annual subscribers, regardless of gender, most often took bike trips on Thursdays followed by Fridays and were least likely to take bike trips on Sundays. All short-term customers, regardless of gender, most often took bike trips on Saturdays followed by Sundays and were least likely to take bike trips on Wednesdays.

## Create a Story
With the above visualizations, I created a story:

[link to dashboard]( https://public.tableau.com/profile/kimberly.charbonneau#!/vizhome/CitiBikeBikesharingPrograminNewYorkCity_16218741787900/CitiBike)

# Summary
As shown in the visualizations, 81.1% of customers for the Citi Bike bike-sharing program in New York City were annual subscribers and 65.3% of customers were male. Bikes were most often used by annual subscribers during the week at 8am and between 5pm and 6pm, suggesting that those who subscribe to the program use the bikes to commute to and from work. On weekends, bikes were more often utilized by short-term customers. When bikes were checked out, most were not used for more than 5 minutes, with less than 1,000 bikes being used for over an hour.

# Additional Visualizations
The “Birth Year” dimension, “Usertype” dimension, and the “201908-citibike-tripdata.csv(Count)” measure could be used to create a bar graph showcasing the number of rides by age and user type to see what approximate age most annual subscribers versus short-term riders are and how often each age uses the bikes. 

![Rides_by_User_Age.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Rides_by_User_Age.png)

The “Start Station Longitude” measure, the “Start Station Latitude” measure, the “Usertype” dimension, and the “201908-citibike-tripdata.csv(Count)” measure could be used to create a symbol map that visualizes the most popular areas for biking based on each customer type. 

![Top_Starting_Locations.png](https://github.com/kcharb7/Bikesharing/blob/main/Images/Top_Starting_Locations.png)

With this information, Kate could identify what age demographic she should advertise her bikeshare program to and in which locations.
