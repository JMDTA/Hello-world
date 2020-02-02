import pandas as pd
import numpy as np
from matplotlib import pyplot as plt

%matplotlib inline
df = pd.read_csv("C:\\Users\\shiva\\Main_Central_warehouse.csv", encoding = 'unicode_escape')
df.head()
# s is the sum
# n is total number of events
#j variable that helps to mention the number of cluster 
#K is the coloumn created for cluster
#l is the coloumn for the lat (of the first and the last point of the cluster)
#m is the coloumn for the lon (of the first and the last point of the cluster)

s=0
n=0
j=0
k=[0]
l=[]
m=[]
summ=[]
#code will run till the length of the data frame (df)
while n<=(len(df)):
# when the sum of the orders are more than 500 then the cluster should be changed.    
    while(s<=480):
    s=s+df.iloc[n][3]
        if (s<=515):
            k.append(j)
            n=n+1
            print(s)
        else:
            break
                
#once the cluster is changing, i.e. we are considering new truck , s should be 0 again.   
    summ.append(s)
    s=0
    #we would consider the longitude and latitude of the that point, as that is the starting point of the next cluster.
    #we would consider the longitude and latitude of previous point too, as that was the ending point of the last cluster.
    l.append(df.iloc[n-1][1])
    m.append(df.iloc[n-1][2])
    l.append(df.iloc[n][1])
    m.append(df.iloc[n][2])
    j=j+1
    
    
 # upload the K as Truck , l as distance_LAt, m as Distance_Lon, summ as Sum
df['Truck'] = pd.Series(k)
df['distance_LAt']= pd.Series(l)
df["distance_lon"]=pd.Series(m) 
df["sum"]=pd.Series(summ)

df.head()
df.to_csv('Main_Central_warehouse.csv')

# we would take the last csv file that we created and would merge the lat and long column , for all the points that the truck would be covering and also the distance of starting and the ending points of the truck from the warehouse
df1 = pd.read_csv("C:\\Users\\shiva\\Main_Central_warehouse.csv", encoding = 'unicode_escape')
from geopy import distance
#from geopy.distance import great_circle
distance_1 = []


for n in range (0,1355):
    distance_1.append(distance.distance(df1.iloc[n][11], df1.iloc[n+1][11]))
for m in range (0,2364):
    distance_1.append(distance.distance( (52.81553914,  9.48523469), df1.iloc[m][8]))

print (distance_1 )  
df2 = pd.DataFrame()
#the distance is exported to csv file. 
df2.to_csv('Main_central_d.csv')


  
df2['distance_2']=distance_1
