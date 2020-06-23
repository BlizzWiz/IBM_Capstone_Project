#  Analysis of Opening Szechuan Restaurants in Toronto
## The Battle of the neighbourhoods - Applied Data Science Capstone Project

## Table of contents
* [1. Introduction](#introduction)
* [2. Data Preparation](#data)
* [3. Methodology](#methodology)
* [4. Analysis](#analysis)
* [5. Discussion](#results)
* [6. Conclusion](#conclusion)
* [7. References](#reference)

## 1. Introduction<a name="introduction"></a>

### 1.1 Backgrounds

The Chinese Canadian community in the Greater Toronto Area (GTA) was first established around 1877. According to Statistics Canada - 2016 Census of Population, there are 631,050 Chinese in the Greater Toronto Area, second only to New York City for largest Chinese community in North America. Food and Restaurant is always a vital part in immigrant communities progress, not only maintaining links to homeland culture but also in allowing immigrants to share this culture with the host community.

### 1.2 Problems

It is well known that there are countless Chinese restaurants located throughout Toronto. However, Szechuan cuisine, which is extremely welcomed by people all over China, is still a newcomer in Toronto. In this capstone project I will try to identify several areas suitable for new **Szechuan Restaurant** in **Greater Toronto Area**, Ontario.

In the following section I will use knowledges and skills learned from IBM Data Science to locate promising neighbourhoods for Szechuan Restaurant.

### 1.3 Interest

Realtors, financial services companies, immigrants services agencies as well as immigrant investors surging into Canada would be very interested in finding new hotspots for investment.

## 2. Data Preparation<a name="data"></a>

### 2.1 Data Sources

According to the criterion of the analysis, factors that will influence our decision are:
* number of existing Chinese restaurants in the neighbourhood
* whether there are Szechuan Restaurant in the neighbourhood, and its distance to the existing Szechuan Restaurants

Data sources will be used in this analysis are:
1. List of Postal code of Canada: This [wikipedia page](https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M) is for obtain all the neighbourhoods, including postal code, borough in Toronto.
2. Coordinates of neighbourhoods: This [CSV](https://cocl.us/Geospatial_data) file is downloaded from Coursera IBM Data Science course page.
3. Restaurants details, including category and location in Toronto neighbourhood will be obtained using __Foursquare API__.
4. Toronto GeoJSON file: City of Toronto Open Data provides [Toronto Boundaries of City of Toronto neighbourhoods](https://open.toronto.ca/dataset/neighbourhoods/). I used this GeoJson file to create choropleth map for Toronto.
5. Immigration and ethnocultural diversity statistics: [City of Toronto Open Data](https://open.toronto.ca/dataset/neighbourhood-profiles/) and [Census of Population, 2016](https://www12.statcan.gc.ca/census-recensement/2016/ref/index-eng.cfm). These two resources are extremely helpful when conducting this analysis.   
6. City Center of Toronto : Here I am using Toronto City Hall as city center. The geographical coordinates are obtained using __GeoPy__.
7. Business Improvement Areas: A Business Improvement Area (BIA) is an association of commercial property owners and tenants within a defined area who work in partnership with the City to create thriving, competitive, and safe business areas that attract shoppers, diners, tourists, and new businesses. The GeoJson file for BIA could be found [here](https://open.toronto.ca/dataset/business-improvement-areas).

### 2.2 Data Cleaning

List of Data obtained and cleaned:
* neighbourhoods List in Toronto (Postal Codes starts with 'M')
* Geographical Coordinates of neighbourhoods in Toronto
* Number of Chinese Restaurants and Szechuan Restaurants in Toronto ( using Foursquare API )
* Toronto GeoJSON file
* East and Southeast Asian population of Toronto
* Coordinates of City Center, Toronto
* Chinatown area GeoJson of Toronto

## 3. Methodology<a name="methodology"></a>

In this section I will try to find patterns for existing Chinese/Szechuan Restaurants in Toronto and spot potential venues for opening new Szechuan Restaurants. Toronto is the most populous city in Canada and the fourth most populous city in North America with 140 neighbourhoods. However, according to [Toronto Neighbourhood Population Profiles](https://www.toronto.ca/wp-content/uploads/2017/12/9386-city-planning-tocore-neighbourhood-population-profiles-aoda-07-04-2016.pdf), the estimated downtown population of Toronto in 2015 is between 242,845 to 245,830, compared to 2,956,024 of [City Toronto](https://www.toronto.ca/city-government/data-research-maps/toronto-at-a-glance/). In that case, there must be some distinct business approaches to open a business in and out of downtown area. Besides, the Immigration and ethnocultural diversity in Toronto, which in our project we will focus on East and Southeast Asians population distributions in Toronto, will have impacts on locations and numbers of our target restaurants.In following sections we are expected to see some totally different strategies for local Chinese and Szechuan restaurants.   

Now we have our data:
* __Toronto GeoJSON file__ and __Toronto neighbourhoods Profiles__ for Choropleth Map based on population density of East and Southeast Asians population
* __Number of Chinese Restaurants and locations in Toronto__
* __Number of Szechuan Restaurants and locations in Toronto__

First I will try to visualize number and locations of Chinese Restaurants in and out of Toronto Downtown area in each neighbourhood.
Then let us try to find patterns for locations of Chinese Restaurants in and out of Toronto Downtown area. I will use __heatmaps__ to conduct this analysis.
Last I will use __Density Based Clustering__ to find patterns of Chinese/Szechuan Restaurants and identify certain cluster groups as candidate neighbourhoods for opening Szechuan Restaurants.

*Most of the traditional clustering techniques, such as k-means, hierarchical and fuzzy clustering, can be used to group data without supervision. However, when applied to tasks with arbitrary shape clusters, or clusters within cluster, the traditional techniques might be unable to achieve good results. That is, elements in the same cluster might not share enough similarity or the performance may be poor. Additionally, Density-based Clustering locates regions of high density that are separated from one another by regions of low density. Density, in this context, is defined as the number of points within a specified radius. In this section, the main focus will be manipulating the data and properties of DBSCAN and observing the resulting clustering.*

## 4. Analysis<a name="analysis"></a>

### 4.1 Visualize number of Chinese Restaurants in Toronto ( neighbourhoods )

Here we can create a bar chart to better analyze the data.
![Neighbourhood Bar Chart ](https://user-images.githubusercontent.com/10215660/85417615-68744080-b5a2-11ea-8156-5f3023078539.png)
We can see from the bar chart that the dominant number of Chinese Restaurants are in Kensington Market, Chinatown and Grange Park, all of them are in downtown area, which totally make sense since it is the commercial and business center of a city.

### 4.2 Visualize number of Chinese Restaurants in Toronto ( boroughs )

We can also create a bar chart to show number of Chinese Restaurants in different Boroughs.
![Borough Bar Chart ](https://user-images.githubusercontent.com/10215660/85417619-6a3e0400-b5a2-11ea-95df-a2dd8eb327ed.png)
However, when we turn to borough analysis, there is a different story. The high density of Chinese restaurants in downtown is totally making sense. Then why the number of Chinese Restaurants in North York area stands out?

### 4.3 Create a choropleth map of Toronto

In order to find reasons for high number of Chinese Restaurants in North York, let us try to create choropleth map of Toronto and see whether there is __positive correlation__ between high population density of East and Southeast Asians and numbers of Chinese Restaurants.
![choropleth map of Toronto ](https://user-images.githubusercontent.com/10215660/85417629-6c07c780-b5a2-11ea-9707-794f3b78ff0d.png)
According to [Toronto Open Data](https://www.toronto.ca/wp-content/uploads/2019/01/9710-City_Planning_2016_Census_Profile_2018_CCA_NorthYork.pdf), the North York neighbourhoods are consisted of ward 6, 8, 15, 16, 17, 18, with high density of East and Southeast Asians origins. From the map above we can conclude that there is __positive correlation__ between high population density of East and Southeast Asians and numbers of Chinese Restaurants. That is our first conclusion.

### 4.4 Create a Map of Szechuan Restaurants

![choropleth map of Szechuan Restaurants](https://user-images.githubusercontent.com/10215660/85417631-6ca05e00-b5a2-11ea-8fed-2d761ca80064.png)
It seems like most of the Szechuan Restaurants tend to stay in clusters with Chinese Restaurants in downtown areas. None of Szechuan Restaurants are spotted in other neighbourhoods besides downtown Toronto. Let us see if we can create a Heatmap to better visualize it.

### 4.5  Create Heatmap for Chinese & Szechuan Restaurants.

![Heatmap for Chinese & Szechuan Restaurants](https://user-images.githubusercontent.com/10215660/85417673-75912f80-b5a2-11ea-92ef-d68e69caf6a1.png)

### 4.6  Create a map for Toronto Business Improvement Area

![Map for Toronto Business Improvement Area](https://user-images.githubusercontent.com/10215660/85417623-6ad69a80-b5a2-11ea-8c16-ccc7e2541eda.png)

This heatmap shows that most of Szechuan Restaurants are in Kensington Market, Chinatown. Let us create a BIA (Business Improvement Area) map to see whether it can back up our conclusion.

*Disclaimer : A Business Improvement Area (BIA) is an association of commercial property owners and tenants within a defined area who work in partnership with the City to create thriving, competitive, and safe business areas that attract shoppers, diners, tourists, and new businesses. The BIA layer represents the active BIAs in the City of Toronto that has been enacted by Council. Each BIA has been defined by a by-law and is represented by a Board of Management. The layer is updated as BIAs are created, amended or deleted by Council.*

Now we can make several conclusions based on the maps above:
1. Most of the Szechuan restaurants (6 out of 12) are in Chinatown. In this area,Szechuan restaurants tend to be in clusters with each other.
2. For Szechuan restaurants outside of Chinatown, most of them are in neighbourhoods with no Szechuan restaurants nearby. However, they are still in Chinese restaurants clusters.
3. There are several candidates locations for Szechuan restaurants, with no Szechuan restaurants nearby and are all in Chinese restaurants clusters.

### 4.7 Find optimal locations for New Szechuan Restaurants using DBSCAN

Now let's continue searching for optimal locations for Szechuan restaurants. In this section I will use Density Based Clustering to locate candidates clusters for opening Szechuan Restaurants.

![DBSCAN Clustering](https://user-images.githubusercontent.com/10215660/85417671-74f89900-b5a2-11ea-9702-ea2c9483b36b.png)
## 5. Discussion <a name="results"></a>

Now we have our candidate clusters for opening new Szechuan Restaurants. In order to find the optimal clusters, here I set criterion for this analysis:
* the restaurant should be in geographic clustering by Chinese restaurant segment;
* the restaurant should be in a neighbourhood with no Szechuan restaurants nearby;
* the restaurant should be located in a BIA area;
* the restaurant should be located in neighbourhoods with high East and Southeast Asian population density;

Let us take a look at clusters, from outlier cluster label 0 to label 12. Outlier with label -1 will not be included in the analysis.

* **Cluster 0: Potential**.
1) In Chinese Restaurants Clusters;
2) No Szechuan Restaurant in Cluster;
3) In Downtown BIA area;
4) High population density of East and Southeast Asian origins.
![Cluster 0](https://user-images.githubusercontent.com/10215660/85417635-6d38f480-b5a2-11ea-8933-290070bc845f.png)

* **Cluster 1: Potential**.
1) In Chinese Restaurants Clusters;
2) Szechuan Restaurant in Cluster;
3) In Downtown BIA area;
4) High population density of East and Southeast Asian origins.
![Cluster 1](https://user-images.githubusercontent.com/10215660/85417638-6dd18b00-b5a2-11ea-9ad0-1afd5a056d56.png)

* **Cluster 2: Not Recommended**.
1) Out of Chinese Restaurants Clusters (potential outlier);
2) No Szechuan Restaurant in Cluster;
3) In Downtown BIA area;
4) High population density of East and Southeast Asian origins.
![Cluster 2](https://user-images.githubusercontent.com/10215660/85417640-6e6a2180-b5a2-11ea-9c51-e31c66597181.png)


* **Cluster 3: Not Recommended**.
1) In Chinese Restaurants Clusters;
2) **FOUR** Szechuan Restaurant in Cluster;
3) In Downtown BIA area;
4) High population density of East and Southeast Asian origins.
![Cluster 3](https://user-images.githubusercontent.com/10215660/85417643-6f02b800-b5a2-11ea-86b6-c7f2137a93aa.png)

* **Cluster 4: Fair**.
1) In Chinese Restaurants Clusters;
2) No Szechuan Restaurant in Cluster;
3) Out of Downtown BIA area;
4) Moderate population density of East and Southeast Asian origins (Near Chinatown).
![Cluster 4](https://user-images.githubusercontent.com/10215660/85417647-6f9b4e80-b5a2-11ea-8926-4721f0cfbc31.png)

* **Cluster 5: Potential**.
1) In Chinese Restaurants Clusters;
2) No Szechuan Restaurant in Cluster;
3) In Downtown BIA area;
4) Low population density of East and Southeast Asian origins (Near Chinatown).
![Cluster 5](https://user-images.githubusercontent.com/10215660/85417652-7033e500-b5a2-11ea-8d32-4ebcd7ae705d.png)

* **Cluster 6: Potential**.
1) In Chinese Restaurants Clusters;
2) No Szechuan Restaurant in Cluster;
3) In Downtown BIA area;
4) Low population density of East and Southeast Asian origins (Near Chinatown).
![Cluster 6](https://user-images.githubusercontent.com/10215660/85417655-70cc7b80-b5a2-11ea-9407-19bade73b189.png)

* **Cluster 7: Not Recommended**.
1) In Chinese Restaurants Clusters;
2) **SIX** Szechuan Restaurant in Cluster;
3) In Downtown BIA area;
4) High population density of East and Southeast Asian origins (Chinatown).
![Cluster 7](https://user-images.githubusercontent.com/10215660/85417658-71651200-b5a2-11ea-8b40-5b43aa630285.png)

* **Cluster 8: Fair**.
1) In Chinese Restaurants Clusters;
2) No Szechuan Restaurant in Cluster;
3) Not in Downtown BIA area;
4) Moderate population density of East and Southeast Asian origins.
![Cluster 8](https://user-images.githubusercontent.com/10215660/85417661-71fda880-b5a2-11ea-947f-5d3c9ebdcc2b.png)

* **Cluster 9: Fair**.
1) In Chinese Restaurants Clusters;
2) No Szechuan Restaurant in Cluster;
3) Not in Downtown BIA area;
4) Moderate population density of East and Southeast Asian origins.
![Cluster 9](https://user-images.githubusercontent.com/10215660/85417662-72963f00-b5a2-11ea-93d1-3987357716c6.png)

* **Cluster 10: Fair**.
1) In Chinese Restaurants Clusters;
2) No Szechuan Restaurant in Cluster;
3) Not in Downtown BIA area;
4) High population density of East and Southeast Asian origins.
![Cluster 10](https://user-images.githubusercontent.com/10215660/85417664-732ed580-b5a2-11ea-8732-fdd5505f2a56.png)

* **Cluster 11: Fair**.
1) In Chinese Restaurants Clusters;
2) No Szechuan Restaurant in Cluster;
3) Not in Downtown BIA area;
4) High population density of East and Southeast Asian origins.
![Cluster 11](https://user-images.githubusercontent.com/10215660/85417666-73c76c00-b5a2-11ea-9344-d83afe75d767.png)

* **Cluster 12: Fair**.
1) In Chinese Restaurants Clusters;
2) No Szechuan Restaurant in Cluster;
3) In Downtown BIA area;
4) Low population density of East and Southeast Asian origins.
![Cluster 12](https://user-images.githubusercontent.com/10215660/85417668-74600280-b5a2-11ea-9c88-4150af18a750.png)

## 6. Conclusion <a name="conclusion"></a>

![Potential Cluster](https://user-images.githubusercontent.com/10215660/85417674-7629c600-b5a2-11ea-8e05-347e070bed4e.png)

Finally we have spotted 4 clusters with high potential to open new business. We give it star label with colour blue.

This concludes our analysis. We have spot **4 cluster Areas** with high potential to open Szechuan restaurants, with Chinese Restaurants nearby, no Szechuan Restaurant in cluster, high population density of East and Southeast Asian origins and all clusters in Downtown Toronto Business Improvement Area.

Please notice this analysis is only a starting line to find and define business patterns of Restaurant & Food service industry. Except for locations, there are still tons of factors that should be taken into considerations such as anticipated sales volume, accessibility to potential customers, rents, security issues et cetera.

## 7. Reference <a name="references"></a>

[1] Vieregge, M., Lin, J. J., Drakopoulos, R., & Bruggmann, C. (2009). Immigrants Perception of Ethnic Restaurants: The Case of Asian Immigrants Perception of Chinese Restaurants in Switzerland. Tourism Culture & Communication, 9(1), 49–64. doi: 10.3727/109830409787556684
[2] [Finding Optimal Locations of New Stores by IBM](http://ibmdecisionoptimization.github.io/docplex-doc/mp/chicago_coffee_shops.html)
[3] [Generating GeoJSON File for Toronto FSAs by Amy Gordon](https://medium.com/dataexplorations/generating-geojson-file-for-toronto-fsas-9b478a059f04)
[4] [Housing Sales Prices & Venues Data Analysis of Istanbul by Sercan Yıldız](https://www.linkedin.com/pulse/housing-sales-prices-venues-data-analysis-ofistanbul-sercan-y%C4%B1ld%C4%B1z/)
[5] [Visualizing Geospatial Data in Python Using Folium by Aly Sivji](https://alysivji.github.io/getting-started-with-folium.html)
[6] [Folium Documentation 0.11.0](https://python-visualization.github.io/folium/)
[7] [Matplotlib 3.2.1 Documentation](https://matplotlib.org/3.2.1/contents.html)
[8] [Scikit-Learn User Guide](https://scikit-learn.org/stable/user_guide.html)
[9] [Statistics Canada](https://www12.statcan.gc.ca/census-recensement/2016/dp-pd/hlt-fst/pd-pl/comprehensive.cfm)
[10] [Toronto Open Data](https://open.toronto.ca/)
[11] [Color encyclopedia](https://www.colorhexa.com/)
[12] [Fousquare Developer Documentation](https://developer.foursquare.com/docs/places-api/authentication)
