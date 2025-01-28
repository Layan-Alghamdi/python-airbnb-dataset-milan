# python-airbnb-dataset-milan
## Select the data of your fav city from
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

## 3. Let`s Count the busiest day!
We will be counting the most unavailable days (given by f)

```python
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
<img width="648" alt="Screenshot 1446-07-28 at 10 08 40 AM" src="https://github.com/user-attachments/assets/5a604def-710c-4f8c-af69-807ccf64b067" />

## 4. Plot a bar graph to show availability percentage
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


