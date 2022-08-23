# Properties_sao-paulo
#This is a treatment of data and some graphics for the city of São Paulo in April 2019.
#Please use Jupyter Notebook or Google Colaboratory to run this code.

#Library Import

!pip install plotly

!pip install Basemap

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import seaborn
from math import radians
import plotly.express as px
from mpl_toolkits.basemap import Basemap

#Accessing Google drive file

#File link 

https://drive.google.com/file/d/17iyZp1HdaQ5_oGMurx6gR64meoFB3Ea4/view?usp=sharing

data = pd.read_csv('/content/drive/MyDrive/sao-paulo-properties-april-2019.csv')
data

plt.figure(figsize=(8, 8))
m = Basemap(projection='ortho', resolution=None, lat_0=-10, lon_0=-65)
m.bluemarble(scale=0.5);

fig = plt.figure(figsize=(8, 8))
m = Basemap(projection='lcc', resolution=None,
            width=8E6, height=8E6, 
            lat_0=-10, lon_0=-65,)
m.bluemarble(scale=0.5, alpha=1)

# Map (long, lat) to (x, y) for plotting
x, y = m(-46.625290,  -23.533773)
plt.plot(x, y, 'ok', markersize=5, color='red')
plt.text(x, y, ' Sao Paulo', fontsize=12, color='white');

# Cleaning up unnecessary data

data ['Negotiation Type'].value_counts()

data_clean = data.drop(['Latitude', 'Longitude', 'New', 'Size', 'Condo', 'Property Type'],axis=1, inplace=False)
data_clean

data_clean2 = data.drop(['Latitude', 'Longitude', 'New', 'Size', 'Condo', 'Property Type'],axis=1, inplace=False)
data_clean2

data2 = data_clean
data3 = data_clean2

remove_rent = data2[data2["Negotiation Type"] == 'rent'].index
data2.drop(remove_rent, inplace=True)
data2


top_sales =  data2 [['Negotiation Type', 'Price']].head(10).set_index('Negotiation Type').sort_values('Price', ascending=False)
top_sales

top_sales.plot(kind='bar', color='black', figsize=(20,10), title='More expensive sales')

remove_sales = data3[data3["Negotiation Type"] == 'sale'].index
data3.drop(remove_sales, inplace=True)
data3

top_rent =  data3 [['Negotiation Type', 'Price']].head(10).set_index('Negotiation Type').sort_values('Price', ascending=False)
top_rent

top_rent.plot(kind='bar', color='black', figsize=(20,10), title='More expensive rental rates')

#CHECKING THE AREAS

data.head()

latitude = data ['Latitude']
longitude = data ['Longitude']
price = data ["Price"]
area = data ['District']

for area in [50, 150 ,250]:
    plt.scatter([latitude], [longitude], c='k', alpha=0.3, s=area, label=str(area) + 'km$^2$')
plt.legend (scatterpoints=1, frameon=False, labelspacing=1, title= 'City Areas')
plt.title ('Area and Population of São Paulo City')
plt.show()

seaborn.set()
plt.scatter (longitude, latitude, label=None, c=np.log10(price), cmap='viridis', s=area, linewidth=0, alpha=0.5)
plt.axis (aspect= 'equal')
plt.xlabel ('Latitude')
plt.ylabel ('Longitude')
plt.colorbar (label= 'log$_{10}$(Preço)')
plt.clim(15, 20)
plt.title ('Area and Values of São Paulo City')


#Interactive world map for accurate location consultation

fig = px.density_mapbox (data, lat='Latitude', lon='Longitude', radius=1,
                         center=dict(lat=0, lon=180), zoom=1.5,
                         mapbox_style='stamen-terrain')
fig.show()

fig2 = px.scatter_mapbox(data, lat="Latitude", lon="Longitude", hover_name="District", hover_data=["District", "Price"],
                        color_discrete_sequence=["fuchsia"], zoom=3, height=600)
fig2.update_layout(mapbox_style="open-street-map")
fig2.update_layout(margin={"r":0,"t":0,"l":0,"b":0})
fig2.show()
