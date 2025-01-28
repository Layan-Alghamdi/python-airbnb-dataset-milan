# python-airbnb-dataset-milan
## :mag: Select the data of your fav city from
python code for insights on airbnb booking of milan, we are using calender and listing csv files.

https://insideairbnb.com/get-the-data/ I have done all the calculations based on data from Milan
<img width="980" alt="Screenshot 1446-07-28 at 9 42 26 AM" src="https://github.com/user-attachments/assets/8be2a23e-b60c-4de6-80dd-7a9366234f43" />


```python
import pandas as pd 
calendar = pd.read_csv('/Users/layanalghamdi/Desktop/calendar.csv')
print(calendar.head())
```
Let`s dive into exciting insights!

## 1. Want to know the number of available and unavailable rooms
```python
calendar.available.value_counts()
```
<img width="538" alt="Screenshot 1446-07-28 at 9 57 20 AM" src="https://github.com/user-attachments/assets/e114efe1-0237-46b4-a366-b394adf2fca0" />
f (false) means not available, t(true) means available.


## 2. Purpose: Calculates the percentage of available (t) and unavailable (f) dates.
Explanation:
* value_counts(normalize=True) gives proportions.
* Multiplying by 100 converts the proportions into percentages
```python
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
<img width="759" alt="Screenshot 1446-07-28 at 10 07 33 AM" src="https://github.com/user-attachments/assets/aaef62af-89ed-4ba1-b4cb-048fb3e3dc81" />

## 3. Let`s Count the busiest day! :rotating_light:
We will be counting the most unavailable days (given by f)

```python
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
<img width="648" alt="Screenshot 1446-07-28 at 10 08 40 AM" src="https://github.com/user-attachments/assets/5a604def-710c-4f8c-af69-807ccf64b067" />

## 4. Plot a bar graph to show availability percentage :chart_with_upwards_trend:
```python
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
<img width="664" alt="Screenshot 1446-07-28 at 10 11 16 AM" src="https://github.com/user-attachments/assets/7d54b25d-462b-4b43-a222-d55597bc9130" />

## 5. Plot the busiest day
```python
busiest_dates.head(10).plot(kind='bar', color='hotpink')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
<img width="564" alt="Screenshot 1446-07-28 at 10 15 37 AM" src="https://github.com/user-attachments/assets/57ef9c14-2b1f-4dbf-bb60-b4ad0f069ce7" />

## Download listings dataset of Milan from :page_facing_up:
```python
import pandas as pd
listings = pd.read_csv('/Users/layanalghamdi/Desktop/listings-2.csv')
```
<img width="842" alt="Screenshot 1446-07-28 at 11 23 44 AM" src="https://github.com/user-attachments/assets/fcb3ecbb-ee47-4c51-b640-b40494dbf07c" />

```python
listings.columns
```
<img width="560" alt="Screenshot 1446-07-28 at 11 25 31 AM" src="https://github.com/user-attachments/assets/3f4a64ac-ecd1-4718-aaab-da57bac6a9d8" />

## Room type and price is given seperately

Let`s Combine and visualize them

```python
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='pink')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```

<img width="553" alt="Screenshot 1446-07-28 at 11 28 35 AM" src="https://github.com/user-attachments/assets/ec1a0817-026d-447d-9c21-ee24472ff6c9" />

## Which are the top 10 neighborhoods with the most listings? :monocle_face:

```python
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

# Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
<img width="645" alt="Screenshot 1446-07-28 at 11 31 25 AM" src="https://github.com/user-attachments/assets/e93a88bb-72ea-476a-ba8f-4e7ac3b58246" />

## Geographical Distribution of Listings (Price Colored)

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

```python
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img width="782" alt="Screenshot 1446-07-28 at 11 34 30 AM" src="https://github.com/user-attachments/assets/ed15bd18-ea28-4274-88ce-9586fd7e3801" />

## Let us see the listings on a real map :boom:

* Hotter Areas (Red/Yellow): High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas. Popular Locations: These regions might be more popular or in high demand. It could be near tourist attractions, popular neighborhoods, or central areas in Milan where people tend to stay more often. Colder Areas (Green/Blue):

* Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available. Less Popular Locations: These areas might be less popular or further from key attractions. If you're looking at pricing or other factors, lower density could imply less competition in these regions, which might indicate more affordable areas or less tourist traffic. Density Patterns:

* Clustered Areas: If you notice clusters of heatmap intensity, they represent hotspots. These might correspond to high-traffic areas like resorts, beaches, or urban centers. Spread-Out Listings: If the heatmap shows a more uniform distribution, it could suggest that listings are more evenly spread across the region, which may reflect a more balanced demand for rentals across different areas of Milan.

```python
import folium
from folium.plugins import HeatMap
import pandas as pd

# Assuming `listings` is your DataFrame with columns 'latitude', 'longitude', and 'price'.
milan_data = listings[['latitude', 'longitude', 'price']]  # Include price if needed

# Create a base map centered around Milan
m = folium.Map(location=[45.468724, 9.180132], zoom_start=12)

# Prepare the data for the heatmap 
heat_data = [[row['latitude'], row['longitude']] for index, row in milan_data.iterrows()]

# Add the heatmap to the map
HeatMap(heat_data).add_to(m)

# Save the map as an HTML file to view in a browser
m.save('milan_heatmap.html')

# If you're using Jupyter Notebook, you can display the map directly in the notebook:
m
```

<img width="830" alt="Screenshot 1446-07-28 at 11 54 04 AM" src="https://github.com/user-attachments/assets/1da0a2e7-574e-4684-b39f-4c6715c204f3" />

## How many listings have more than 100 reviews?
```python
# Filter the dataset to include only listings with more than 100 reviews
listings_with_100_plus_reviews = listings[listings['number_of_reviews'] > 100]

# Count the number of listings that match the criteria
count_of_listings = listings_with_100_plus_reviews.shape[0]

# Display the result
print(f"The number of listings with more than 100 reviews is: {count_of_listings}")
```
<img width="464" alt="Screenshot 1446-07-28 at 2 37 52 PM" src="https://github.com/user-attachments/assets/db371abd-3e01-4ced-b61a-db69f43cd554" />

## What are the top 5 most expensive listings in terms of price?
```python
# Sort the data by price in descending order and select the top 5 listings
top_5_expensive_listings = listings.sort_values(by='price', ascending=False).head(5)

# Plot the top 5 most expensive listings
plt.figure(figsize=(10, 6))
plt.bar(top_5_expensive_listings['name'], top_5_expensive_listings['price'], color='purple')
plt.title("Top 5 Most Expensive Listings")
plt.xlabel("Listing Name")
plt.ylabel("Price")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

```
<img width="840" alt="Screenshot 1446-07-28 at 2 43 48 PM" src="https://github.com/user-attachments/assets/c1ad0432-a082-4539-b94e-050f0635f103" />
