
 ## <font color=black> PyCity Schools Analysis </font>

Trends Observed:

<br />1 The top 5 performing schools are Charter schools and bottom 5 performing schools are District schools.
<br />2 District schools are larger in size compared to Charter schools.
<br />3 District schools receive higher budget than Charter schools.
<br />4 Despite less funding, Charter school students have higher passin scores than District school students.
<br />5 There does not seem to be significant difference in passing scores by subject when compared by grade levels.



```python
# Dependencies
import pandas as pd
import numpy as np
import os
```


```python
# Use pd to read files
schools_df = pd.read_csv('./schools_complete.csv')
schools_df.head() 
 


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Use pd to read files
students_df = pd.read_csv('./students_complete.csv')
students_df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>



 ## <font color=black> District Summary </font>


```python
# Get the total schools
total_schools = len(schools_df['School ID'])

# Get the total students
total_students = schools_df['size'].sum()

# Get the total budget
total_budget = schools_df['budget'].sum()

# Get the Average Math Score 
avg_math_score = round(students_df["math_score"].mean(), 2)

# Get the Average Reading Score 
avg_reading_score = round(students_df["reading_score"].mean(), 2)

# Calculate the percentage of students passing math
students_passing_math = students_df.loc[students_df["math_score"] >= 70,:]
percent_pass_math = round(float(students_passing_math["math_score"].count()/total_students)*100, 1)

# Calculate the  percentage of students passing reading test
students_passing_read = students_df.loc[students_df["reading_score"] >= 70,:]
percent_pass_read = round(float(students_passing_read["reading_score"].count()/total_students)*100, 1)

# Calculate the overall passing rate for math and reading
overall_pass_rate = round((percent_pass_math + percent_pass_read)/2, 2)

# Create a District Summary DataFrame using the above values
summary_df = pd.DataFrame({"Total Schools": [total_schools],
                          "Total Students": [total_students],
                          "Total Budget": [total_budget],
                          "Average Math Score": [avg_math_score],
                          "Average Reading Score":[avg_reading_score],
                          "% Passing Math": [percent_pass_math],
                          "% Passing Reading": [percent_pass_read],
                          "% Overall Passing Rate": [overall_pass_rate]})
district_summary_df = pd.DataFrame(summary_df, columns=["Total Schools", "Total Students", "Total Budget","Average Math Score",
                                                       "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"])

district_summary_df["Total Students"] = district_summary_df["Total Students"].map('{:,}'.format)
district_summary_df["Total Budget"] = district_summary_df["Total Budget"].map('${:,.2f}'.format)


district_summary_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39,170</td>
      <td>$24,649,428.00</td>
      <td>78.99</td>
      <td>81.88</td>
      <td>75.0</td>
      <td>85.8</td>
      <td>80.4</td>
    </tr>
  </tbody>
</table>
</div>



 ## <font color=black> School Summary </font>


```python
# Change the name of the header in the schools dataframe from name to school
schools_df = schools_df.rename(columns={"name": "school"})
schools_df.columns

# Merge the two DataFrames together based on the school name
schools_data_df = pd.merge(schools_df, students_df, on="school")

# Count the number of students in each school
student_counts = schools_data_df["school"].value_counts()


# Get the school type  
school_type = schools_data_df.groupby('school')['type'].unique()
school_type = school_type.str[0]

# Calculate the total school budget for each school 
each_school_budget = schools_data_df.groupby('school')['budget'].unique()
each_school_budget = each_school_budget.astype(float)

# Calculate each student budgget 
budget_per_student = round(each_school_budget/student_counts,2)
budget_per_student = budget_per_student.astype(float)

# Get the average math and reading scores for each school using groupby and mean functions
school_avg_math_score = round(schools_data_df.groupby('school')['math_score'].mean(),2)
school_avg_reading_score = round(schools_data_df.groupby('school')['reading_score'].mean(),2)

# Create new Dataframe with passing scores for reading and math 
passing_df = schools_data_df.loc[(schools_data_df['math_score'] >= 70) & (schools_data_df['reading_score'] >=70)]
passing_math_df = schools_data_df.loc[(schools_data_df['math_score'] >= 70)]
passing_reading_df = schools_data_df.loc[(schools_data_df['reading_score'] >= 70)]

# Use groupby to get score percentage of students passing math and reading and overall passing rate %
percent_passing_math = round((passing_math_df.groupby('school')['math_score'].count()/student_counts)*100, 1)
percent_passing_reading = round((passing_reading_df.groupby('school')['reading_score'].count()/student_counts)*100, 1)
percent_overall_passing = round((percent_passing_math + percent_passing_reading)/2, 2)

# Creating the School Summary Dataframe based on the given data
school_summary_table = pd.DataFrame({"School Type":school_type,
                                "Total Students":student_counts,
                                "Total School Budget":each_school_budget,
                                "Per Student Budget":budget_per_student,
                                "Average Math Score": school_avg_math_score,
                                "Average Reading Score": school_avg_reading_score,
                                "% Passing Math": percent_passing_math,
                                "% Passing Reading": percent_passing_reading,
                                "% Overall Passing Rate":percent_overall_passing})

# Arrange the columns in order 
school_summary_table = pd.DataFrame(school_summary_table, columns=["School Type", "Total Students", "Total School Budget","Per Student Budget", "Average Math Score",
                                                       "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"])

# Format the df 
school_summary_table["Total Students"] = school_summary_table["Total Students"].map('{:,}'.format)
school_summary_table["Total School Budget"] = school_summary_table["Total School Budget"].map('${:,.2f}'.format)
school_summary_table["Per Student Budget"] = school_summary_table["Per Student Budget"].map('${:,.2f}'.format)

school_summary_table
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4,976</td>
      <td>$3,124,928.00</td>
      <td>$628.00</td>
      <td>77.05</td>
      <td>81.03</td>
      <td>66.7</td>
      <td>81.9</td>
      <td>74.30</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1,858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.06</td>
      <td>83.98</td>
      <td>94.1</td>
      <td>97.0</td>
      <td>95.55</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2,949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.71</td>
      <td>81.16</td>
      <td>66.0</td>
      <td>80.7</td>
      <td>73.35</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2,739</td>
      <td>$1,763,916.00</td>
      <td>$644.00</td>
      <td>77.10</td>
      <td>80.75</td>
      <td>68.3</td>
      <td>79.3</td>
      <td>73.80</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1,468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.35</td>
      <td>83.82</td>
      <td>93.4</td>
      <td>97.1</td>
      <td>95.25</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4,635</td>
      <td>$3,022,020.00</td>
      <td>$652.00</td>
      <td>77.29</td>
      <td>80.93</td>
      <td>66.8</td>
      <td>80.9</td>
      <td>73.85</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$248,087.00</td>
      <td>$581.00</td>
      <td>83.80</td>
      <td>83.81</td>
      <td>92.5</td>
      <td>96.3</td>
      <td>94.40</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2,917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.63</td>
      <td>81.18</td>
      <td>65.7</td>
      <td>81.3</td>
      <td>73.50</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4,761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.07</td>
      <td>80.97</td>
      <td>66.1</td>
      <td>81.2</td>
      <td>73.65</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.84</td>
      <td>84.04</td>
      <td>94.6</td>
      <td>95.9</td>
      <td>95.25</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3,999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.84</td>
      <td>80.74</td>
      <td>66.4</td>
      <td>80.2</td>
      <td>73.30</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1,761</td>
      <td>$1,056,600.00</td>
      <td>$600.00</td>
      <td>83.36</td>
      <td>83.73</td>
      <td>93.9</td>
      <td>95.9</td>
      <td>94.90</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1,635</td>
      <td>$1,043,130.00</td>
      <td>$638.00</td>
      <td>83.42</td>
      <td>83.85</td>
      <td>93.3</td>
      <td>97.3</td>
      <td>95.30</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2,283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.27</td>
      <td>83.99</td>
      <td>93.9</td>
      <td>96.5</td>
      <td>95.20</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1,800</td>
      <td>$1,049,400.00</td>
      <td>$583.00</td>
      <td>83.68</td>
      <td>83.96</td>
      <td>93.3</td>
      <td>96.6</td>
      <td>94.95</td>
    </tr>
  </tbody>
</table>
</div>



## <font color=black> Top Five Performing Schools (By Percent Passing Rate) </font>


```python
# Arrange schools by passing rate; from high to low
top_5_schools = school_summary_table.sort_values("% Overall Passing Rate", ascending=False, inplace=False)
top_5_schools.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1,858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.06</td>
      <td>83.98</td>
      <td>94.1</td>
      <td>97.0</td>
      <td>95.55</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1,635</td>
      <td>$1,043,130.00</td>
      <td>$638.00</td>
      <td>83.42</td>
      <td>83.85</td>
      <td>93.3</td>
      <td>97.3</td>
      <td>95.30</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1,468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.35</td>
      <td>83.82</td>
      <td>93.4</td>
      <td>97.1</td>
      <td>95.25</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.84</td>
      <td>84.04</td>
      <td>94.6</td>
      <td>95.9</td>
      <td>95.25</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2,283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.27</td>
      <td>83.99</td>
      <td>93.9</td>
      <td>96.5</td>
      <td>95.20</td>
    </tr>
  </tbody>
</table>
</div>



## <font color=black> Bottom Five Performing Schools (By Percent Passing Rate) </font>


```python
# Arrange schools by passing rate; from low to high
bottom_5_schools = school_summary_table.sort_values("% Overall Passing Rate", ascending=True, inplace=False)
bottom_5_schools.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3,999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.84</td>
      <td>80.74</td>
      <td>66.4</td>
      <td>80.2</td>
      <td>73.30</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2,949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.71</td>
      <td>81.16</td>
      <td>66.0</td>
      <td>80.7</td>
      <td>73.35</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2,917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.63</td>
      <td>81.18</td>
      <td>65.7</td>
      <td>81.3</td>
      <td>73.50</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4,761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.07</td>
      <td>80.97</td>
      <td>66.1</td>
      <td>81.2</td>
      <td>73.65</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2,739</td>
      <td>$1,763,916.00</td>
      <td>$644.00</td>
      <td>77.10</td>
      <td>80.75</td>
      <td>68.3</td>
      <td>79.3</td>
      <td>73.80</td>
    </tr>
  </tbody>
</table>
</div>



## <font color=black> Math Scores By Grade </font>


```python
# Create grade level average math scores for each school 
ninth_math = students_df.loc[students_df['grade'] == '9th'].groupby('school')["math_score"].mean()
tenth_math = students_df.loc[students_df['grade'] == '10th'].groupby('school')["math_score"].mean()
eleventh_math = students_df.loc[students_df['grade'] == '11th'].groupby('school')["math_score"].mean()
twelfth_math = students_df.loc[students_df['grade'] == '12th'].groupby('school')["math_score"].mean()

math_scores = pd.DataFrame({
        "9th": ninth_math,
        "10th": tenth_math,
        "11th": eleventh_math,
        "12th": twelfth_math
})
math_scores = math_scores[['9th', '10th', '11th', '12th']]
math_scores.index.name = "School"

# Display results and format
math_scores.style.format({'9th': '{:.2f}', 
                          "10th": '{:.2f}', 
                          "11th": "{:.2f}", 
                          "12th": "{:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" >School</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row0_col0" class="data row0 col0" >77.08</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row0_col1" class="data row0 col1" >77.00</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row0_col2" class="data row0 col2" >77.52</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row0_col3" class="data row0 col3" >76.49</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row1_col0" class="data row1 col0" >83.09</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row1_col1" class="data row1 col1" >83.15</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row1_col2" class="data row1 col2" >82.77</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row1_col3" class="data row1 col3" >83.28</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row2_col0" class="data row2 col0" >76.40</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row2_col1" class="data row2 col1" >76.54</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row2_col2" class="data row2 col2" >76.88</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row2_col3" class="data row2 col3" >77.15</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row3_col0" class="data row3 col0" >77.36</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row3_col1" class="data row3 col1" >77.67</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row3_col2" class="data row3 col2" >76.92</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row3_col3" class="data row3 col3" >76.18</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row4_col0" class="data row4 col0" >82.04</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row4_col1" class="data row4 col1" >84.23</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row4_col2" class="data row4 col2" >83.84</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row4_col3" class="data row4 col3" >83.36</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row5_col0" class="data row5 col0" >77.44</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row5_col1" class="data row5 col1" >77.34</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row5_col2" class="data row5 col2" >77.14</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row5_col3" class="data row5 col3" >77.19</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row6_col0" class="data row6 col0" >83.79</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row6_col1" class="data row6 col1" >83.43</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row6_col2" class="data row6 col2" >85.00</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row6_col3" class="data row6 col3" >82.86</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row7_col0" class="data row7 col0" >77.03</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row7_col1" class="data row7 col1" >75.91</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row7_col2" class="data row7 col2" >76.45</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row7_col3" class="data row7 col3" >77.23</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row8_col0" class="data row8 col0" >77.19</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row8_col1" class="data row8 col1" >76.69</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row8_col2" class="data row8 col2" >77.49</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row8_col3" class="data row8 col3" >76.86</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row9_col0" class="data row9 col0" >83.63</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row9_col1" class="data row9 col1" >83.37</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row9_col2" class="data row9 col2" >84.33</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row9_col3" class="data row9 col3" >84.12</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row10_col0" class="data row10 col0" >76.86</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row10_col1" class="data row10 col1" >76.61</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row10_col2" class="data row10 col2" >76.40</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row10_col3" class="data row10 col3" >77.69</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row11_col0" class="data row11 col0" >83.42</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row11_col1" class="data row11 col1" >82.92</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row11_col2" class="data row11 col2" >83.38</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row11_col3" class="data row11 col3" >83.78</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row12_col0" class="data row12 col0" >83.59</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row12_col1" class="data row12 col1" >83.09</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row12_col2" class="data row12 col2" >83.50</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row12_col3" class="data row12 col3" >83.50</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row13_col0" class="data row13 col0" >83.09</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row13_col1" class="data row13 col1" >83.72</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row13_col2" class="data row13 col2" >83.20</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row13_col3" class="data row13 col3" >83.04</td> 
    </tr>    <tr> 
        <th id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3level0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row14_col0" class="data row14 col0" >83.26</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row14_col1" class="data row14 col1" >84.01</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row14_col2" class="data row14 col2" >83.84</td> 
        <td id="T_a8c54950_5662_11e8_9580_a4ac86ba53d3row14_col3" class="data row14 col3" >83.64</td> 
    </tr></tbody> 
</table> 



## <font color=black> Reading Scores By Grade </font>


```python

# Create grade level average reading scores for each school
ninth_reading = students_df.loc[students_df['grade'] == '9th'].groupby('school')["reading_score"].mean()
tenth_reading = students_df.loc[students_df['grade'] == '10th'].groupby('school')["reading_score"].mean()
eleventh_reading = students_df.loc[students_df['grade'] == '11th'].groupby('school')["reading_score"].mean()
twelfth_reading = students_df.loc[students_df['grade'] == '12th'].groupby('school')["reading_score"].mean()

# Merge the reading score averages by school and grade
reading_scores = pd.DataFrame({
        "9th": ninth_reading,
        "10th": tenth_reading,
        "11th": eleventh_reading,
        "12th": twelfth_reading
})
reading_scores = reading_scores[['9th', '10th', '11th', '12th']]
reading_scores.index.name = "School"

# Format the results
reading_scores.style.format({'9th': '{:.2f}', 
                             "10th": '{:.2f}', 
                             "11th": "{:.2f}", 
                             "12th": "{:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" >School</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row0_col0" class="data row0 col0" >81.30</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row0_col1" class="data row0 col1" >80.91</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row0_col2" class="data row0 col2" >80.95</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row0_col3" class="data row0 col3" >80.91</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row1_col0" class="data row1 col0" >83.68</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row1_col1" class="data row1 col1" >84.25</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row1_col2" class="data row1 col2" >83.79</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row1_col3" class="data row1 col3" >84.29</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row2_col0" class="data row2 col0" >81.20</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row2_col1" class="data row2 col1" >81.41</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row2_col2" class="data row2 col2" >80.64</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row2_col3" class="data row2 col3" >81.38</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row3_col0" class="data row3 col0" >80.63</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row3_col1" class="data row3 col1" >81.26</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row3_col2" class="data row3 col2" >80.40</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row3_col3" class="data row3 col3" >80.66</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row4_col0" class="data row4 col0" >83.37</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row4_col1" class="data row4 col1" >83.71</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row4_col2" class="data row4 col2" >84.29</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row4_col3" class="data row4 col3" >84.01</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row5_col0" class="data row5 col0" >80.87</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row5_col1" class="data row5 col1" >80.66</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row5_col2" class="data row5 col2" >81.40</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row5_col3" class="data row5 col3" >80.86</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row6_col0" class="data row6 col0" >83.68</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row6_col1" class="data row6 col1" >83.32</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row6_col2" class="data row6 col2" >83.82</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row6_col3" class="data row6 col3" >84.70</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row7_col0" class="data row7 col0" >81.29</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row7_col1" class="data row7 col1" >81.51</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row7_col2" class="data row7 col2" >81.42</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row7_col3" class="data row7 col3" >80.31</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row8_col0" class="data row8 col0" >81.26</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row8_col1" class="data row8 col1" >80.77</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row8_col2" class="data row8 col2" >80.62</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row8_col3" class="data row8 col3" >81.23</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row9_col0" class="data row9 col0" >83.81</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row9_col1" class="data row9 col1" >83.61</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row9_col2" class="data row9 col2" >84.34</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row9_col3" class="data row9 col3" >84.59</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row10_col0" class="data row10 col0" >80.99</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row10_col1" class="data row10 col1" >80.63</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row10_col2" class="data row10 col2" >80.86</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row10_col3" class="data row10 col3" >80.38</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row11_col0" class="data row11 col0" >84.12</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row11_col1" class="data row11 col1" >83.44</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row11_col2" class="data row11 col2" >84.37</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row11_col3" class="data row11 col3" >82.78</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row12_col0" class="data row12 col0" >83.73</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row12_col1" class="data row12 col1" >84.25</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row12_col2" class="data row12 col2" >83.59</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row12_col3" class="data row12 col3" >83.83</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row13_col0" class="data row13 col0" >83.94</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row13_col1" class="data row13 col1" >84.02</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row13_col2" class="data row13 col2" >83.76</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row13_col3" class="data row13 col3" >84.32</td> 
    </tr>    <tr> 
        <th id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3level0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row14_col0" class="data row14 col0" >83.83</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row14_col1" class="data row14 col1" >83.81</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row14_col2" class="data row14 col2" >84.16</td> 
        <td id="T_a901e78c_5662_11e8_b2a4_a4ac86ba53d3row14_col3" class="data row14 col3" >84.07</td> 
    </tr></tbody> 
</table> 



## <font color=black> Scores By School Spending </font>


```python
# Create the bins to hold Data
bins = [0, 600, 625, 650, 1000]

# Create the names for the four bins created above
spending_ranges = ["$0-585", "$586-615", "$616-645", "$645-675"]

# Schools_spending_df = school_summary_table
school_summary_table["Spending Ranges (Per Student)"] = pd.cut(budget_per_student, bins=bins, labels=spending_ranges)

# Arrange Dataframe Columns 
spending_math_scores = school_summary_table.groupby(["Spending Ranges (Per Student)"])["Average Math Score"].mean()
spending_reading_scores = school_summary_table.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_summary_table.groupby(["Spending Ranges (Per Student)"])["% Passing Math"].mean()
spending_passing_reading = school_summary_table.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()
overall_passing_rate = (spending_math_scores + spending_reading_scores) / 2


scores_by_spend = school_summary_table[["Spending Ranges (Per Student)", "Average Math Score","Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
scores_by_spend = scores_by_spend.groupby("Spending Ranges (Per Student)").mean()

scores_by_spend
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Ranges (Per Student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>$0-585</th>
      <td>83.434000</td>
      <td>83.894000</td>
      <td>93.540000</td>
      <td>96.460000</td>
      <td>95.000000</td>
    </tr>
    <tr>
      <th>$586-615</th>
      <td>83.595000</td>
      <td>83.930000</td>
      <td>94.000000</td>
      <td>96.500000</td>
      <td>95.250000</td>
    </tr>
    <tr>
      <th>$616-645</th>
      <td>78.031667</td>
      <td>81.416667</td>
      <td>71.133333</td>
      <td>83.433333</td>
      <td>77.283333</td>
    </tr>
    <tr>
      <th>$645-675</th>
      <td>76.960000</td>
      <td>81.055000</td>
      <td>66.250000</td>
      <td>81.100000</td>
      <td>73.675000</td>
    </tr>
  </tbody>
</table>
</div>



## <font color=black> Scores By School Size </font>


```python
# Create the bins to hold Data
bins = [0, 1000, 2000, 5000]

# Create the names for the four bins created above
size_ranges = ["Small (0-1000)", "Medium (1000-2000)", "Large (2000-5000)"]

schools_size_df = school_summary_table
schools_size_df["School Size"] = pd.cut(student_counts, bins=bins, labels=size_ranges)

# Arrange scores by columns
spending_math_scores = schools_size_df.groupby(["School Size"])["Average Math Score"].mean()
spending_reading_scores = schools_size_df.groupby(["School Size"])["Average Reading Score"].mean()
spending_passing_math = schools_size_df.groupby(["School Size"])["% Passing Math"].mean()
spending_passing_reading = schools_size_df.groupby(["School Size"])["% Passing Reading"].mean()
overall_passing_rate = (spending_math_scores + spending_reading_scores) / 2

scores_by_size = school_summary_table[["School Size", "Average Math Score","Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
scores_by_size = scores_by_size.groupby("School Size").mean()

scores_by_size
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small (0-1000)</th>
      <td>83.820</td>
      <td>83.92500</td>
      <td>93.5500</td>
      <td>96.10</td>
      <td>94.82500</td>
    </tr>
    <tr>
      <th>Medium (1000-2000)</th>
      <td>83.374</td>
      <td>83.86800</td>
      <td>93.6000</td>
      <td>96.78</td>
      <td>95.19000</td>
    </tr>
    <tr>
      <th>Large (2000-5000)</th>
      <td>77.745</td>
      <td>81.34375</td>
      <td>69.9875</td>
      <td>82.75</td>
      <td>76.36875</td>
    </tr>
  </tbody>
</table>
</div>



## <font color=black> Scores By School Type </font>


```python
# Using groupby, arrange scores by school type
scores_school_type = school_summary_table[["School Type","Average Math Score","Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
scores_school_type = scores_school_type.groupby('School Type').mean()
scores_school_type
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.472500</td>
      <td>83.897500</td>
      <td>93.625000</td>
      <td>96.575000</td>
      <td>95.100000</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.955714</td>
      <td>80.965714</td>
      <td>66.571429</td>
      <td>80.785714</td>
      <td>73.678571</td>
    </tr>
  </tbody>
</table>
</div>


