# BU Sustainability: Water Bottle Filling Stations-Team C
This repo suggests where to put new water bottle filling stations on BU's campus.<br>
Team C member<br>
Jingyi Zhang, jyz0328@bu.edu (Team Rep)<br>
Houjie Jiang, houjie@bu.edu<br>
Tristan Lee, tlee10@bu.edu<br>
Yida Wang, yidwang@bu.edu<br>
Finn Jensen, fjensen@bu.edu


## How to run code instruction
- Step 0: Download all folders in this branch, keep all files in their original folder.
- Step 0.5: Download `updated_densitymap_hourly_cleaned.csv`from https://drive.google.com/drive/folders/11Zu-7h4pp9KWnJbkMMpTo6H_6xsmfzBb, and put it into  [`original_data`](https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/original_data) folder, this file is not included in this repo because it is very large. However this file is our necessary file for running code.
- Step 1: Now open [`code`](https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/code) folder , run [`wifi_data_process.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/code/wifi_data_process.ipynb) to produce the  processed wifi(foot traffic data) [`updated_wifi_data.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/updated_wifi_data.csv).
- Step 2: Then run [`produce_kitchen_station_tables.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/code/produce_kitchen_station_tables.ipynb) to produce the processed kitchen data and existed water station data [`Kitchens.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/Kitchens.csv) and [`Stations.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/Stations.csv).
- Step 3: Then run [`seperate_covered_floor_or_not.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/code/seperate_covered_floor_or_not.ipynb) to produce the [`covered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/covered_floors.csv) and [`uncovered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/uncovered_floors.csv).
- Step 4: Finally, run [`final_selection_and_visualization`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/code/final_selection_and_visualization.ipynb) to produce all images and html files and  in the `visualization` folder, as well as [`selected_candidates.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/selected_candidates.csv).
- Now you can see all generated datasets and visualization files in [`new_data`](https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/new_data) folder and [`visualization`](https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/visualization) folder. To ensure better effect you can empty all files in these two folders before you run code.
 
## Base/Extended Questions Addressed
- Key Question 1:Establish how many students and employees are impacted by each current water filling station and kitchen.
  - From our filtered water station and kitchen list [`covered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/covered_floors.csv), we found the mean and max foot traffic of these floors. This gives a rough idea of how many people each water filling station affects. 
- Key Question 2:Establish a full list of the factors and criteria for installation of new filling stations.
  - We separate the full list into the filter section and factor section.<br> 
    Filter section: as I mentioned in base analysis before, with following exclusion:<br>
  - Exclude covered floor (water station & kitchen floor)
  - Exclude Parking ,Housing, and Commercial buildings . For residential buildings we only consider large dorm-style residences 
  - Exclude dining hall floors<br>
     Now we get [`uncovered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/uncovered_floors.csv) for further new station choosing.<br>
    Factor section : We list the following four factors for installation of new filling station consideration.<br>
  - Mean Density (Foot Traffic)
  - Max Density (Foot Traffic)
  - Air Condition (Mechanical Ventilation)
  - High Volume
- Key Question 3:Determine criteria and factors for new, high-priority water filling station locations.
  - As we explained in the key question 2, we make some filters and get `uncovered_floors.csv` and then take mean density, max density, air condition and high volume as factors. Then we build a weight score standard based on these factors , and select top 50 weight score floors from `uncovered_floors.csv`  and define them as new candidates. Here is the details about this weight score standard:
  - Here is the details about this weight score standard:
  - Weight for selecting new water bottle station = ( mean density × 1 + max density × 0.3 ) × volume factor × ventilation factor
  - Volume factor is: 10 if high volume, else 1 
  - Ventilation factor is: 1 if Mechanical Ventilation is 'Yes', 2 if Mechanical Ventilation is 'Partial', 3 if Mechanical Ventilation is 'No'
  - Now we get [`selected_candidates.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/selected_candidates.csv) shows the new stations we select based on this weight score standard

- Key Question 4:Establish how many students and employees would have access to each of the new filling stations.
  - As we mentioned in the last question, we build a new weight standard for finding new stations and we get [`selected_candidates.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/selected_candidates.csv). Mean and max density in this sheet is the estimation of how many students and employees would be impacted by proposed new filling stations.
- Key question 5:A geospatial map of new water filling stations highlighted by priority (page 21-22)
  - We build a html file called [`Top_15_candidates_map_by_weight.html`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/visualization/top_15_candidates_map_by_weight.html). When we download this html file and open it, we can see a google map showing the location of top 15 new water stations highlighted by weights as priority. Top 1-5 are marked as green points, rank 6 -10 are marked as orange points , and rank 11-15 are marked as red points. 
  - When we click on these new station points, details of that point will show, including building address, building floor, weight score, building type and ranking. 


## Code Methods
We combine the data cleaning and analysis steps together in our various notebooks:
- [`wifi_data_process.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/code/wifi_data_process.ipynb) :This is our first step code. In this file, we do 2 Data Filter and Processing steps for original wifi data.
  - (1) We exclude Housing, and Commercial buildings because they're private . For residential buildings we only consider large dorm-style residences for similar reason. We also exclude parking building or floors since people usually do not stay long here.
  - (2) We simplify our analysis by calculating and focusing on average and max density counts for each floor. This helps us quickly identify when and where traffic peaks.
  - After running this code, we can get [`updated_wifi_data.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/updated_wifi_data.csv) in `new_data` folder .
- [`produce_kitchen_station_tables.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/code/produce_kitchen_station_tables.ipynb) : This is our second step code. We do data Filter and Processing for original Kitchen and Station data, includes:
  - For existed water stations, we combine address A and B into a full building description and retain only [Floor,Building Description,Space@Bu room,Quantity,Date Installed,Type].
  - For the kitchens, we retain only [ Floor, Building, Code,Building Description,Room #].
  - After running this code, we can get [`Kitchens.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/Kitchens.csv) and [`Stations.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/Stations.csv) in `new_data` folder.
- [`separate_covered_floor_or_not.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/code/seperate_covered_floor_or_not.ipynb): This is our third step code. In this code, we do further selection before answer our key question, includes:
  - Exclude covered floor (water station & kitchen floor). Floors with existing water bottle stations or kitchens are excluded from new station consideration because they already have drinking water access, and kitchen floors often restrict public access.
  - Add high volume and air condition (Mechanical Ventilation)
  - Delete dining hall floors. Many types of beverage are offered in dining halls.
  - After running this code, we can get [`covered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/covered_floors.csv) and [`uncovered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/uncovered_floors.csv) in `new_data` folder as result data , which means floors with or without existing water staton and kitchen after data filtering and processing.
- [`final_selection_and_visualization`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/code/final_selection_and_visualization.ipynb): This is our final step code. In this code, we make final selection for installing new water bottle station.We also make visualizations for covered floors sheet, uncovered floors sheet and select candidates sheet. Answers to key questions are based on this code.Here are all the steps we done for this code file :
  - We build a weight score standard based on [mean density ,max density, Mechanical Ventilation , high volume] , select top 50 weight score floors from `uncovered_floors.csv` ,and then define them as new candidates. The formed new candidates are called [`selected_candidates.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/selected_candidates.csv).
  - Here is the details about this weight score standard:
  - Weight for selecting new water bottle station = ( mean density × 1 + max density × 0.3 ) × volume factor × ventilation factor
  - Volume factor is: 10 if high volume, else 1 
  - We make a google map for showing the location of top 15 new water stations highlighted by weights as priority, this google map is saved as [`top_15_candidates_map_by_weight.html`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/visualization/top_15_candidates_map_by_weight.html). When we click on these new station points, details of that point will show, including building address, building floor, weight score, building type and ranking.
  - We make some pie charts and bar charts as visualizations for covered floors sheet, uncovered floors sheet and select candidates sheet, see more details in Visualization section below.
  - All the images and `top_15_candidates_map_by_weight.html` are saved in [`visualization`](https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/visualization) folder. `selected_candidates.csv` is saved in [`new_data`](https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/new_data) folder.
 
## Visualization 
Explanation about all the images and html files saved in [`visualization`](https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/visualization) folder:
- `mean and max density.png`: a figure which shows average mean and max density for covered and uncovered floors
- `mechanical ventilation influence.png`: a figure which shows average mean and max density for covered and uncovered floors with Mechanical Ventilation status shown
- `Top_15_covered_floors_based_on_Mean_density.png` and `Top_15_covered_floors_based_on_Mean_density_with_Max_density_also_shown.png`: the bar chart to display top15 mean density covered floors and the bar chart to display top15 mean density covered floor with max density also shown.
- `Top_15_selected_candidates_based_on_Mean_density.png` and `Top_15_selected_candidates_based_on_Mean_density_with_Max_density_also_shown.png` and `Top_15_Selected_Candidates_Based_on_Mean_Density_with_Weights_also_Shown.png`: the bar chart to display top15 selected new stations based on mean density, the bar chart to display top15 selected new stations based on mean density with max density also shown, and the bar chart to display top15 selected new stations based on mean density with weights also shown.
- `Top_15_selected_candidates_based_on_Weight.png` and `Top_15_Selected_Candidates_Based_on_Weight_with_Max_Density_Also_Shown.png` and `Top_15_Selected_Candidates_Based_on_Weight_with_Mean_Density_Also_Shown.png` : the bar chart to display top15 selected new stations based on weight, the bar chart to display top15 selected new stations based on weight with max density also shown, and the bar chart to display top15 selected new stations based on wieght with mean density also shown.
- [`top_15_candidates_map_by_weight.html`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/visualization/top_15_candidates_map_by_weight.html):  The google map for showing the location of top 15 new water stations highlighted by weights as priority. When we click on these new station points, details of that point will show, including building address, building floor, weight score, building type and ranking.
- `Distribution_of_Building_Types_for_selected_candidates.png` and `Distribution_of_Mechanical_Ventilation_for_selected_candidates.png`:pie charts for showing distribution of building types and Mechanical Ventilation for selected candidates

## Original Datsets
The various CSV files are original data given by client, which are all in [`original_data`](https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/original_data)
- [`updated_densitymap_hourly_cleaned.csv`]（https://drive.google.com/drive/folders/11Zu-7h4pp9KWnJbkMMpTo6H_6xsmfzBb): This dataset records density count each hour for various BU floors, which can represent foot traffic, since it is too large we do not put it into github, but it is required for running [`wifi_data_process.ipynb`] .
- [`Bottle Fillers Inventory - ongoing.xlsx`]and [`Building and Kitchen List.xlsx`]  (https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/original_data): These two datasets include detailed information for places with existed bottle station and kitchen,including
- [`High Volume Event Spaces.xlsx - Sheet1.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/original_data/High%20Volume%20Event%20Spaces.xlsx%20-%20Sheet1.csv) : This dataset lists all detailed information about high volume event places.

## New Datsets
The various CSV files are created during our coding process, which are all in [`new_data`](https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/new_data):
- [`updated_wifi_data.csv`]((https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/updated_wifi_data.csv): this file can be produced by running [`wifi_data_process.ipynb`], which contains mean and max foot traffic data for each building and floor (instead of showing many hors' density for same place), and exclude certain building types as we mentionded on answeres for key questions.This file is then used for running `seperate_covered_floor_or_not.ipynb` .
- [`Kitchens.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/Kitchens.csv) and [`Stations.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/Stations.csv): these files can be produced by running `produce_kitchen_station_tables.ipynb`,which contain the locations of kitchens and water bottle filling stations. `Kitchens.csv` also contains the building type and ventilation status for each building. These files are then used for running `seperate_covered_floor_or_not.ipynb` .
- [`covered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/covered_floors.csv) and [`uncovered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/uncovered_floors.csv): these files can be produced by running `seperate_covered_floor_or_not.ipynb`, which contain information about each building/floor, such as foot traffic information, ventilation, and latitude/longitude. These two files are used for running `final_selection_and_visualization.ipynb`
- [`selected_candidates.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/new_data/selected_candidates.csv): this file can be produced by running `final_selection_and_visualization.ipynb` , which contains various information about our selected building/floor candidates for new water filling stations.
