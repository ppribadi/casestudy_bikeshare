# Cyclistic Case Study
This is a case study for completing the requirement of Google Data Analytics professional certification. The certification courses work through the roadmap of six stages of the data analytics, namely, ASK, PREPARE, PROCESS, ANALYZE, SHARE and ACT. This is the link to the [courses](https://www.coursera.org/professional-certificates/google-data-analytics). 

## Data and Scope
The dataset for this case study is provided by Divvy Bike Sharing Company from Chicago. Cyclistic is a fictional company with the data analytic task based on real-life data from Divvy. The dataset is open source and provided with the project. The tabluar data contain trip data with start and end bike stations, start and end trip time stamps, bike types, and user types. Detailed scope of this project is described in 0_Scope.ipynb and 1_Ask.ipynb.
The following sections provide summary from each stage in the roadmap. The detailed works are processed and documented in the jupyter notebooks. I set out to capture the work process and not to seek the ultimate answers which means I limited the data scope to the data provided by the project. In case I have more time later, I would like to acquire more public data such as demographics and locational features.

### Stage 1 - ASK
The business task is defined as: In order to support the business stakeholder for increasing rider conversion from casual riders to annual members, the data analyst needs to help discover how they use Cyclistic bikes differently.

#### Business Tasks:
I will break down the task into answering these questions:
For casual and member riders:

    - How different are usages by locations?  
    - How many rides per month? How are different are they trending in time? 
    - How long are the trips? How different are the stats? 
    - How different are the rides by seasons, by hour of day and by day of week.
    
### Stage 2 - PREPARE
The dataset consists of individual csv files for each month. The zipped files are downloaded via wget and saved locally. 

### Stage 3 - PROCESS

#### Data Schema
The data columns are consistent throughout the csv files. The left column is the header and right column is the data type. The datatype are declared for python pandas datatype.

    ride_id                       object
    rideable_type               category
    started_at            datetime64[ns]
    ended_at              datetime64[ns]
    start_station_name          category
    start_station_id            category
    end_station_name            category
    end_station_id              category
    start_lat                    float64
    start_lng                    float64
    end_lat                      float64
    end_lng                      float64
    member_casual               category

#### Cleaning
Each csv file is cleaned though a set of rules and saved to a cleaned_csv data folder.

    - drop rows with null data
    - drop rows with trip start time greater than trip stop time
    - drop rows with station ids with lat/lng association abnomaly
    - drop rows with station names with "Temp" indicating test trips
    - drop rows with durations abnomaly (one trip was almost one month)

#### Merge
The cleaned csv files are merged into one big csv file for future manipulations.
Keep in mind, it is also possible to do data manipulations for individual csv and aggregate the analysis data month to month. I have opted for using a big dataframe this time.

## Stage 4 - ANALYZE
### Trip Counts by Station Locations
In order to best visualize the bike trips and their locations. I summarized the trip counts by station and by month and published the data in Tableau.
The dashboard contains two panes so it will be easier to do comparison side by side by filtering different month or rider types.
This is the link to it 
[Cyclistic Bike Trip Count Map](https://public.tableau.com/views/CyclisticTripCountsInterativeMap/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link)
![Cyclistic Bike Trip Count Map_snapshot](Figures/Tableau_map.png)
#### Observations
- It is very clear that bike traffic is densest near Chicago harbor. One great example is at Streeter and Grand Ave. 
- Some areas with great member/casual ratio like Greek Town, U of Illionis at Chicago and South Commons can be good member conversion zone. It can be helpful to identify demographic information by regions to further analyze the local characteristics.
- During the cold months, the casual rides are reduced more significant than rides by members. This means commuters still braved the cold for commutes.

#### Conclusions
- Though it is beyond scope to analyze locational demographics and ride profiles of users. It can be a signicant factor in deciding which user group to covert to membership.
- We also want to rationalize whether we want to convert riders from locations with highest casual/member ratio (more rider candidates to promote membership), or locations with lowest casual/member ratio (this could mean, the demographics are most benefited by ride share membership). There should be difference strategies to approach the two groups for member conversion.


### Ride Count by Rider type and Bike type
![Ride Count by Rider type and Bike type](Figures/Ride_Count_by_BikeType_RiderType.png)
##### Observations:

- Rideable type docked_bike was intitially the only type but is getting phased out. The casual members are slower to phase out docked_bike type. 
- Classic_bike becomes most popular but electric_bike is gaining popularity. 
- It is surprising that annual member which I perceive as communters choose to use classic bike over electric in comparison to casual members. Need to explore further on the motivation of choosing bike types such as bike for health, or leisure, or cost/ availability factors. (It is beyond scope for this analysis, but in reality, it is a import question.)
- The number of rides corresponds to seasons. The rides peaked in July and August months and lowest during December through February. The ride number patterns are very similar between casual riders and members.
##### Conclusion:
    The ride counts between memembr and casual rider are actually quite similar. This is good news, since that also means there are many casual riders that are already familiar with using bike sharing services. We just need to find out further what will motivate them to commit to membership.
    
### Ride Duration Stats by Rider type and Bike type
![Ride Duration Stats (min max) by Rider type and Bike type](Figures/Ride_Duration_by_BikeType_RiderType_minmax.png)
![Ride Duration Stats (25-75) by Rider type and Bike type](Figures/Ride_Duration_by_BikeType_RiderType_25_75.png)
##### Observations:

- Compare left and right figures for 0-100% (min and max), they look very similar. Meaning the maximum length of trips are not good differentiators for the rider types. 
- When looking at 25-75% of the duration stats, we see know that majority of the trips taken by members are shorter and varying less than the casual. This is likely the members use the bike for commuting so the distances are more constant. Whereas for casual riders, the ride purpose vary a lot more hence different length.
  
##### Conclusion:
- The ride duration stats shows that casual riders have more diverse trip length which differ from member riders who probably use bikes for more similar trips such as commuting and shorter trips.

We need to find pattern of casual riders in more detailed partitions. For example, are they doing long trips during weekend or frequent short trips during certain hour of day? This leads to the next analysis.
    
    
### Median Ride Duration by Rider type and Day of Week/Hour of Day
Now we will try to break up the monthly stats two kinds of partitions:

     - Weekday or Weekend
     - Hour of Day
     
 We use Heatmaps with color intensity to show the dimensions:
 
     - Median Duration
     - Trip Count
 
![HeatMap_Ride_MedianDuration_by_DayofWeek](Figures/HeatMap_Ride_MedianDuration_by_DayofWeek.png)
![HeatMap_Ride_Count_by_DayofWeek](Figures/HeatMap_Ride_Count_by_DayofWeek.png)
##### Observations:
- During weekends, both casual and member take longer trips than duraing weekdays. And in general, as observed previously, casual riders take longer trips. 
- During weekends, member riders take less trips than weekdays. This indicates that they do not take rides for pleasure.
- We also observe the seasonality on both duration and trip count (due to weather) that colder months correspond to shorter and less trips.
    
 
![HeatMap_Ride_MedianDuration_by_HourofDay](Figures/HeatMap_Ride_MedianDuration_by_HourofDay.png)
![HeatMap_Ride_Count_by_HourofDay](Figures/HeatMap_Ride_Count_by_HourofDay.png)

##### Observations:
 - As expected, member riders counts are most heavy during 6-7am and 4-6pm due to work commute. Somewhat heavy during lunch hour.
 - As to casual riders, they also show high count of trips during 4-7pm. This should be a key behavior to further investigate. It is likely they will need to compete the use of the bikes with the commuters. The thought is if membership offers some special benefit during this peak time, they may want to convert.
    
![Weekday_Ride_Count_byHr_by_RiderType](Figures/Weekday_Ride_Count_byHr_by_RiderType.png)
![Weekend_Ride_Count_byHr_by_RiderType](Figures/Weekend_Ride_Count_byHr_by_RiderType.png)
![Weekday_Ride_Duration_byHr_by_RiderType](Figures/Weekday_Ride_Duration_byHr_by_RiderType.png)
![Weekend_Ride_Duration_byHr_by_RiderType](Figures/Weekend_Ride_Duration_byHr_by_RiderType.png)
##### Conclusion:
- Since our focus is on converting casual to member riders, we need to examine further if some casual riders actually can benefit from membership from the aspect of saving money or conveniences. We need to further examine the demographics of the users in order to profile them better. 
- From the polularity of ridership for casual riders during the peak hours of the days, we conclude that using bike as transportation is very relevant to the riders' needs. However, I think the best approach is to collect some surveys from casual users who use the bikes frequently to find out their pain points.
    
