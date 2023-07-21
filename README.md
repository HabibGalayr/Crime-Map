# London Boroughs
Using SQL &amp; SpatiaLite to extract information on London boroughs.
Task 1: Spatial Extent of London
Union multiple conditions
Using spatial functions of “Area”, “Mbr” along with “Max”, “Min”
Find out London boroughs with the smallest / largest area, with the lowest / highest
population density, as the southernmost / northernmost / most east / most west
borough.
QUERY ANALYSIS
The steps involved are as follows:
1.	Create a table called 'london_guinness_book' by selecting all columns from the 'london_boroughs' table. This creates a new table 'london_guinness_book' that is a copy of the 'london_boroughs' table.
2.	Add a geometry column to the 'london_guinness_book' table using the RecoverGeometryColumn function. This adds a new column called 'geometry' to the 'london_guinness_book' table and specifies that it contains polygon geometries in the British National Grid (EPSG code 27700) with X and Y coordinates.
3.	Select the London borough name, region, area, population density, and north, south, east, and west extents:
From my findings, using the ‘ORDER BY’ function – 
•	Smallest Area – CITY 
•	Largest Area – Bromley
•	Lowest Pop. Density – Bromley
•	Highest Pop. Density – Islington
•	Northernmost Borough – Enfield
•	Most East Borough – Havering
•	Most West Borough – Hillingdon
•	Southernmost Borough – Croydon

 
Figure 1- Table of London_guiness_book, showing London boroughs of interest.
TASK 4 – 
Business / Services within Boroughs (Points within Polygons)
Open Visual Shapefile “credit_union.shp”
Join or Left Join the Point table and the Polygon table
Using spatial relation of “Contain”
Find out which credit unions within which London boroughs.
Try the same procedure with the table of “UndergroundStations”
Find out which underground stations within which London boroughs. 
Query Analysis
1.	The first query calculates the number of credit unions for each London borough. It joins the credit_union and london_guinness_book tables on the condition that the geometry of the credit union is contained within the geometry of the London borough using the ST_Contains() function. It then groups the results by the name of the London borough and sorts them in alphabetical order.
2.	The second query selects the name of the underground station, the name of the line it belongs to, and the name of the London borough where it is located. It joins the UndergroundStations and london_guinness_book tables on the condition that the geometry of the underground station is contained within the geometry of the London borough using the ST_Contains() function. It then sorts the results by the line name and station name.
3.	The third query is similar to the second query, but it uses a left join instead of an inner join. This ensures that all underground stations are included in the result set, even if there is no corresponding London borough that contains them.
4.	The fourth query calculates the number of tube stations for each London borough. It joins the UndergroundStations and london_guinness_book tables on the condition that the geometry of the underground station is contained within the geometry of the London borough using the ST_Contains() function. It then groups the results by the name of the London borough and sorts them in alphabetical order. The resulting table (see figure 2) shows the number of tube stations for each London borough.

London Borough Name	Number of Tube Station
Barking and Dagenham	6
Barnet	12
Brent	22
Camden	29
City	27
Ealing	16
Enfield	4
Greenwich	5
Hackney	1
Hammersmith and Fulham	20
Haringey	6
Harrow	11
Havering	4
Hillingdon	21
Hounslow	10
Islington	13
Kensington and Chelsea	21
Lambeth	14
Lewisham	2
Merton	5
Newham	32
Redbridge	10
Richmond upon Thames	2
Southwark	7
Tower Hamlets	34
Waltham Forest	4
Wandsworth	6
Westminster	63

the London borough with the most tube stations is Westminster with 63 stations, while the one with the least number of stations are Greenwich, Enfield, Havering, and Lewisham, each with only 4 or fewer stations.





Task 6
Motorways across London Boroughs (Intersection)
Open Shapefile “motorway.shp”
Using spatial relation of “Intersect”
Join the other table
Find out Which motorways across which London boroughs.
Query Analysis
I used this code and will explain further the steps taken:
1.	‘SELECT mw.number AS Motorway, lua.name AS "London Borough Name", lio.in_out AS Region: This selects the columns that will be returned in the output. Specifically, it selects the motorway number, the name of the London borough that the motorway intersects with, and the region of the borough (i.e., whether it is located in inner or outer London).
2.	FROM motorway AS mw: This specifies the table from which the mw alias will be used. In this case, it is the motorway table, which presumably contains information about motorways in the UK.
3.	JOIN london_guinness_book AS lua ON (ST_Intersects(mw.geometry, lua.geometry)): This performs a spatial join between the motorway table and the london_guinness_book table. Specifically, it checks whether the geometry of a motorway intersects with the geometry of a London borough, and if so, it returns the corresponding rows from both tables.
4.	JOIN london_io AS lio ON (lua.in_out = lio.in_out): This performs another join, this time between the london_guinness_book table and the london_io table. It joins on the in_out column, which appears to be a region identifier for the boroughs.
5.	ORDER BY mw.number, lua.name: This orders the output by the motorway number and the London borough name.

Motorway	London Borough Name	Region
M1	Barnet	Outer
M1	Harrow	Outer
M11	Redbridge	Outer
M25	Bromley	Outer
M25	Enfield	Outer
M25	Havering	Outer
M25	Hillingdon	Outer
M4	Hillingdon	Outer
M4	Hounslow	Outer
Figure 3 – Table showing London boroughs and motorways which intersect that borough.
From looking at this table we can see the M1 motorway is seen in Barnet and Harrow which are both considered the outer region. The M11 interacts with Redbridge, this motorway only encounters one London borough. The M25 intersects the most London boroughs which is 4, these are Bromley, Enfield, Havering and Hillingdon. The M4 meets Hillingdon and Hounslow. This data suggests Hillingdon has the most encounters with Motorways in London as both M25 and M4 meet with this borough. 

Task 7 
Tube Stations near Motorways in London (Distance between Points and Lines)
(Need to load Shapefile “UndergroundStations.shp”)
Using spatial relation of “Distance”
Multiple Join and Left Join
Find out distance between Underground Stations and Motorways.
Query Analysis

1.	The first step is to load the Shapefile "UndergroundStations.shp" which contains the data for London's underground stations.
2.	The next step involves using spatial relations of "Distance" to find out the distance between underground stations and motorways in London.
3.	To achieve this, the code performs multiple joins and left joins between different tables to get the necessary information.
4.	Specifically, the code joins the "UndergroundStations" table with the "motorway" table on the spatial relationship of "Distance". This gives us a list of all underground stations that are within 1km of any motorway.
5.	To get the names of the London boroughs where these underground stations are located, the code performs a left join between the result obtained in step 4 and the "london_guinness_book" table on the spatial relationship of "contains".
6.	Finally, the code selects the relevant columns from the joined tables, including the underground station name, the name of the motorway, and the name of the London borough where the station is located, and orders the results accordingly.




Motorway	Station	Line	distance to motorway < 1km	distance to motorway < 2km	distance to motorway <= 3km
M1	Hendon Central Underground Station	Northern	1	0	0
M1	Colindale Underground Station	Northern	1	0	0
M11	Theydon Bois	Central Line	1	0	0
M11	Debden	Central Line	1	0	0
M11	Roding Valley	Central Line	1	0	0
M11	Chigwell	Central Line	1	0	0
M11	South Woodford	Central Line	1	0	0
M25	Terminal 5	Piccadilly	1	0	0
M4	Chiswick Park Underground Station	District	1	0	0
M4	Gunnersbury Underground Station	District	1	0	0
M4	Boston Manor Underground Station	Piccadilly	1	0	0
Figure 4 – Table showing underground stations in London and distance from motorway.
The table shows underground stations in London and the distance to motorways, which is categorised by distances of less than 1km, less than 2km, and less than or equal to 3km. This table shows that there are several underground stations in London located within a 1km distance of several motorways, with most of the stations being on the Central Line.



Task 8
Buffers of Motorways Create a table of motorway_buffers with Multipolygon geometry Using spatial function of “CastToMultipolygon” to create 1km buffer zones for Motorways Using spatial function of “CastToMultipolygon” to create 1km – 2.5km buffer rings for Motorways.









Figure 5 – Map showing Motorway in London with buffer zones – SpataLite


















Session 6 
Task 2 
Map of intersect 












 Task 3
 
Map of credit union with Voronoi polygons (with 10% buffer region), catchment areas can be created around each credit union. 
Task 4














Task 6
Updated table containing household size (HH) and population density (Popden)
 
 






Session 7
Task 1


Task 2
The map below shows a choropleth map which shows total counts of crime per 1000 persons in the London area. The inner London area shows a higher crime rate, with figures around 131 counts of crime per 1000 persons. When looking into the data even further, Westminster has the highest crime rate with a value of 247 counts per 1000 persons. The lowest area of crime is in the borough of Kingston Upon Thames with 64. This borough could have less crime as it has a lower population density than the inner boroughs of London. Or it could be that the people in this area are not reporting many crime incidents. 








Session 8
Task 1
The map below is a heatmap of GP in Haringey. The hotspots of GP surgeries are mainly seen in the centre of the borough. However, in the there is a high density of GP’s located in the southern & northern parts of the borough. This spatial pattern could reflect that these are the regions in which there is a higher density of people in comparison to the outer part of the borough. 














Map showing GP hotspot in Haringey Borough










Session 9
Task 1
From my findings, deprivation has a strong spatial dependence. A Moran index of 0.489 is seen in the graph below. Also, high levels of deprivation are seen in in the boroughs highlighted in red, whereas the lowest deprivation levels are seen in blue. 










Below is a graph of crime count per 1000 persons. The Moran index is at a moderate level of 0.220. Upon looking at the cluster map, it shows high levels of crime in Kensington & Chelsea, it also suggests that there is possibility of high crime rate in neighbouring boroughs. 
 





Task 2
REGRESSION
----------
SUMMARY OF OUTPUT: ORDINARY LEAST SQUARES ESTIMATION
Data set            :  london_life_polygon
Dependent Variable  :   LIFE_MALE  Number of Observations:   32
Mean dependent var  :     77.4125  Number of Variables   :    6
S.D. dependent var  :      1.8562  Degrees of Freedom    :   26 

R-squared           :    0.803673  F-statistic           :     21.2864
Adjusted R-squared  :    0.765918  Prob(F-statistic)     : 1.92573e-08
Sum squared residual:     21.6461  Log likelihood        :    -39.1514
Sigma-square        :    0.832541  Akaike info criterion :     90.3029
S.E. of regression  :    0.912437  Schwarz criterion     :     99.0973
Sigma-square ML     :    0.676439
S.E of regression ML:    0.822459

-----------------------------------------------------------------------------
       Variable      Coefficient      Std.Error    t-Statistic   Probability
-----------------------------------------------------------------------------
          CONSTANT       69.3676        3.95948        17.5194     0.00000
           P_SMOKE      0.101164      0.0802025        1.26135     0.21838
           P_BINGE     -0.361891       0.198456       -1.82353     0.07974
           P_OBESE     0.0444868      0.0876369       0.507626     0.61599
         P_HEALTHY       0.35326      0.0791595        4.46264     0.00014
            DEPRIV    -0.0399811      0.0302057       -1.32363     0.19715
-----------------------------------------------------------------------------

REGRESSION DIAGNOSTICS  
MULTICOLLINEARITY CONDITION NUMBER   70.463146
TEST ON NORMALITY OF ERRORS
TEST                  DF           VALUE             PROB
Jarque-Bera            2             1.3617          0.50619

DIAGNOSTICS FOR HETEROSKEDASTICITY  
RANDOM COEFFICIENTS
TEST                  DF           VALUE             PROB
Breusch-Pagan test     5             5.7773          0.32849
Koenker-Bassett test   5             6.3910          0.27001
============================== END OF REPORT ================================

