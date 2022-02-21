# 👯 Customer Segmentation: Project Overview 
* Unsupervised learning project to apply groupings to customer data 

## Table of Contents 
[Resources](#resources)<br>
[Data Collection](#DataCollection)<br>
[Data Pre-processing](#DataPre-processing)<br>
[Data Warehousing](#DataWarehousing)<br>
[Exploratory data analysis](#EDA)<br>
[Feature Engineering](#FeatEng)<br>
[ML/DL Model Building](#ModelBuild)<br>
[Deployment](#ModelDeploy)<br> 
[Project Management (Agile | Scrum)](#Prjmanage)<br>
[Project Evaluation](#PrjEval)<br>
[Looking Ahead](#Lookahead)<br>
[Questions | Contact me ](#Lookahead)<br>

<a name="Resources"></a>  

## Resources Used
**Python 3, PostgreSQL** 

[**Anaconda Packages:**](requirements.txt) **pandas numpy pandas_profiling ipywidgets sklearn matplotlib seaborn sqlalchemy kaggle psycopg2 ipykernel**<br><br>
Powershell command for installing anaconda packages used for this project    
```powershell
pip install pandas numpy pandas_profiling ipywidgets sklearn matplotlib seaborn sqlalchemy kaggle psycopg2 ipykernel
```

<a name="DataCollection"></a>  

## [Data Collection](Code/P13_Code.ipynb)
Powershell command for data import using kaggle API <br>
```powershell
!kaggle datasets download -d vjchoudhary7/customer-segmentation-tutorial-in-python -p ..\Data --unzip 
```
[Data source link](https://www.kaggle.com/vjchoudhary7/customer-segmentation-tutorial-in-python)
[Data](Data/Mall_Customers.csv)
*  Rows: 200 | Columns: 5
    *   CustomerID                
    *   Gender                    
    *   Age                       
    *   Annual Income (k$)        
    *   Spending Score (1-100)    

 

<a name="DataPre-processing"></a>  

## [Data Pre-processing](Code/joining_data.sql)
After I had all the data I needed, I needed to check it was ready for exploration and later modelling.   
*   General NULL and data validity checks  

<a name="DataWarehousing"></a>

## [Data Warehousing](Code/P6_Code.ipynb)
I warehouse all data in a Postgre database for later use and reference.

*   ETL in python to PostgreSQL Database.
*   Formatted column headers to SQL compatibility. 

<a name="EDA"></a>  

## [Exploratory data analysis](Code/P6_Code.ipynb) 
I looked at the distributions of the data and the value counts for the various categorical variables that would be fed into the model. Below are a few highlights from the analysis.

*   Age has a negative relationship with spending score, like because those in the data that are older spend less and those in the data that are younger spend more.
*   Annual income and spending score have almost no correlation.   

<img src="images/correlation.png" />

<a name="FeatEng"></a>  

## [Feature Engineering](Code/P6_Code.ipynb) 
I kept only 2 columns; annual_income_(k$) and spending_score_(1-100) as they showed no initial correlation. 

<img src="images/WSS_formula.png"/>

*   I applied the Within Sum of squares to find the inertia for the optimum number of clusters. Inertia is calculated as the sum of squared distances between data points and the centres of the clusters they belong to. Inertia quantifies the within-cluster variation.

<img src="images/elbow_point_graph.png"/>

*   The elbow graph was used to show the best number of clusters, here shown to be 6 for there is a gradual decline in WCSS. 

  

<a name="ModelBuild"></a> 

## [ML/DL Model Building](Code/P6_Code.ipynb)

I applies the KMeans algorithm with 5 clusters and predicted this on the initial input values (annual_income_(k$) and spending_score_(1-100)). 

```python
# Training the KMeans Clustering Model
kmeans = KMeans(n_clusters=5, init='k-means++', random_state=23)

# return a label for each data point based on their cluster
Y = kmeans.fit_predict(X)
print(Y)
```

<a name="ModelDeploy"></a> 

## Deployment
Seeing the clusters visually shows the clear subgroups within the customers dataset. 

<img src="images/images/customer_groups.png" />

<a name="Prjmanage"></a> 

## [Project Management (Agile | Scrum)](https://www.atlassian.com/software/jira)
* Resources used
    * Jira
    * Confluence
    * Trello 

<a name="PrjEval"></a> 

## [Project Evaluation]() 
*   WWW
    *   Completing and implementation of idea. 
    *   Mathematical understanding 
*   EBI 
    *   Larger dataset used 
    *   Search for better use case. i.e a more complex problem. 

<a name="Lookahead"></a> 

## Looking Ahead
*   What next
*   How can this be applied at scale? Research us in business and how to class customers based on several attributes

<a name="Questions"></a> 

## Questions | Contact me 
For questions, feedback, and contribution requests contact me
* ### [Click here to email me](mailto:theanalyticsolutions@gmail.com) 
* ### [See more projects here](https://github.com/MattithyahuData?tab=repositories)









