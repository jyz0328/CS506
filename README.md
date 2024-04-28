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
  - From our filtered water station and kitchen list `covered_floors.csv`, we found the mean and max foot traffic of these floors. This gives a rough idea of how many people each water filling station affects. 
- Key Question 2:Establish a full list of the factors and criteria for installation of new filling stations.
  - We separate the full list into the filter section and factor section.<br> 
    Filter section: as I mentioned in base analysis before, with following exclusion:<br>
     Exclude covered floor (water station & kitchen floor)<br>
     Exclude Parking ,Housing, and Commercial buildings . For residential buildings we only consider large dorm-style residences<br> 
     Exclude dining hall floors<br>
     Now we get `uncovered_floors.csv` for further new station choosing.<br>
    Factor section : We list the following four factors for installation of new filling station consideration.<br>
     Mean Density (Foot Traffic)<br>
     Max Density (Foot Traffic)<br>
     Air Condition (Mechanical Ventilation)<br>
     High Volume<br>
- Key Question 3:Determine criteria and factors for new, high-priority water filling station locations.
  - As we explained in the key question 2, we make some filters and get uncovered_floors.csv and then take mean density, max density, air condition and high volume as factors. Then we build a weight score standard based on these factors , and select top 50 weight score floors from uncovered_floors.csv  and define them as new candidates. Here is the details about this weight score standard:
  - Here is the details about this weight score standard:
  - Weight for selecting new water bottle station = ( mean density × 1 + max density × 0.3 ) × volume factor × ventilation factor
  - Volume factor is: 10 if high volume, else 1 
  -Ventilation factor is: 1 if Mechanical Ventilation is 'Yes', 2 if Mechanical Ventilation is 'Partial', 3 if Mechanical Ventilation is 'No'

- How many people would have access to the new water bottle filling stations?
  - Similar to the covered floors(existed water bottle station floors&kitchen floors), we found the foot traffic for floors without a water filling station nor a kitchen, which roughly estimates the imapct of each one.
- How can we determine new, high priority water filling stations?
  - We use the same factors for determing candidates (foot traffic, ventilation, etc.) and weight them in a systematic way to determine a "goodness" score, which we used to determine the candidates given here but can also be used to predict how good future buildings/floors are for stations (see Methods section).
- Determine criteria and factors for new, high-priority water filling station locations.
  - For uncovered floors,as we mentioned in [What factors should be considered for new water bottle filling stations?], we consider foot traffic (average and maximum), existing locations of stations/kitchens, mechanical ventilation (air conditioning), and high volume spacesas criteria and factors. Specifically, we assign a weight to each building/floor based on the average/maximum foot traffic, the air ventilation status, and the markers for high volume spaces as follows: take the sum of the average foot traffic and 0.3 times the maximum traffic, multiply it by 1, 2, or 3 if the ventilation is fully present, partial, or absent, and rank high volume spaces as 10 weight. Also we filter some building types, for residential building we only remain large dormitory, we also exclude Parking & Commercial & Housing building type from consideration. We also exclude floors of dinning hall mannually. Dinning hall floors are also excludes directly.

## Methods
We combine the data cleaning and analysis steps together in our various notebooks:
- [`new_wifi_data_process.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/new_wifi_data_process.ipynb): this file reads the foot traffic data (not on GitHub for the sake of saving space), and computes the various statistics we need for each building and floor. Specifically, we used `mean_density_cnt` (average foot traffic over the day) and `max_density_cnt` (peak foot traffic over the day), among other location data. This file produces the `wifi_update.csv` file containing the information for each building and floor (see Datasets section). Besides, this file removes floors with Parking & Commercial & Housing as building type, since these places are usually private . For  residential building this file only remain large dormitory types, other residential types are consdiered private and should not be selected.
- [`produce_kitchen_station_tables.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/produce_kitchen_station_tables.ipynb) : this files read original kitchen data and existed water station data(see from original data folder),and initially process existed bottle station data and kitchen data, for kitchen this file only remain[ Floor	Building, Code,	Building Description,Room #],for existed bottle staion data this file only remain [Floor,Building Description,Space@Bu room,Quantity,Date Installed,Type] .Then this file formed `Kitchens.csv` and `Stations.csv` for further use.
- [`separate_covered_floor_or_not.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/seperate_covered_floor_or_not.ipynb): this file combines the water filling station and kitchen data with the foot traffic data to determine teh foot traffic information for each floor covered by a water source (existed water bottle station floors&kitchen floors), and not covered by a water source. It also converts the different floor naming conventions into a single standard. Mechanical Ventilation information is also added for foot traffic data and parking floors are excluded. Hight volume or not is also added into uncovered floors. Dinning hall floors are excluded from uncovered floors.This file produces the `covered_floors.csv` and `uncovered_floors.csv` files (see Datasets section).
- [`Mannually_exlcuded_report.pdf`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/Mannually_exlcuded_report.pdf):explanation about why exclude certain dinning hall floors.
- [`select_new_stations.ipynb`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/select_new_stations.ipynb): this file takes the uncovered floors data and selects some of them as candidates for new water filling stations. Specifically, we assign a weight to each building/floor based on the average/maximum foot traffic, the air ventilation status, and the markers for high volume spaces as follows: take the sum of the average foot traffic and 0.3 times the maximum traffic, multiply it by 1, 2, or 3 if the ventilation is fully present, partial, or absent, and rank high volume spaces at the 10 weight). In terms of rationale, average foot traffic should be more important than the maximum traffic, but both should matter. Also, places with less air conditioning should gain more priority because of water loss through sweat, the cooling affect drinking water can have, and so on. After assigning these weights, we take the top 50 to be our candidates. This file also produces various visualizations of the covered&uncovered floors, candidates, and the HTML map file. This file produces the `mean and max density.png`,`mechanical ventilation influence.png`, `selected_candidates.csv``top_15_candidates_map_by_weight.html` and all png files with "top 15..." as heading (see Datasets section).

## Datsets (notice:dateasets from original data is not included in current readme )
The various CSV files are a combination of given data and intermediary tables used in our process:
- [`updated_densitymap_hourly_cleaned.csv`]（https://drive.google.com/drive/folders/11Zu-7h4pp9KWnJbkMMpTo6H_6xsmfzBb): original foot traffic sheet, since it is too large we do not put it into github, but it is required for running [`new_wifi_data_process.ipynb`] .
- [`wifi_update.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/wifi_update.csv): this file can be produced by running [`new_wifi_data_process.ipynb`], which contains mean and max foot traffic data for each building and floor (instead of showing many hors' density for same place), and exclude certain building types as we mentionded on answeres for key questions.This file is then used for running `seperate_covered_floor_or_not.ipynb` .
- [`Bottle Fillers Inventory - ongoing.xlsx`]and [`Building and Kitchen List.xlsx`]  (https://github.com/BU-Spark/ds-bu-sustainability-water/tree/team-c_extension/sp24-team-c/original_data): Oringal existed bottle station data and kitchen data, these two data sheets are required for running [`produce_kitchen_station_tables.ipynb`] and `seperate_covered_floor_or_not.ipynb`
- [`Kitchens.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/Kitchens.csv) and [`Stations.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/Stations.csv): these files can be produced by running `produce_kitchen_station_tables.ipynb`,which contain the locations of kitchens and water bottle filling stations. `Kitchens.csv` also contains the building type and ventilation status for each building. These files are then used for running `seperate_covered_floor_or_not.ipynb` .
- [`High Volume Event Spaces.xlsx - Sheet1.csv`] ( https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/High%20Volume%20Event%20Spaces.xlsx%20-%20Sheet1.csv ) : This file lists detail information about high volume event places and is required for running `seperate_covered_floor_or_not.ipynb`.
- [`covered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/covered_floors.csv) and [`uncovered_floors.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/uncovered_floors.csv): these files can be produced by running `seperate_covered_floor_or_not.ipynb`, which contain information about each building/floor, such as foot traffic information, ventilation, and latitude/longitude. These two files are used for running `select_new_stations.ipynb`
- [`selected_candidates.csv`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/selected_candidates.csv): this file can be produced by running `select_new_stations.ipynb` , which contains various information about our selected building/floor candidates for new water filling stations.
- [`top_15_candidates_map_by_weight.html`](https://github.com/BU-Spark/ds-bu-sustainability-water/blob/team-c_extension/sp24-team-c/data/top_15_candidates_map_by_weight.html): this file can be produced by running `select_new_stations.ipynb` , this is the HTML file that contains map of the top 15 candidates from uncovered floors that can be viewed using a brower.
