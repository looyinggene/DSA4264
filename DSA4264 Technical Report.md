# DSA4264 Technical Report

**Project: Problem 1 Parallel bus routes to MRT lines**  
**Members: 
Alicia Ong Ching Ern (A0129849H)
Aiko Liana Amran (A0240406U)
Loo Ying Gene (A0245638W)
Ramasamy Shreya (A0244253L)**  
Last updated on 6/11/2024

## Section 1: Context

This project was initiated in response to the ongoing expansion of Singapore's MRT system, which has introduced new lines such as the Downtown Line and [Thomson-East Coast Line (TEL)](https://landtransportguru.net/thomson-east-coast-line/). These additions are designed to enhance the efficiency and coverage of public transportation. However, with these expansions, many existing bus routes now parallel these MRT lines, resulting in reduced ridership for those bus services and potential redundancy. The Ministry of Transport (MOT), specifically the Land Division and its Public Transportation team, has tasked the Data Science Department with conducting a comprehensive analysis of these bus routes to identify those that overlap with MRT lines. The goal is to streamline bus services, improve resource allocation, and encourage commuters to shift to MRT services where possible.

The analysis aims to prioritise trunk bus routes that significantly overlap with MRT lines for possible adjustment, rerouting, or removal. By leveraging data science methodologies, the project will use a combination of geospatial analysis, passenger volume data, and travel time comparisons to assess route efficiency and recommend solutions. Given the constraints of working with public datasets due to IT issues, this project will focus on publicly available data sources to ensure a robust yet feasible analysis.


## Section 2: Scope

### 2.1 Problem

The Ministry of Transport (MOT) faces the ongoing challenge of optimizing bus services to align with the extensive network of MRT lines across Singapore. With the expansion of the MRT system, several existing bus routes now parallel these rapid transit services, leading to potentially redundant overlaps that reduce the efficiency of the public transportation system.

Continuing to operate parallel bus routes that duplicate MRT services not only leads to unnecessary operational expenditures but also restricts the potential reallocation of resources to enhance or introduce bus services where they are more critically needed. Addressing this inefficiency is crucial for maintaining a financially sustainable and strategically optimized public transportation network. Effective management of these overlaps is essential for freeing up funding, which could be redirected to meet emerging public transportation demands and improve service coverage in underserved areas.

The government allocates substantial funds to support public transportation. With the operating costs of public transport increasing by an average of [7% annually](https://www.channelnewsasia.com/singapore/public-transport-operating-costs-revenue-s-iswaran-2934876) due to factors such as energy prices and inflation, efficient use of these funds becomes crucial. Misallocation or inefficient use of these resources, such as maintaining redundant bus routes, could lead to substantial financial waste. Currently, the government spends SGD 2 billion yearly on subsidies alone​.

Without proper route management and optimization, operational costs of bus routes can escalate the financial burden on the public transport system. For example, the introduction of the [Bus Contracting Model](https://landtransportguru.net/bus/bus-contracting-model/) aimed to improve efficiency by increasing competition and service standards among bus operators. However, maintaining excessive or overlapping routes can dilute these gains by spreading resources too thinly over underused services.

Under the [Bus Contracting Model](https://landtransportguru.net/bus-contracting-model-faq/), service levels have been structured to ensure no more than 15-minute headways during peak periods for all buses, with even shorter intervals for feeder services. Inefficient route planning that does not consider MRT overlaps can lead to service redundancies, under-utilization of buses, and unnecessary environmental and traffic impacts, all of which undermine the goals of a streamlined and responsive public transport system

Data science offers a vital solution for analyzing and optimizing these transportation networks due to the voluminous and complex nature of transportation data. Traditional methods may fall short in the precision and speed required for analyzing extensive datasets covering all bus and MRT routes. Utilizing data science, stakeholders can quantitatively determine the degree of overlap between bus routes and MRT services. This analysis allows for the identification of routes that can be adjusted or streamlined, thereby improving the overall efficiency of public transport services.


### 2.2 Success Criteria

**Business Goals:**
1. **Cost Savings:** Successfully identifying and optimizing 2-3 redundant bus routes will reduce the operational costs of maintaining underutilized services. This could free up funds to support at least 3 new bus routes in areas of higher demand, helping to reallocate financial resources more effectively across Singapore’s public transportation network.
2. **Improved Public Transport Efficiency:** Encouraging commuters to switch from bus services to the more efficient MRT network by streamlining overlapping bus routes will reduce commute times and improve overall public satisfaction with the transportation system. This will also help align Singapore’s transport policies with broader sustainability goals by minimizing fuel consumption and emissions from redundant bus services.

**Operational Goals:**
1. **Reduction in Route Redundancy:** By using data science to identify overlapping bus and MRT routes, the project aims to reduce the number of bus routes that serve the same areas as MRT lines. Success would be measured by a decrease in the number of parallel services, leading to more efficient bus route planning and resource allocation.
2. **Passenger Volume Optimization:** Through the reconfiguration of bus services, the goal is to optimize passenger volumes per bus route. A successful outcome would involve increasing the utilization rates of bus routes that are retained after the optimization, ensuring buses serve areas with higher demand rather than duplicating services provided by MRT lines.


### 2.3 Assumptions

**Data Availability:** The project assumes that the public datasets used (passenger volumes, bus and train routes, travel times) are representative of the actual ridership and operational data, despite the limitation of not having access to internal datasets.

**Commuter Behaviour:** Another assumption is that commuters will generally prefer the MRT over bus routes when travel time and convenience are comparable. Thus, reducing bus services that overlap with MRT routes will not lead to significant commuter dissatisfaction, provided that alternative routes or transportation modes are available.

**Feasibility of Implementation:** It is assumed that the LTA has the operational flexibility to adjust or remove bus routes based on the project's recommendations, including making necessary public announcements and obtaining feedback from affected communities before changes are enacted.


## Section 3: Methodology

### 3.1 Technical Assumptions

Our project is structured into three main phases. Phase 1 is centred around identifying bus services that run parallel to MRT lines for significant portions of their routes. Phase 2 builds on this by incorporating ridership data and additional features to select 2-3 bus routes that may require modifications or potential removal. Phase 3 involves proposing changes to the selected routes identified in the previous phases.

The overarching hypothesis driving this analysis is that bus services with high parallelism to MRT lines, especially when combined with a drop in ridership, may reveal inefficiencies in public transportation. Such findings could suggest redundancies where adjustments to these bus services might encourage more commuters to switch to MRT usage, thus optimising public transportation coverage.

**Phase 1: Identification of Parallel Routes**

The primary hypothesis for Phase 1 is that if a bus service route runs parallel to an MRT line, the routes should remain within a defined proximity and the geometric angle difference between them should not exceed a certain threshold. The extent to which a bus service route runs parallel to an MRT line is the focus of this phase.

The concept of parallelism is quantified using a custom metric termed the Weighted Parallel Score. This score is calculated as the composite score of parallelism against different MRT lines. 

The parallel score of bus service to individual train line is calculated as below:

$$
\text{Parallel Score} = 
\frac{
\begin{array}{c}
\text{Total number of distinct parallel bus stops} + \\
(\text{Longest consecutive parallel length} \times \text{Consecutive Weight})
\end{array}
}{
\text{Total number of bus stops}
}
$$


Since we are emphasising on the bus routes that run parallel consecutively to any train line for a large extent of the bus route, we proceed to calculate the weighted parallel score by weighing the parallel score across all train lines.

We define 
![Screenshot 2024-11-06 at 16.39.16](https://hackmd.io/_uploads/B1xdji_Wyg.png)



In the case where a bus service runs in two directions, the mean of composite parallel score will be taken.

**Phase 2: Clustering of Bus Services**

Phase 2 involves clustering the bus service routes based on three main features:

1. The Weighted Parallel Score derived from Phase 1. 

2. The Average Distance within 1km from the bus stops to nearby train stations for each bus service. This feature indicates the degree of accessibility and proximity of the bus stop to the train station.

3. The change in passenger volume from July to September 2024, defined as the total number of tap-ins and tap-outs across all bus stops within a bus service. The change in passenger volume is calculated as the difference between the passenger volume in September and in July.

Due to data limitations, we cannot isolate the change in tap-in and tap-out volume attributed solely to a single bus service. Nonetheless, the overall change in passenger volume serves as an indicator of ridership trends, with only the past three months of data available for analysis.

**Phase 3: Proposing Alternative Routes**

Phase 3 focuses on exploring alternative routes for the bus services identified in Phase 2 as requiring potential modification or removal. The analysis in this phase is geared towards assessing the impact of proposed changes on the public and identifying the most viable route adjustments.

The analysis is conducted using local computational resources, specifically a system with a Ryzen 5 5600X CPU, 32GB RAM, and an Nvidia GTX 1660 Super GPU. The data is sourced from the Land Transport Authority (LTA) Datamall, ensuring the reliability of the data used throughout the project.


### 3.2 Data

The datasets used in this project were sourced from reliable government and public APIs, such as LTA DataMall and external source from [SGBusRouter Github repository](https://github.com/cheeaun/sgbusdata). The datasets can be accessed under /datasets folder

**Overview of datasets**
1. The datasets from LTA Datamall used in our project are as below:
Dynamic Datasets:(Provide some descriptions)
*2.2. Bus Services
2.3 Bus Routes
2.4 Bus Stops
2.5 Passenger Volume by bus stops : Saved under /datasets/pv_bus_stops
2.22 Geospatial Whole Island : Saved under /datasets/geospatial_layer*
    * Train Stations
    * Bus Stops

    Static Datasets:
    *Train Station Codes and Chinese names: Saved under /datasets/*

2. Dataset retrieved from [SGBusRouter Github repository](https://github.com/cheeaun/sgbusdata) : 
routes.min.geojson : Saved under /datasets/routes/

**Data Collection**
The data used for this project was obtained from the Land Transport Authority (LTA) DataMall API, which provides comprehensive information about bus routes, bus stops, and passenger volume. The API key can be registered on the LTA website and should be stored in an .env file. The API scripts used for data collection are stored in the /src directory and include the following Jupyter notebooks:
* **get_bus_info_function.ipynb:** Script to retrieve detailed information on bus services, routes, and stops.
* **get_geospatial_function.ipynb:** Script used for obtaining and processing geospatial data.
* **get_passenger_volume.ipynb:** Script to fetch and process passenger volume data from LTA.

**Data Cleaning** 
The datasets primarily contained categorical and geospatial data. Initial checks confirmed data consistency across multiple datasets, including bus and MRT route data.

**Geospatial Merging:** We merged datasets to include geospatial information, integrating bus routes with bus stop coordinates and MRT station locations to create a comprehensive geospatial representation of routes.

**Coordinate System Projection:** The coordinates for both bus stops and MRT stations were projected to the EPSG 4326 coordinate system (WGS 84) to standardise all geospatial calculations and ensure compatibility during mapping and distance calculations.

**Duplicates and Missing Values:** The data was checked for duplicates and missing values. No duplicates were found, but missing values were found in passenger volume by bus stops. We filtered for common bus stops that were populated with passenger volume data for all 3 months, excluding bus stops where values were missing for at least 1 month.

**Outlier Treatment:** The nature of the data did not require extensive outlier treatment, as the analysis focused on spatial relationships and route alignment.

**Feature Engineering**
To conduct a comprehensive spatial analysis, various feature engineering and transformation steps were performed to create attributes necessary for evaluating the parallelism between bus routes and MRT lines:

**Geometric Representation:**
* The coordinates of bus stops and MRT stations were transformed into geometry objects to enable essential spatial operations such as calculating distances , angle difference and performing proximity checks in phase 1.
* Bus routes and train lines were segmented into LineString objects by connecting consecutive bus stops and train stations. This transformation provided a geometric representation of entire routes, facilitating the assessment of parallelism between bus services and MRT lines in Phase 1 of the project.

**Trend Detection:**
To assess passenger volume trends, each service was labelled as "Decreasing" only if it showed a decline over three consecutive months (July, August, September); otherwise, it was labelled as "Increasing." This rule-based approach ensures that only clear, sustained decreases are flagged, providing a reliable basis for prioritising routes in Phase 2. By focusing exclusively on consecutive monthly declines, we account for potential fluctuations due to external factors and prioritise trends that indicate a more consistent downward pattern.

**Standardisation:**
For fair comparison across bus routes, the Change metric was normalised using the TotalStops feature, yielding per-stop values. The Change values were then standardised using z-scores, which measured each route’s change relative to the dataset's mean and standard deviation. To add context, the standardised values were adjusted for signs based on the trend, giving positive values for increases and negative values for decreases, thereby aligning with the observed trend direction.

$$
\text{Trend} = 
\begin{cases} 
\text{"Decreasing"} & \text{if } \text{Volume}_{\text{July}} > \text{Volume}_{\text{August}} > \text{Volume}_{\text{September}} \\
\text{"Increasing"} & \text{otherwise}
\end{cases}
$$

$$
\text{Normalized Change} = \frac{\text{Change in Volume}}{\text{Total Stops}}
$$

$$
\text{Standardized Change} = \frac{\text{Normalized Change} - \text{Mean (Normalized Change)}}{\text{Standard Deviation (Normalized Change)}}
$$ 

$$
\text{Adjusted Standardized Change} = 
\begin{cases} 
-\left| \text{Standardized Change} \right| & \text{if Trend is "Decreasing"} \\
\left| \text{Standardized Change} \right| & \text{if Trend is "Increasing"}
\end{cases}
$$


**Data Normalisation:**
To prepare for clustering analysis in Phase 2, data normalisation was performed. Features such as average distances to the nearest MRT station and changes in passenger volume were normalised using Min-Max scaling. This transformation scaled all features to a consistent range between 0 and 1, ensuring that each feature contributed proportionally to the clustering process and enabling fair comparisons between features with different units of measurement.

### 3.3 Experimental Design

**Phase 1: Identification of Parallel Routes**
In addressing the problem of identifying bus routes that run parallel to MRT lines, a geospatial approach was chosen due to its clarity, simplicity, and suitability for accurately capturing the spatial relationships between routes. This method is comprehensible and straightforward, allowing for easy interpretation and validation of results.

**General approach**
The core algorithm of Phase 1 involves segmenting bus routes and determining their alignment with MRT routes. Each bus route is divided into segments formed by consecutive bus stops. These segments are represented as LineStrings, a geometric representation that allows for precise spatial analysis. The process for evaluating parallelism between bus segments and MRT lines is detailed below:

**Geodesic Distance Calculation** 
![3.3_geodesic distance line](https://hackmd.io/_uploads/S1e8Vt_-kg.png)

For each bus segment, the algorithm calculates the geodesic distance from the midpoint of the segment to the nearest point on an MRT line segment. This midpoint is used as it represents the central point of the bus segment, ensuring an unbiased and consistent distance measurement. The nearest point on the MRT line segment is identified using geospatial operations, which project the bus segment onto the MRT route to find the closest corresponding point. The distance is calculated using geodesic measurements to ensure real-world accuracy.

**Angle Difference Calculation**
To assess the alignment of the bus segment with the MRT line, the angle difference between the vectors formed by the bus segment and a corresponding MRT segment is computed. To do this, the algorithm creates a line vector from the nearest MRT point by projecting 0.01 units forward along the segment. The dot product formula is then applied to calculate the angle difference between the two vectors:

$$
\cos(\theta) = \frac{\vec{A} \cdot \vec{B}}{|\vec{A}| |\vec{B}|}
$$

where theta is the angle difference, vec{A} and vec{B} are the vectors representing the bus segment and the MRT segment, respectively. are the vectors representing the bus segment and the MRT segment, respectively.

 A threshold of 25 degrees was selected to define parallelism, as angles smaller than this value ensure that the routes align in the same general direction. This criterion effectively differentiates routes that genuinely run parallel thus minimising false positives from intersecting or non-parallel routes.
 
**Defining Parallel Segments** 
A bus segment is considered parallel to an MRT line if the geodesic distance from the midpoint to the MRT line is within 350 metres and the angle difference is 25 degrees or less. The 350-metre distance threshold is a conservative estimate that accounts for the practical walking distance between a bus stop and an MRT station, ensuring that the routes are within reasonable proximity for passenger accessibility.

**Incorporating Consecutive Weights:**
To emphasise the importance of longer uninterrupted sequences of parallel segments, a consecutive weight multiplier of 1.2 was applied. This multiplier ensures that routes maintaining parallel alignment over extended segments are rewarded, aligning with the objective of identifying bus services that closely mimic MRT routes. 
![consecutive_sensity](https://hackmd.io/_uploads/Sk42B5_WJe.png)

Sensitivity analysis was conducted across a range of consecutive weights, confirming that a multiplier of 1.2 balanced the recognition of long parallel sequences without excessively amplifying their importance. This analysis demonstrated that the ranking of the bus routes remained relatively stable across different weight values, highlighting the robustness of the approach and its resilience to minor parameter changes.

The combination of Alpha and Beta at 0.55 and 0.45 in Weighted Parallel Score, respectively, strikes a balance between valuing significant parallel alignment with one MRT line while still accounting for cumulative parallelism with other lines. This approach helps avoid over-emphasizing smaller, fragmented alignments that may not strongly indicate redundancy, while ensuring that meaningful, long stretches of parallelism are prioritized in the score.

**Validation of Results** 
To validate the algorithm, the top three and bottom three bus routes identified as parallel were visually inspected and cross-checking with third-party mapping tools like BusRouter SG. This validation step confirmed that the routes flagged by the algorithm accurately reflected real-world parallel alignments with MRT lines. Comparative analysis against these external mapping resources provided confidence in our method's reliability and demonstrated its capability to identify potential candidates for service modification accurately on a high level.

**Phase 2: Clustering**

**Rationale for Clustering**
Clustering analysis was selected as the primary approach for Phase 2 due to the nature of the dataset and the analytical objectives. Given the relatively small dataset size and the limited number of features, clustering enables the identification of patterns and relationships within the data that may not be immediately apparent through other methods. The goal is to group bus services into clusters based on shared characteristics, allowing for a clearer assessment of which routes may be redundant or suitable for partial modifications. Clustering also supports exploratory analysis by highlighting potential group structures that align with the features defined in Section 3.1 (Technical Assumptions). We performed clustering on top 20 and top 50 bus services.

To ensure a robust analysis, two clustering algorithms were employed:
1. **K-Means Clustering (KNN)**
K-Means clustering is widely used for its simplicity and effectiveness in partitioning data into k clusters. It relies on the concept of centroids, which represent the mean of data points within each cluster. This method is computationally efficient and well-suited for our datasets, as it can quickly adapt to changes and scales well with continuous, normalised features.
2. **Hierarchical Clustering (HC)**
Hierarchical clustering was used as a comparative method to K-Means to examine how the dataset behaves under different clustering strategies. HC builds a hierarchy of clusters using a dendrogram, which visually represents the arrangement of data points and the order in which they are merged. This method is particularly valuable for understanding nested cluster relationships and assessing whether alternative structures emerge.

**Hyper-parameter Tuning**
To identify the optimal number of clusters and the type of linkage to use for hierarchical clustering, the following methods were applied:
1. **Elbow Method:**
For K-Means, this method was used to visually estimate the optimal number of clusters (k). The elbow point represented the most efficient balance between explained variance and simplicity.
The diagrams below show the application of elbow methods to determine the optimal number of clusters. A clear elbow indicates that additional clusters contribute minimally to reducing WCSS, suggesting that the optimal k has been reached.

![20_knn_optimal_K](https://hackmd.io/_uploads/SyoqEY_bJg.png)
<center>Diagram 1: Elbow method for top 50 bus services</center>
<br>

![50_KNN_optimal_K](https://hackmd.io/_uploads/Skn54FO-Jl.png)
<center>Diagram 2: Elbow method for top 20 bus services</center>

2. **Silhouette Score:**
The Silhouette Score quantifies the consistency within clusters by measuring how similar each point is to its assigned cluster compared to other clusters..The Silhouette Score was used alongside the Elbow Method to determine the optimal number of clusters and type of linkage to be used in Hierarchical clustering. The Silhouette Score  provided a more comprehensive assessment of cluster cohesion and separation.
The diagrams below show the application of Silhouette Score to determine the type of linkage and optimal number of clusters

![hc_20](https://hackmd.io/_uploads/HkNUwYOWyl.png)
<center>Diagram 3: Silhouette scores of top 20 bus services</center>
<br>

![hc_50](https://hackmd.io/_uploads/H1VIDF_bye.png)
<center>Diagram 4: Silhouette Scores of top 50 bus services</center>

A comparative table of Silhouette Scores for various parameters chosen are as below:


| KNN Clustering | Number of k |Silhouette Score |
 | -------- | -------- |-------- |
| Top 20 Bus Services     | 3    |0.335    |
| Top 50 Bus Services    | 7    |0.338    |

| Hierarchical Clustering | Number of k | Type of linkage | Silhouette Score |
|-------------------------|-------------|-----------------|------------------|
| Top 20 Bus Services     | 5        | Ward         | 0.28          |
| Top 50 Bus Services     | 5         | Complete         | 0.38          |


**Evaluation Metrics**
Silhouette Score was computed for each chosen cluster configuration to ensure the identified clusters were well-formed and meaningful.

Visual analysis is performed to evaluate our model. Dendrograms and scatterplots were generated to visualise the clustering results for both K-Means and hierarchical clustering. These plots helped confirm that clusters were logically distributed and aligned with domain expectations. Visual comparisons of top 20 and top 50 bus services were conducted to evaluate how different dataset sizes affected the clustering outcomes.

## Section 4: Findings

### 4.1 Results

**Phase 1:** 

![Screenshot 2024-11-06 at 13.57.05](https://hackmd.io/_uploads/Skc0vY_Zkx.png)
<center>Diagram 5: Top 20 Pre Filter </center>
<br>

![Screenshot 2024-11-06 at 13.56.52](https://hackmd.io/_uploads/By1RDt_bJe.png)
<center>Diagram 6: Top 20 Post Filter </center>

**Filter for bus without suffixes**
We focus on bus services without alphabetic suffixes because they represent longer trunk routes that align more closely with our optimization objectives. These routes typically serve major corridors and provide essential connectivity across broader areas. In contrast, services with suffixes like "C," "A," or "B" are Short-Trip Services or route variants that cover specific or localized areas, which are not relevant for this analysis. Express services marked with "E" or "e" are designed to provide faster commute times but serve more niche markets, further differentiating them from the longer trunk services we aim to optimize. By excluding these services, we can focus on improving the efficiency of key transit routes.

**Validation and Accuracy**
To further validate the accuracy of our analysis, we used overlaying graphs of bus and MRT lines in addition to utilizing BusRouter. These graphs allowed us to visually inspect the spatial alignment between the bus routes and MRT lines, providing a clearer understanding of the overlap between the two transportation networks.

By plotting the bus routes alongside the MRT lines, we were able to directly compare the geographical coverage of each bus service with the proximity of MRT stations. The overlay graphs highlighted areas where bus routes intersected or ran parallel to MRT lines, confirming that services with high parallel scores indeed had substantial overlap with MRT routes. This visual representation provided an intuitive way to verify that bus routes identified as redundant (with high parallel scores) were correctly classified as such, with their routes largely paralleling MRT lines.

BusRouter served as a secondary validation method, supplementing the quantitative analysis with a visual inspection that further corroborated the accuracy of our clustering results. This helped ensure that the recommended changes—whether cancellations, modifications, or frequency adjustments—were based on an accurate assessment of the routes' geographic and operational relevance. The consistency between the data-driven clustering analysis and the overlaying visualizations gave us confidence that our proposed recommendations would lead to a more efficient and effective public transportation system.

![bottom_90](https://hackmd.io/_uploads/BJp6tYObJg.png)
<center>Diagram 7: Bottom-ranked Bus Service 90 </center>
<br>

![bottom_405](https://hackmd.io/_uploads/Byh15KdWJx.png)
<center>Diagram 8: Bottom-ranked Bus Service 405 </center>
<br>

![bus_48](https://hackmd.io/_uploads/r1pTYKd-Je.png)
<center>Diagram 9: Top-ranked Bus Service 48 </center>
<br>

![bus_67](https://hackmd.io/_uploads/SkT6tY_Wkg.png)
<center>Diagram 10: Top-ranked Bus Service 60 </center>

**Phase 2 :**
The diagrams below are scatterplots generated from KNN Clustering.
<table>
  <tr>
    <td style="text-align: center;">
      <img src="https://hackmd.io/_uploads/r15OqYub1x.png" alt="20_knn_clusters_2D" width="300"/>
      <p>2D Scatterplots for top 20 bus services</p>
    </td>
    <td style="text-align: center;">
      <img src="https://hackmd.io/_uploads/BkcO9tdZyl.png" alt="2-_knn_clusters_3D" width="300"/>
      <p>3D Scatterplots for top 20 bus services</p>
    </td>
  </tr>
  <tr>
    <td style="text-align: center;">
      <img src="https://hackmd.io/_uploads/ry40qtO-yl.png" alt="50_knn_clusters_2D" width="300"/>
      <p>2D Scatterplots for top 50 bus services</p>
    </td>
    <td style="text-align: center;">
      <img src="https://hackmd.io/_uploads/ry4C5t_Zyx.png" alt="50_knn_clusters_3D" width="300"/>
      <p>3D Scatterplots for top 50 bus services</p>
    </td>
  </tr>
</table>
<center>Diagram 11: Scaterplots for Clustering </center>


The diagrams below are dendograms generated from hierarchical clustering.
![20_dendogram](https://hackmd.io/_uploads/r12Vjt_-yx.png)
<center>Diagram 12: Dendogram for top 20 bus services </center>
<br>

![50_dendogram](https://hackmd.io/_uploads/HknNjF_-Jl.png)
<center>Diagram 13: Dendogram for top 50 bus services </center>
<br>

**Interpretation of clusters**
The clustering analysis of bus routes, conducted using both K-Nearest Neighbors (KNN) and Hierarchical Clustering (HC) on top 20 and top 50 bus services, revealed three primary clusters. These clusters display consistent groupings between KNN and HC, with each cluster representing distinct characteristics in passenger volume trends and proximity to MRT stations. Below is a brief interpretation of each identified cluster:

1. **Cluster 0:**

This cluster primarily includes bus routes that exhibit inconsistent trends in passenger volume. Specifically, while there is a general decrease in passenger volume over the three months, the pattern is not uniform across all routes within this cluster. Some routes show signs of stabilization or even slight growth in certain months, indicating that factors other than the availability of MRT services may influence commuter behavior. These routes could be subject to dynamic changes based on demand fluctuations and should be further analyzed for targeted interventions.

2. **Cluster 1:**

Routes in this cluster display characteristics that do not align as clearly with the patterns observed in the other two clusters. This group includes bus routes that either have a moderate level of overlap with MRT stations or show more stable passenger volume trends. These routes may require targeted modifications, such as adjusting intervals or improving service in low-density areas, to better serve commuter needs. They represent routes that are neither highly redundant nor in drastic decline, suggesting potential for optimization without complete removal.

3. **Cluster 2:**

Routes within this cluster have high Weighted Parallel Scores, meaning they significantly overlap with MRT routes. Additionally, these routes have experienced the most substantial declines in passenger volume over the three months. The high redundancy between these bus services and nearby MRT stations, combined with decreasing demand, signals a strong case for route reduction or modification. These routes are prime candidates for phase-out or frequency adjustments, as passengers can likely switch to MRT services without disrupting their commute.

These findings provide an essential foundation for the subsequent recommendations on route cancellations, modifications, and adjustments to service intervals. 

### 4.2 Discussion

**Proposed Recommendations**

![6149924875159061488](https://hackmd.io/_uploads/rJI1QFOZ1l.jpg)
<center>Diagram 14: Decision Making Framework </center>
<br>

Due to the small size of the dataset (with only 20 or 50 rows), the clustering results were scattered, leading to a low silhouette score. This made it challenging to derive clear, actionable insights purely from the clustering analysis. To address this, we implemented a decision-making framework, which involved a combination of additional factors such as presence of alternative bus and train services as well as more in-depth manual analysis of the key metrics—such as passenger volume trends and proximity to MRT stations—that were already considered in the clustering. Based on the deeper analysis, we identified three types of recommendations: route cancellations, modifications, and adjustments to bus intervals, each tailored to the service's redundancy and passenger trends. By cross-referencing the clustering results with BusRouter visualizations and overlay graphs, we validated our findings, ensuring that the routes identified for modification, cancellation, or frequency adjustment were both supported by data and geographically relevant.

1. **Cancellation of route:**
![Bus67](https://hackmd.io/_uploads/ryG0yiOWJg.jpg)
<center>Diagram 15: Bus 67 Route </center>
<br>

For Service No. 67, a comprehensive analysis supports the decision to phase out this route gradually due to significant redundancy with MRT services and declining demand. Bus 67 holds the highest parallel score among the analyzed routes, with 86.2% of its path overlapping with MRT lines, primarily the Downtown Line and East West Line. Additionally, there has been a substantial decrease in passenger volume between July and September 2024, totaling approximately 2.87 million in combined tap-in and tap-out volumes. This steep decline indicates a marked shift away from this bus service, likely due to the convenience and speed of nearby MRT alternatives.

Bus stops served by Service 67 are in close proximity to MRT stations. With this high degree of overlap, passengers already have access to efficient alternative services, including the Downtown Line and East West Line trains, which mirror much of Service 67’s route. Moreover, Bukit Panjang is well served by alternative bus routes (such as 188, 974, 974A, and 976) that connect passengers to key locations. Given the availability of these comprehensive public transit options, passengers can conveniently transition to MRT or other bus services without compromising accessibility.

The phased removal of Service 67 allows for a smoother transition, aligning with travel patterns that show increasing reliance on MRT services, particularly during off-peak hours. This also minimizes the risk of alienating commuters who may still rely on the service, particularly during weekends or holidays where train services may start slightly later. Moreover, train travel on the overlapping routes can reduce travel time by approximately 30 minutes, offering a more efficient solution for passengers.

To ensure a smooth transition, it is proposed to phase out Service 67 by initially increasing the intervals between buses and monitoring public feedback. This approach will allow adjustments to be made based on commuter needs and sentiments, particularly for those who may still rely on this service on weekends or holidays.

2. **Modification to route:**
![Bus111](https://hackmd.io/_uploads/HyOb7sdW1x.jpg)
<center>Diagram 16: Bus 111 Route with Overlap with Bus 106</center>
<br>

In the discussion of Service No. 111, several key factors justify the decision to modify rather than remove or maintain the route unchanged. Service 111 currently runs between Ghim Moh Ter and The Float @ Marina Bay, with a notable degree of route overlap with MRT services. In addition, a marked decline in passenger volume was observed over the three-month period from July to September 2024, with a combined decrease in total tap-in and tap-out volumes of around 45,800. This trend suggests that the demand for this service may be waning, particularly in areas where alternative MRT options are available.

Further supporting this modification, the bus stops served by Service 111 are within close proximity to MRT stations. This high percentage reinforces the idea that passengers could easily switch to MRT services for comparable routes. Additionally, a significant section of Service 111’s route is identical to that of Service 106, particularly between Orchard Road and Marina Centre, resulting in direct duplication that reduces operational efficiency. Given this redundancy, we propose to adjust Service 111 by having it loop back at Orchard Road instead of continuing to Marina Bay, which is already well-served by other public transport options.

It is also important to recognize the unique role that Service 111 plays for certain neighbourhoods. The route primarily serves residential areas around Ghim Moh, Commonwealth, and Queenstown, providing them with convenient access to destinations like Orchard Road and Marina Centre. Moreover, Service 111 remains one of only two routes serving the low-density developments along Tanglin Road. For this reason, a complete removal of Service 111 is not feasible, as it would disrupt access to these areas. Instead, modifying the route allows for a balance between operational efficiency and maintaining essential service coverage for lower-density residential zones.

3. **Increase time interval:**
![Bus48](https://hackmd.io/_uploads/S1xrXoOWyx.jpg)
<center>Diagram 17: Bus 48 Route </center>
<br>

We propose increasing the time frequency for Bus 48 due to its relatively high parallel score of 66.0%, with a significant overlap with the Thomson-East Coast Line. However, despite this overlap, the decline in total tap-in and tap-out volumes between July and September 2024 is not as drastic as other services, with a decrease of approximately 548,824. Furthermore, the bus stops served by Bus 48 are in close proximity to MRT stations, indicating that passengers have alternative transit options nearby.

Bus 48 serves a critical role by connecting key areas such as Simpang Bedok, Bedok South, Marine Parade, Bukit Timah, Farrer, and Holland Village with the city areas of Bugis, Rochor Canal, Little India, and Newton Circus. The route also includes an express sector along the East Coast Parkway (ECP) between Rochor and Tanjong Katong. As a popular link between Marine Parade and Little India with limited alternative routes, much of its demand is concentrated along this corridor.

The route is particularly important for residents in the Simpang Bedok area, which currently lacks train coverage. Additionally, the extension of Bus 48 from Upper East Coast Ter to Bedok North Depot in 2011 has improved accessibility for residents in Bedok South Avenue 3 and Simpang Bedok. Given its importance to these areas, completely cancelling the route is not feasible.

Since modifying the route would undermine the express nature of its service, increasing the time frequency offers a balanced solution. This would help manage demand while maintaining service for areas that lack train coverage, ensuring that Bus 48 continues to serve its critical role efficiently.

**Discussion of Public Sector Impact and Technical Considerations**

The results of this project provide the Land Division and its Public Transportation team with a data-driven framework that enhances public transit efficiency by optimizing bus routes with significant overlap with MRT lines. The derived metrics—such as parallel score, changes in passenger volume, and proximity to MRT stations—directly translate into potential cost savings and better resource allocation. By focusing on routes with high redundancy to MRT lines and decreasing passenger volumes, resources can be reallocated or optimized to better meet actual passenger demand. This can lead to reduced operational costs for underutilized routes and a more efficient overall network. For the Ministry of Transport (MOT), this approach proactively addresses ridership trends and offers a sustainable transit solution that adapts to evolving commuter behaviors.

1. **Interpretability**
A key strength of this analysis is its interpretability. Metrics such as the percentage of overlap with MRT lines and monthly changes in passenger volume are easy for decision-makers to understand. These metrics help assess which routes are redundant and justify the proposed modifications or cancellations. This transparency in the decision-making process fosters greater understanding and support for the changes. Clear insights into the rationale behind each recommendation are essential for gaining stakeholder buy-in and building public trust, especially when implementing changes that may disrupt established routes.

2. **Fairness**  
Ensuring fairness to all affected commuters is crucial when modifying or canceling routes. This project prioritizes minimizing negative impacts, particularly for neighborhoods with low-density development, by ensuring continued access through alternative options. Proposals such as gradually phasing out Bus 67 include public feedback mechanisms, allowing adjustments to be made based on commuter responses. This approach ensures that the changes are equitable, offering a balanced solution that serves both the needs of areas with limited alternatives and those with better access to MRT lines.

3. **Deployability**
Deploying the proposed route changes will require a phased approach to reduce service disruptions, with a strong focus on public communication and feedback. Potential challenges, such as public resistance to changes in well-established routes, can be mitigated through targeted awareness campaigns and a flexible implementation timeline. A gradual reduction in frequency, followed by monitored route cancellations, ensures a smoother transition and allows for real-time adjustments based on public response. This approach makes the deployment more manageable and adaptable to unforeseen challenges. Additionally, the methodology and metrics developed in this project are reusable, providing a solid foundation for future optimizations as transit demands evolve.

**Addressing the Problem**  
Our analysis directly addresses the problem by identifying misalignments in resource allocation and service overlaps with MRT infrastructure. By optimizing routes based on actual commuter demand, we expect to see reductions in operational costs and improvements in resource efficiency. This targeted approach results in a more sustainable, cost-effective, and responsive transportation network, meeting the changing needs of commuters while improving service quality.


### 4.3 Recommendations

Based on the results and the methodology applied in this project, we propose the following next steps to enhance the efficiency of the public transportation system and maximize the impact of the developed model. These recommendations focus on both deployment and future improvements, addressing both the business value and technical considerations involved.

1. **Deployment of Optimized Routes for Pilot Implementation**
* Recommendation: Begin a pilot deployment of the optimized bus routes, focusing on the top 20 identified routes that exhibit significant overlap with MRT lines and are part of clusters with declining passenger volumes.
* Rationale: This will allow real-world testing and feedback from commuters, providing an opportunity to refine the decision-making framework before wider implementation. The pilot can help assess the feasibility of reduced bus frequencies, cancellations, or rerouting, ensuring minimal disruption while maximizing the potential for resource optimization.
* IT and Business Value: Deploying these changes on a smaller scale will allow for careful monitoring of passenger behavior and operational costs. It helps to mitigate potential resistance to changes while validating the model’s predictions. If successful, the findings can be scaled across other routes in the network, leading to long-term cost savings and more efficient resource allocation.

2. **Ongoing Monitoring and Model Updates**
* Recommendation: Set up a system for continuous monitoring of the pilot routes, including passenger volume, operational costs, and user satisfaction, to track the real-world performance of the optimized routes.
* Rationale: As commuter patterns can change over time, it’s important to periodically update the model with new data to maintain its relevance and accuracy. By collecting real-time feedback from both passengers and the operational side of the transit network, the model can be fine-tuned and recalibrated to reflect the latest trends in passenger demand and route usage.
* IT and Business Value: Continuous monitoring ensures that any adjustments can be made in a timely manner, preventing underutilization or overburdening of bus routes. This will help avoid the scenario of resource misallocation in the future and further improve the system's cost-efficiency.

3. **Enhancement of Data Quality and Availability**
* Recommendation: Invest in improving data quality and availability, particularly for passenger volume at various times of day and across different seasons, to refine the model’s predictions.
* Rationale: In some cases, the model was limited by the availability of high-quality data on passenger flows. Additional data sources, such as more granular ridership data (e.g., real-time occupancy data or ticketing system data), can improve the model’s accuracy. Similarly, integrating more detailed geospatial data on urban developments and upcoming infrastructure projects will ensure that the model reflects the most current conditions.
* IT and Business Value: Enhanced data quality and coverage will improve the robustness of the model, reducing the risk of inaccurate predictions. This would increase the likelihood of achieving optimal cost reductions while maintaining or improving service levels for commuters. Further, better data enables more precise decision-making and long-term planning.

4. **Exploring Alternative Methods for Clustering and Decision-Making**
* Recommendation: Experiment with alternative clustering techniques and decision-making frameworks to assess whether a more granular approach could yield additional insights.
* Rationale: While the current methodology—clustering based on overlap score, passenger volume change, and distance to MRT stations—has proven effective, there may be value in exploring other factors, such as temporal trends in passenger demand or service performance metrics (e.g., on-time performance or bus congestion). 
* IT and Business Value: Exploring alternative methods could further refine the model’s ability to recommend route changes. A more tailored solution could address specific commuter needs or service gaps, contributing to an even more efficient transportation network. In addition, it could uncover previously overlooked opportunities for optimization.

**Conclusion**
The next steps for this project revolve around the careful deployment of optimized routes, ongoing monitoring and updates to ensure accuracy, and improvements to the data infrastructure for better model performance. By scaling the model, experimenting with alternative techniques, and expanding its application, the public transportation system can achieve significant cost reductions while improving service efficiency for commuters. The recommendations here aim to balance both technical improvements and the real-world challenges of public transit, ensuring that the solutions are sustainable and impactful.
