
# Academy of Py - School District Analysis



```python
#import dependencies

import pandas as pd
import numpy as np
```

## District Summary

Create a high level snapshot (in table form) of the district's key metrics, including:
+ Total Schools
+ Total Students
+ Total Budget
+ Average Math Score
+ Average Reading Score
+ % Passing Math
+ % Passing Reading
+ Overall Passing Rate (Average of the above two)


```python
#files to resources
school_dir=("Resources/schools_complete.csv")

students_dir=("Resources/students_complete.csv")

#read files
schools_complete=pd.read_csv(school_dir)
students_complete=pd.read_csv(students_dir)

students_complete.head()
schools_complete.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
#count total students
total_students=len(students_complete["name"].unique())
total_students

#count total schools
total_schools=len(schools_complete["name"].unique())
total_schools

#add total school budgets
total_budget=schools_complete["budget"].sum()
total_budget
print(str("total students:") + str(total_students) + str(" ; ") +
      str("total schools:") + str(total_schools) + str(" ; ") + 
      str("total budget:" + str(total_budget)))
```

    total students:32715 ; total schools:15 ; total budget:24649428



```python
#Print student table
students_complete.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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




```python
#school score averages: Math, Reading

average_math=students_complete["math_score"].mean()
average_math

average_english=students_complete["reading_score"].mean()
average_english

#percentage passing math and english

pass_math=students_complete.loc[(students_complete['math_score']>=70),
                               ['Student ID', 'name', 'gender', 'grade', 'school', 'reading_score','math_score']]
percent_math=(len(pass_math["name"].unique())/total_students)*100
percent_math

pass_english=students_complete.loc[(students_complete["reading_score"]>=70),['Student ID', 'name', 'gender', 'grade', 'school', 'reading_score',
       'math_score']]
percent_english=(len(pass_english["name"].unique())/total_students)*100
percent_english

print(str("Students Passed English:")+ str(percent_english) + str("% ;" ),
     str("Students Passed Math:")+ str(percent_math) + str("%"))

average_two=((percent_math+ percent_english)/2)
average_two

print(str("average of the two: " + str(average_two) + "%"))
```

    Students Passed English:87.38193489225125% ; Students Passed Math:77.71970044322177%
    average of the two: 82.55081766773651%



```python
#rename schools_complete to column ="school" instead of column ="name"

schools_renamed=schools_complete.rename(columns={"name":"school"})


```


```python
#create dataframe

district_summary=pd.DataFrame({ "Total Students": [total_students],
                               "Total Schools" : [total_schools],
                               "Total Budget" : [total_budget],
                               "Average Math Score":[average_math],
                               "Average Reading Score":[average_english],
                               "% Passing Math" :[percent_math],
                               "% Passing Reading":[percent_english],
                               "% Overall Passing":[average_two]})
district_summary[["Total Students","Total Schools","Total Budget","Average Math Score","Average Reading Score",
                 "% Passing Math", "% Passing Reading","% Overall Passing"]]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Students</th>
      <th>Total Schools</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>32715</td>
      <td>15</td>
      <td>24649428</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>77.7197</td>
      <td>87.381935</td>
      <td>82.550818</td>
    </tr>
  </tbody>
</table>
</div>



## School Summary

#### Create an overview table that summarizes key metrics about each school, including:
+ School Name
+ School Type
+ Total Students
+ Total School Budget
+ Per Student Budget
+ Average Math Score
+ Average Reading Score
+ % Passing Math
+ % Passing Reading
+ Overall Passing Rate (Average of the above two)


```python
#merge tables
students_schools=pd.merge(schools_renamed,students_complete,on="school",how="outer")
students_schools.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>reading_score</th>
      <th>math_score</th>
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
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>




```python
#list of schools
school_names=students_schools["school"].unique()

#list of school type
school_type=students_schools.groupby("school")["type"].unique().str.get(0)

#total students per school
student_count=students_schools.groupby("school")["name"].count()


#total budget per school
school_budget=students_schools.groupby("school")["budget"].unique().astype(float)

#budget per student
per_student_budget=(school_budget/student_count).astype(float)

#average math and reading score for each school
reading_school_avg=(students_schools.groupby("school")["reading_score"].mean()).astype(float)
math_school_avg=(students_schools.groupby("school")["math_score"].mean()).astype(float)

```


```python
# % passing math 
pass_math_school=students_schools.loc[(students_complete['math_score']>=70)].groupby("school").count()
percent_students_math_school= ((pass_math_school["math_score"]/student_count)*100).astype(float)
percent_students_math_school

#% Passing Reading
pass_reading_school=students_schools.loc[(students_complete['reading_score']>=70)].groupby("school").count()
percent_students_reading_school= ((pass_math_school["reading_score"]/student_count)*100).astype(float)
percent_students_reading_school

# Overall Passing Rate (Average of the above two)
overall_passing_schools=((percent_students_math_school+percent_students_reading_school)/2).astype(float)
```


```python
#create school overview summary
#need to fix the budget into $ and .00

school_summary_df=pd.DataFrame ({"Type":school_type,
                                "Student Count":student_count,
                                "School Budget": school_budget,
                                "School Budget per Student":per_student_budget,
                                "Average Reading Score":reading_school_avg,
                                "Average Math Score":math_school_avg,
                                "% Students Passing Math":percent_students_math_school,
                                "% Students Passing English":percent_students_reading_school,
                                "Overall Passing Average":overall_passing_schools})

school_summary_df[["Type","Student Count","School Budget",
                   "School Budget per Student","Average Reading Score","Average Math Score",
                  "% Students Passing Math","% Students Passing English","Overall Passing Average"]]
#turn into dolla sign
school_summary_df["School Budget"] = school_summary_df["School Budget"].map("${:.2f}".format)

school_summary_df["School Budget per Student"] = school_summary_df["School Budget per Student"]#.map("${:.2f}".format)
school_summary_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Students Passing English</th>
      <th>% Students Passing Math</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Passing Average</th>
      <th>School Budget</th>
      <th>School Budget per Student</th>
      <th>Student Count</th>
      <th>Type</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>66.680064</td>
      <td>66.680064</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>66.680064</td>
      <td>$3124928.00</td>
      <td>628.0</td>
      <td>4976</td>
      <td>District</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>94.133477</td>
      <td>94.133477</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>$1081356.00</td>
      <td>582.0</td>
      <td>1858</td>
      <td>Charter</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>65.988471</td>
      <td>65.988471</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>$1884411.00</td>
      <td>639.0</td>
      <td>2949</td>
      <td>District</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>68.309602</td>
      <td>68.309602</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>$1763916.00</td>
      <td>644.0</td>
      <td>2739</td>
      <td>District</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>93.392371</td>
      <td>93.392371</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>$917500.00</td>
      <td>625.0</td>
      <td>1468</td>
      <td>Charter</td>
    </tr>
  </tbody>
</table>
</div>



## Top Performing Schools (By Passing Rate)

Table that highlights the ** top 5 performing schools ** based on * Overall Passing Rate * Based on set characteristics
+ School Name
+ School Type
+ Total Students
+ Total School Budget
+ Per Student Budget
+ Average Math Score
+ Average Reading Score
+ % Passing Math
+ % Passing Reading
+ Overall Passing Rate (Average of the above two)


```python
overall_passing_top=school_summary_df.sort_values("Overall Passing Average",ascending=False)[["Type","Student Count","School Budget",
                   "School Budget per Student","Average Reading Score","Average Math Score",
                  "% Students Passing Math","% Students Passing English","Overall Passing Average"]]
overall_passing_top.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Student Count</th>
      <th>School Budget</th>
      <th>School Budget per Student</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Students Passing Math</th>
      <th>% Students Passing English</th>
      <th>Overall Passing Average</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585858.00</td>
      <td>609.0</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>94.594595</td>
      <td>94.594595</td>
      <td>94.594595</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$1081356.00</td>
      <td>582.0</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>94.133477</td>
      <td>94.133477</td>
      <td>94.133477</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$1319574.00</td>
      <td>578.0</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.867718</td>
      <td>93.867718</td>
      <td>93.867718</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>$1056600.00</td>
      <td>600.0</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>93.867121</td>
      <td>93.867121</td>
      <td>93.867121</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>$917500.00</td>
      <td>625.0</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>93.392371</td>
      <td>93.392371</td>
    </tr>
  </tbody>
</table>
</div>



## Bottom Performing Schools (By Passing Rate)

Create a table that highlights the ** bottom 5 performing schools ** based on * Overall Passing Rate *. Include all of the same metrics as above.


```python
overall_passing_top=school_summary_df.sort_values("Overall Passing Average")[["Type","Student Count","School Budget",
                   "School Budget per Student","Average Reading Score","Average Math Score",
                  "% Students Passing Math","% Students Passing English","Overall Passing Average"]]
overall_passing_top.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Student Count</th>
      <th>School Budget</th>
      <th>School Budget per Student</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Students Passing Math</th>
      <th>% Students Passing English</th>
      <th>Overall Passing Average</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$1910635.00</td>
      <td>655.0</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>65.683922</td>
      <td>65.683922</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$1884411.00</td>
      <td>639.0</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>65.988471</td>
      <td>65.988471</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$3094650.00</td>
      <td>650.0</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>66.057551</td>
      <td>66.057551</td>
      <td>66.057551</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$2547363.00</td>
      <td>637.0</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>66.366592</td>
      <td>66.366592</td>
      <td>66.366592</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>$3124928.00</td>
      <td>628.0</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>66.680064</td>
      <td>66.680064</td>
      <td>66.680064</td>
    </tr>
  </tbody>
</table>
</div>



# Math Scores by Grade

+ Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.


```python
#turn math score and reading score to float!

students_schools['math_score']=students_schools['math_score'].astype(float)

#Calculate Math Score by Grade Level

score_grade_df=students_schools[["school","grade","math_score","reading_score"]]
math_score_grade=score_grade_df.groupby(["school","grade"])[["math_score"]].mean()

math_score_grade.unstack()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">math_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
      <td>77.083676</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
      <td>83.094697</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
      <td>76.403037</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
      <td>77.361345</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
      <td>82.044010</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
      <td>77.438495</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
      <td>83.787402</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
      <td>77.027251</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
      <td>77.187857</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
      <td>83.625455</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
      <td>76.859966</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
      <td>83.420755</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
      <td>83.590022</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
      <td>83.085578</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
      <td>83.264706</td>
    </tr>
  </tbody>
</table>
</div>



# Reading Score by Grade


```python
score_grade_df=students_schools[["school","grade","math_score","reading_score"]]
reading_score_grade=score_grade_df.groupby(["school","grade"])[["reading_score"]].mean()

reading_score_grade.unstack()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">reading_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
      <td>81.303155</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
      <td>83.676136</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
      <td>81.198598</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
      <td>80.632653</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
      <td>83.369193</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
      <td>80.866860</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
      <td>83.677165</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
      <td>81.290284</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
      <td>81.260714</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
      <td>83.807273</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
      <td>80.993127</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
      <td>84.122642</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
      <td>83.728850</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
      <td>83.939778</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
      <td>83.833333</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Spending

Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. Include in the table each of the following:
+ Average Math Score
+ Average Reading Score
+ % Passing Math
+ % Passing Reading
+ Overall Passing Rate (Average of the above two)


```python
group_name=["$570-$610","$610-$650","> $650"]
bins=np.arange(570,700,40)
#bins= [570, 610, 650, 690]


school_spending=pd.cut(school_summary_df["School Budget per Student"],bins,labels=group_name)
school_spending_df=pd.DataFrame({"School Spending":school_spending,
                                "Average Math Score":math_school_avg,
                                "Average Reading Score":reading_school_avg,
                                "% Students Passing Math":percent_students_math_school,
                                "% Students Passing English":percent_students_reading_school,
                                "Overall Passing Average":overall_passing_schools
                                })
school_spending_df=school_spending_df.reset_index().drop("school",1)
school_spending_df.groupby("School Spending").mean()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Students Passing English</th>
      <th>% Students Passing Math</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Passing Average</th>
    </tr>
    <tr>
      <th>School Spending</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>$570-$610</th>
      <td>93.717016</td>
      <td>93.717016</td>
      <td>83.503495</td>
      <td>83.917613</td>
      <td>93.717016</td>
    </tr>
    <tr>
      <th>$610-$650</th>
      <td>74.295260</td>
      <td>74.295260</td>
      <td>78.792545</td>
      <td>81.759287</td>
      <td>74.295260</td>
    </tr>
    <tr>
      <th>&gt; $650</th>
      <td>66.218444</td>
      <td>66.218444</td>
      <td>76.959583</td>
      <td>81.058567</td>
      <td>66.218444</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Size

Group schools based on a reasonable approximation of school size (Small, Medium, Large).


```python
bins=[0,1000,3000,10000]
bin_group=["Small (< 1000)","Medium (1000 - 3000)","Large (>3000)"]
score_school_size= pd.cut(school_summary_df["Student Count"],bins,labels=bin_group)

score_school_df=school_summary_df.reset_index()
score_school_df=score_school_df[["Average Math Score","Average Reading Score","% Students Passing Math",
                                 "% Students Passing English","Overall Passing Average"]]

#initiate new column by adding 'school size' onto dataframe
score_school_df['School Size']=score_school_size.values

score_school_final_df=score_school_df.groupby("School Size").mean()
score_school_final_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Students Passing Math</th>
      <th>% Students Passing English</th>
      <th>Overall Passing Average</th>
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
      <th>Small (&lt; 1000)</th>
      <td>83.821598</td>
      <td>83.929843</td>
      <td>93.550225</td>
      <td>93.550225</td>
      <td>93.550225</td>
    </tr>
    <tr>
      <th>Medium (1000 - 3000)</th>
      <td>81.176821</td>
      <td>82.933187</td>
      <td>84.649798</td>
      <td>84.649798</td>
      <td>84.649798</td>
    </tr>
    <tr>
      <th>Large (&gt;3000)</th>
      <td>77.063340</td>
      <td>80.919864</td>
      <td>66.464293</td>
      <td>66.464293</td>
      <td>66.464293</td>
    </tr>
  </tbody>
</table>
</div>



## Scores by School Type 

Charter vs. District


```python
scores_type= school_summary_df[['% Students Passing English', '% Students Passing Math',
       'Average Math Score', 'Average Reading Score',
       'Overall Passing Average','Type']]
scores_type=scores_type.reset_index()[['% Students Passing English', '% Students Passing Math',
       'Average Math Score', 'Average Reading Score',
       'Overall Passing Average','Type']]
scores_type_df=scores_type.groupby("Type").mean()
scores_type_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Students Passing English</th>
      <th>% Students Passing Math</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Passing Average</th>
    </tr>
    <tr>
      <th>Type</th>
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
      <td>93.620830</td>
      <td>93.620830</td>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>93.620830</td>
    </tr>
    <tr>
      <th>District</th>
      <td>66.548453</td>
      <td>66.548453</td>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>66.548453</td>
    </tr>
  </tbody>
</table>
</div>



## Conclusions:

Observable trends based on findings: 

1. Out of 15 schools, the top performing schools were all charter schools while district schools had the lower overall passing average. The student population for the charter schools range from small to medium school sizes. 
2. The school budget per student doesn't seem to improve the overall passing average for the schools. The schools with lower budget per student has the highest overall passing average while schools with a higher budget per student has a lower overall passing grade. 
3. Small school sizes with less than 1000 students tend to score higher in their standardized tests than larger school sizes with more than 3000 students. This may be due to greater focus on individual students
4. Charter schools were smaller in size and had on average a lower student budget compared to district schools. Based on the data collected,there seems to be a negative correlation between school size and overall passing average. There is also a negative correlation between the budget per student and overall passing average. 

Increasing budget funding doesn't appear to improve overall passing average in schools. Factors that were not recorded, such as quality of teaching, may affect overall performance. I would recommend proritizing the school budget to hire quality instructors and see if this may immprove passing average.  

