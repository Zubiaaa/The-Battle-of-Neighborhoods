# Comparative Analysis of Venues in Makati and Cebu City in the Philippines 

## I. Introduction: Business Problem 
 
### Problem Statement
This capstone project aims to explore in what ways are the venues in the neighbourhood-areas of 
Makati City and Metropolitan Cebu similar or dissimilar. 
After working overseas for over 10 years, in countries such as Thailand and Singapore, a usual 
question I get from friends and colleagues would be about what to see and where to go outside the 
capital of Metro Manila or an alternative to Makati? 
This Capstone project aims to study the landscape of venues available in the two metropolitan areas, 
Makati City and Metropolitan Cebu. Both locations have business establishments, industrial sites, 
and a destination for local and foreign talent. It will be interesting to see what common venues 
comprise each locale, particularly Metro Cebu.

### Background
In the Philippines, Makati City is known as the financial capital wherein a large number of local and 
multinational businesses are established. Situated in the country's National Capital Region (NCR), 
Makati was founded on November 4, 1670 and has a total land area of 21.57 sq.km (8.33 sq mi)[4]. 
It has a population of 582,602 based on the 2015 Census. An assortment of banking and financial 
institutions, commercial shopping spaces and entertainment hubs can be found here. The wide 
availability and developed modes of transportation such as the Metro Rail System (MRT), buses and 
jeepneys help promote trading and the bustling business-centric environment.
South of NCR is Metropolitan Cebu or Metro Cebu for short. A metropolitan area located at the 
central eastern portion of Cebu island in the Central Visayas Region of Philippines. It has an area of 
1,062.88 sq.km (410.38 sq mi)and a population of 2,849,213 according to the 2015 Census. In 2005 
and 2007, the region was host to the Southeast Asian Games and ASEAN Summit and 2nd East Asia 
Summit, respectively[1]. Having a strategic location in the center of the Philippines, Metro Cebu has 
seen growth in various industries, such as trade, manufacturing, Business Process Outsourcing 
(BPO), and real estate. The other surrounding islands provides an assortment of leisure spots, 
picturesque beaches and dive sites outside the business-centric metro.

### Target Audience
The study is applicable to investors and traders who plan to visit the country for business. Having an 
idea of the popular establishments accessible to the local population of Makati and Metro Cebu. 
Another perspective is that, would Cebu City be an alternative to Makati, for individuals who plan to 
bring business over or just simply to visit as tourists and experience what each location has to offer.

## II. Data 
1. Online/Wikipedia Scraping
• Makati and Metro Cebu Postal Codes, Area, and Location Names. Philippine-based 
datasets will have different column naming and arrangement compared to the 
Toronto datasets in the course and will need to be standardized. 
• HTML table location of Postal Codes and figures are ordered differently within the 
webpage. As such, some changes in BeautifulSoup API parameters and Pandas are 
needed to create and clean the dataframes. 
2. Geographical Coordinates
• Philippines Geo-coordinates. After researching, a consolidated csv will be created 
from www.aggdata.com and www.geonames.org, which will have the Postal/ZIP code 
for each area. 
• Latitude and longitude coordinates will be merged with Makati and Metro Cebu 
Postal Codes. 
3. Venue data
• The Foursquare API will be used to collect venues based on the geo-coordinates of 
each Postal code or area. 
4. Other methods will be mentioned as the study progresses, applying Coursera-learned 
material on different set of data.
• Charts to describe statistics of Makati and Metro Cebu 
• Folium library to visualize location data 
• Data wrangling, filtering dataframes and cleaning the data. 

## III. Methodology 
1. Generate the starting raw data for the areas in the Philippines and their corresponding Zip or 
Postal codes.
• Source: https://en.wikipedia.org/wiki/List_of_ZIP_codes_in_the_Philippines. 
• Use the function request.get with HTML text option to create the raw object data. 
• BeautifulSoup API was used to pull HTML data and save in a soup object. 
2. From researching online (www.aggdata.com and www.geonames.org), a consolidated zip 
code csv file (PHZIP.csv) was generated and converted into a Pandas dataframe (df_PH_geo) 
by pd.read_csv. 
3. Generate the working dataframes for the Makati City and Metro Cebu. DF column names 
were standardized and cleaned.
• Main groups under study, Makati and Metro Cebu, had column names of "Province or 
city", are renamed to "Area". 
• Postal code column name was renamed to ZIP Code. 
• From the raw data, ZIP codes can be used to refer to a neighbourhood or commercial 
establishments. "Area" was used as the column name. 
4. Create the Makati dataframe by filtering Area = 'Makati' and applying merging with 
df_PH_geo. Zip code was referenced as the join parameter. 
5. For the Metro Cebu dataframe, an intermediate step was needed. Metropolitan Cebu is the 
collection of cities and smaller municipalities in Cebu province [1]. A list was created for the 
member locations (mtro_cbu = ['Carcar', 'Cebu City', 'Danao', 'Lapu-Lapu', 'Mandaue', 'Naga', 
'Talisay','Compostela', 'Consolacion', 'Cordova', 'Liloan', 'Minglanilla', 'San Fernando']). The 
working dataframe was made by filtering based on this list [df_cebu['Area'].isin(mtro_cbu)] . 
Applied the same filtering and merging operations from step 4, but using 'Cebu'. 
6. Folium map object creation for Makati and Metro Cebu.
• Geopy was used for geocode coordinate data 
• Labelling points with Area labels 
7. Utilizing the Foursquare API to explore the areas in Makati and Metro Cebu.
• Provide Foursquare credentials and other client details. 
• Preview venue information on selected locations. Greenbelt, for Makati City and 
Cebu City, for Metro Cebu. 
8. Use getNearbyVenues function for each area in Makati and Metro Cebu to create a new 
dataframes. 
9. Perform one_hot encoding function to get the frequency of venue occurrence in each area. 
Then group rows (using groupby method) by area and by taking the mean of the frequency 
of occurrence of each venue category. Rank by top 10. 
10. Apply K-means clustering. 
11. Consolidate by merging cluster data and the top 10 venues for each area. 
12. Visualization of resulting clusters of venues observed in the Makati and Metro Cebu 
locations.
• Latitude and longitude geodata were used for the Folium map. 
• Marker definitions, colors and label data are based on kcluster data derived from the 
previous steps. 
• Kcluster coordinates are then overlayed on the respective maps. 
13. Examine and analyze each cluster for Makati and Metro Cebu and observed similarities and 
differences.

## IV. Results 
Makati City
1. Overall, the west region of Makati shows the highest density of venue clusters. Notedly, 
majority is comprised of cluster 2 coffee shop establishments and various forms of 
restaurants. It is shown to be a densely commercialized area bordered by Palanan, Bel-Air, 
Makati-Ayala Commercial Center and Dasmarinas Village. 
2. At the middle of cluster 2, interestingly cluster 0 is comprised of Karaoke spots and Beer 
Gardens, followed by hotel residential areas. 
3. Clusters 1, 4 , 5 and 6 are situated farther apart however similarly they are also restaurant 
and convenience store venues. 
4. Cluster 3, made up of La Paz, Singkamas, and Tejeros areas is where the market place is 
located, north-west of the hotel areas. The placement provides convenience and 
accessibility to both restaurant and hotel venues. 

A higher concentration of clusters (majority cluster 2) are more prominent on the west side of 
Makati.
 
Metropolitan Cebu
1. Metro Cebu venue clusters are isolated to each other, as compared to Makati City, however 
are strategically located lined-up along the eastern-region of Cebu island. 
2. Cluster 3 has more commercial establishments in the region. Like Makati City, top venues are 
Cafe shops and Fast Food restaurants found in Cebu City capital and Mandaue. 
3. A unique difference with Metro Cebu is the variety venue types offered in each cluster. 
4. The top venue in cluster 0 at Liloan is it's mountain region and veterinary establishments. An 
area for outdoor nature and animal sight-seeing enthusiasts. 
5. Cordova in cluster 1 has an assortment of recreational venues for enjoyment. A basketball 
stadium being the top, followed by beach and resorts. 
6. Cluster 2 (Danao) and cluster 4 (Minglanilla) are comprised of restaurants and BBQ venues. 
7. Cluster 5 (Consolacion) and cluster 6 (Talisay) have parks and beaches.
 
## V. Discussion 
Based on the venue classification types observed, Makati City has long been well known for the 
upscale nature of businesses and establishments in its locality. It is shown to be densely 
commercialized with food-based, lifestyle and shopping venues evident from cluster 2 that surround 
the hotel areas of Dasmarinas and Valenzuela.
Sample Top Venues in Greenbelt, Makati Sample Top Venues in Metro Cebu, Cebu
Filipinos do like their food, as the top establishments are an assortment of restaurants from the 2 
sample locations.
Top 10 Venue Categories in Makati and Metro Cebu 
Makati City (First 10 Areas)
Frequent occurrence of fast food establishments, convenience stores and coffee shops as top 
categories.
Metro Cebu (All 8 Areas)
Metro Cebu top 2 categories are diverse, covering not just food establishments but outdoor and 
recreational venues as well. The study also gave light to the limited Foursquare and other user 
review site availability that can be aggregated for the study.
Hopefully with more exposure for the Cebu region overall, 
more people from other parts of the globe can discover the 
venues it offers or even open new business in the future.
Metropolitan Cebu has low density of venue groups that are isolated across the island. This allows 
more potential for commercial growth in the future and given the strategic location. Surrounded by 
shipping trade-routes and neighbouring islands. It is evident that the Cebu City capital is the most 
commercialized with top venue-classifications like Makati City such as fast-food restaurants and 
hotels. The strength and unique attribute of Metro Cebu lies in the diversity of what each other 
venue cluster has to offer. Outside of the bustling city-scapes of the capital, you have a mountain 
range in one location and on the other end of the spectrum, you have beaches and resorts.

## VI. Conclusion 
The Data Science Capstone goal was to compare the venue similarities and differences between 
Makati City and Metropolitan Cebu locations in the Philippines. Folium maps representing Makati 
City and Metro Cebu locales were successfully generated, combined with cluster info from 
Foursquare statistics are overlaid in order to represent the clustering of venues/establishments 
present in each area under study.
For stakeholders, Metro Cebu is an ideal location to visit to experience nature and the outdoors, 
with the option to satisfy the upscale commercial life as provided by Cebu City. Visualization data in 
this study also help to show the spread of clusters with the help of geo-positions in Metro Cebu. 
Makati benefits from accessibility and modern transportation, while Metro Cebu clusters are
separated such that additional transportation and travel planning may be required. Whichever 
location you choose, there is something for you in the Philippines. 

### References
[1] https://en.wikipedia.org/wiki/Metro_Cebu
[2] https://www.aggdata.com/
[3] https://geonames.org/
[4] https://psa.gov.ph
