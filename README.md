# **GATSON_EMR_PA4**

# **ECE Programming Assignment 4**

## **PROBLEM 1**
This program performs data exploration on a student board exam performance dataset. It loads student records from an Excel file, computes a per-student average across four subjects, and filters the data based on specific conditions.

---
This imports the required libraries and loads the dataset from the 'board2.csv' sheet inside the Excel file.
```python
import pandas as pd    #Import pandas for data manipulation
import matplotlib.pyplot as plt    #Import matplotlib for plotting

df = pd.read_excel("board2 (1).xlsx", sheet_name="board2.csv")    #Load the board exam dataset from the specified sheet
```

This computes a new column 'Average' for each student by taking the mean of their four subject scores (Math, Electronics, GEAS, Communication).
```python
df["Average"] = df[["Math", "Electronics", "GEAS", "Communication"]].mean(axis=1)    #Compute the row-wise average across the four subjects
```

This displays the first five rows of the dataset to inspect its structure and confirm the new 'Average' column.
```python
df.head()    #Show the first five rows to understand data structure
```

This filters the dataset to find students from the **Visayas** hometown who are in the **Communication** track, returning only their name, gender, Math, Electronics, and Average.
```python
VisComm = df[(df["Hometown"] == "Visayas") & (df["Track"] == "Communication")][
    ["Name", "Gender", "Math", "Electronics", "Average"]
]

display(VisComm)    #Display the filtered Visayas-Communication students
print("Number of rows:", VisComm.shape[0])    #Print how many students matched the filter
```

---

## **PROBLEM 2**
This program demonstrates conditional row selection on the board exam dataset. It shows how to filter rows by multiple conditions and how to narrow the results further based on a numeric threshold.

---
This filters the dataset to find **female** students from the **Visayas** hometown, returning only their name, track, GEAS, Electronics, and Average.
```python
VisFemale = df[(df["Hometown"] == "Visayas") & (df["Gender"] == "Female")][
    ["Name", "Track", "GEAS", "Electronics", "Average"]
]

display(VisFemale)    #Display the filtered female Visayas students
```

This narrows the previous result further, keeping only the female Visayas students whose Average is greater than or equal to 60.
```python
VisFemale_high = VisFemale[VisFemale["Average"] >= 60]    #Keep only students with Average >= 60
display(VisFemale_high)
```

---

## **PROBLEM 3**
This program demonstrates group-wise aggregation on the board exam dataset. It computes the mean Average by Track, Gender, and Hometown, then visualizes the results with bar charts to compare group performance.

---
This groups the dataset by Track, Gender, and Hometown, and computes the mean Average for each group. The results are sorted in descending order so the highest-performing group appears first.
```python
track_mean = df.groupby("Track")["Average"].mean().sort_values(ascending=False)    #Mean Average per Track
gender_mean = df.groupby("Gender")["Average"].mean().sort_values(ascending=False)    #Mean Average per Gender
hometown_mean = df.groupby("Hometown")["Average"].mean().sort_values(ascending=False)    #Mean Average per Hometown

print("Mean Average by Track")
display(track_mean)

print("Mean Average by Gender")
display(gender_mean)

print("Mean Average by Hometown")
display(hometown_mean)
```

This visualizes the three sets of group means using side-by-side bar charts and prints a short interpretation of the highest-performing group for each category.
```python
fig, axes = plt.subplots(1, 3, figsize=(15, 5))    #Create a 1x3 grid of bar charts

axes[0].bar(track_mean.index, track_mean.values, color="steelblue")    #Bar chart for Track
axes[0].set_title("Mean Average by Track")
axes[0].set_xlabel("Track")
axes[0].set_ylabel("Mean Average")
axes[0].tick_params(axis="x", rotation=15)

axes[1].bar(gender_mean.index, gender_mean.values, color="seagreen")    #Bar chart for Gender
axes[1].set_title("Mean Average by Gender")
axes[1].set_xlabel("Gender")
axes[1].set_ylabel("Mean Average")

axes[2].bar(hometown_mean.index, hometown_mean.values, color="indianred")    #Bar chart for Hometown
axes[2].set_title("Mean Average by Hometown")
axes[2].set_xlabel("Hometown")
axes[2].set_ylabel("Mean Average")

plt.tight_layout()    #Adjust spacing so labels do not overlap
plt.show()

print("1. Track: Communication has the highest sample mean Average.")
print("2. Gender: Male has the highest sample mean Average.")
print("3. Hometown: Visayas has the highest sample mean Average.")
print("Note: These describe the observed dataset only. A difference in group means")
print("does not, by itself, establish that a feature causes a higher board-exam score.")
```
This identifies the group with the highest sample mean Average in each category (Track, Gender, Hometown) and prints the results with a short interpretation note.
```python
highest_track = track_mean.idxmax()       #Track with the highest mean Average
highest_gender = gender_mean.idxmax()     #Gender with the highest mean Average
highest_hometown = hometown_mean.idxmax() #Hometown with the highest mean Average

print(f"Track: {highest_track} has the highest sample mean Average of {track_mean.max():.2f}.")
print(f"Gender: {highest_gender} has the highest sample mean Average of {gender_mean.max():.2f}.")
print(f"Hometown: {highest_hometown} has the highest sample mean Average of {hometown_mean.max():.2f}.")

print("Note: These describe the observed dataset only. A difference in group means")
print("does not, by itself, establish that a feature causes a higher board-exam score.")
```

---
