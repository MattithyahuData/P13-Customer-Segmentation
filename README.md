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
[Project Management (Agile/Scrum/Kanban)](#Prjmanage)<br>
[Project Evaluation](#PrjEval)<br>
[Looking Ahead](#Lookahead)<br>
[Questions & Contact me](#Lookahead)<br>

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
*  Rows: 200 / Columns: 5
    *   CustomerID                
    *   Gender                    
    *   Age                       
    *   Annual Income (k$)        
    *   Spending Score (1-100)    

 

<a name="DataPre-processing"></a>  

## [Data Pre-processing](Code/P13_Code.ipynb)
After I had all the data I needed, I needed to check it was ready for exploration and later modelling.   
*   General NULL and data validity checks  

*   Formatted column headers to SQL compatibility. 
The data contained no null values and all datatypes lined up with their field description. <br>

```python
# Viewing the data types of the columns
data.dtypes

# Viewing dataset shape
data.shape

# 1st check for null values and datatype check 
data.info()
```

<br>

*   Some programming languages can be case sensitive like python and C++ for example, so using lower case letters for variable names allows for straightforward use of data in different programming languages.<br>

```python
# (SQL standard) Formatting column headers by removing potential capital letters and spaces in column headers 
data.columns = data.columns.str.lower()
data.columns = data.columns.str.replace(' ','_')
```


<a name="DataWarehousing"></a>

## [Data Warehousing](Code/P13_Code.ipynb)
I warehouse all data in a Postgre database for later use and reference.

*   ETL in python to PostgreSQL Database.
*   Formatted column headers to SQL compatibility. 


```python 
# Function to warehouse data in a Postgre database 
def store_data(data,tablename):
    """
    :param data: variable, enter name of dataset you'd like to warehouse
    :param tablename: str, enter name of table for data 
    """

    # SQL table header format
    tablename = tablename.lower()
    tablename = tablename.replace(' ','_')

    # Saving cleaned data as csv
    data.to_csv(f'../Data/{tablename}_clean.csv', index=False)

    # Engine to access postgre
    engine = create_engine('postgresql+psycopg2://postgres:password@localhost:5432/projectsdb')

    # Loads dataframe into PostgreSQL and replaces table if it exists
    data.to_sql(f'{tablename}', engine, if_exists='replace',index=False)

    # Confirmation of ETL 
    return("ETL successful, {num} rows loaded into table: {tb}.".format(num=len(data.iloc[:,0]), tb=tablename))
 
# Calling store_data function to warehouse cleaned data
store_data(data,"P13 Customer Segmentation")
```

<a name="EDA"></a>  

## [Exploratory data analysis](Code/P13_Code.ipynb) 
I looked at the distributions of the data and the value counts for the various categorical variables that would be fed into the model. Below are a few highlights from the analysis.

*   Age has a negative relationship with spending score, like because those in the data that are older spend less and those in the data that are younger spend more.
*   Annual income and spending score have almost no correlation.   

<img src="images/customer_seg_correlation.png" />

<a name="FeatEng"></a>  

## [Feature Engineering](Code/P13_Code.ipynb) 
I kept only 2 columns; annual_income_(k$) and spending_score_(1-100) as they showed no initial correlation. 

```python
# Grouping columns that will be used for clustering (Annual Income & Spending score column)
X = data.iloc[:,[3,4]].values
```

<img src="images/WSS_formula.png"/>

*   I applied the Within Sum of squares to find the inertia for the optimum number of clusters. Inertia is calculated as the sum of squared distances between data points and the centres of the clusters they belong to. Inertia quantifies the within-cluster variation.

```python
# Chosing the optimum number of clusters 
# Finding wcss(within clusters sum of squares) value for different number of clusters

# Defining wcss list 
wcss = []

# For loop to find optimum number of clusters between 1 and 11
for i in range(1,11):
  
  # Initiating KMeans algorithm 
  kmeans = KMeans(n_clusters=i, init='k-means++', random_state=23)

  # Fitting KMeans to X
  kmeans.fit(X)

  # Inertia append for values 1 to 10 
  wcss.append(kmeans.inertia_)
```

<img src="images/elbow_point_graph.png"/>

*   The elbow graph was used to show the best number of clusters, here shown to be 6 for there is a gradual decline in WCSS. 

  

<a name="ModelBuild"></a> 

## [ML/DL Model Building](Code/P13_Code.ipynb)

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

<img src="images/customer_groups.png" />

<a name="Prjmanage"></a> 

## [Project Management (Agile/Scrum/Kanban)](https://www.atlassian.com/software/jira)
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

## Questions & Contact me 
For questions, feedback, and contribution requests contact me
* ### [Click here to email me](mailto:contactmattithyahu@gmail.com) 
* ### [See more projects here](https://mattithyahudata.github.io/)









