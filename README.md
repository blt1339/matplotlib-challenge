## Matplotlib Homework - The Power of Plots
![Laboratory](Images/Laboratory.jpg)

# Background
[Project Details](project_instructions_readme.md)

## Observations and Insights 

* The number of mice utilized for each drug regimen is consistant at 24 or 25 mice. But the actual datapoints varies between 148 and 230.
* There is a positive correlation between Mouse Weight and Tumor Volume.   As the mouse weight increases so does the tumor volume.
* The two best drugs in terms of last tumor volume are Capomulin and Ramicane.  

|Drug Regimen|All Data Points Average Tumor Volume|Last Average Tumor Volume|
|------------|------------------------------------|-------------------------|
|Capomulin   |         	                     40.68|	                   36.67|
|Ceftamin	 |                               52.60|	                   57.75|
|Infubinol   |                               52.89|                    58.18|
|Ketapril    |                               55.24|	                   62.81|
|Naftisol	 |                               54.33|	                   61.21|
|Placebo 	 |                               54.03|	                   60.51|
|Propriva	 |                               52.32|	                   56.49|
|Ramicane	 |                               40.22|	                   36.19|
|Stelasyn	 |                               54.23|	                   61.00|
|Zoniferol   |                               53.23|	                   59.18|
|All Drugs   |                               50.98|	                   54.97|

This trend continues when a random mouse was investigaged for each of the 4 suggested Drug Regimens

![line_chart](Images/random_mouse_line.jpg)

* The mice are evenly split between Male (50.40 %) and Female (49.60 %)


```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import scipy.stats as st
from scipy.stats import linregress
import matplotlib.patches as mpatches

# Setup paths to files
mouse_metadata_path = "data/Mouse_metadata.csv"
study_results_path = "data/Study_results.csv"

# Read the mouse data and the study results
mouse_metadata_df = pd.read_csv(mouse_metadata_path)
study_results_df = pd.read_csv(study_results_path)

# Combine the data into a single dataset
combined_mouse_df = pd.merge(mouse_metadata_df, study_results_df, how='outer', on='Mouse ID')

# Display the data table for preview
combined_mouse_df
```




<div>
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
      <th>Mouse ID</th>
      <th>Drug Regimen</th>
      <th>Sex</th>
      <th>Age_months</th>
      <th>Weight (g)</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>0</td>
      <td>45.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>5</td>
      <td>38.825898</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>10</td>
      <td>35.014271</td>
      <td>1</td>
    </tr>
    <tr>
      <td>3</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>15</td>
      <td>34.223992</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>20</td>
      <td>32.997729</td>
      <td>1</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>1888</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>25</td>
      <td>63.145652</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1889</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>30</td>
      <td>65.841013</td>
      <td>3</td>
    </tr>
    <tr>
      <td>1890</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>35</td>
      <td>69.176246</td>
      <td>4</td>
    </tr>
    <tr>
      <td>1891</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>40</td>
      <td>70.314904</td>
      <td>4</td>
    </tr>
    <tr>
      <td>1892</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>45</td>
      <td>73.867845</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>1893 rows × 8 columns</p>
</div>



| # of Purchases |  # of Users |
|----------------|-------------|
|               1|          414|
|               2|          124|
|               3|           35|
|               4|            2|
|               5|            1|


```python
# Checking the number of mice
orig_no_mice = len(pd.unique(combined_mouse_df['Mouse ID']))
orig_no_mice
```




    249




```python
# Getting the duplicate mice by ID number that shows up for Mouse ID and Timepoint. 
count_series = combined_mouse_df.groupby(['Mouse ID','Timepoint']).size()

# Turn back into a data frame so it is easier to work with
count_df = count_series.to_frame(name = 'size').reset_index()

# Sort the data and create a duplicates data frame of the duplicate records
sorted_count_df = count_df.sort_values('size', ascending=False)
dupicates_count_mouse_df = sorted_count_df.loc[sorted_count_df['size'] > 1]

# Output the duplicates to identify the mice that have duplicates
dupicates_count_mouse_df
```




<div>
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
      <th>Mouse ID</th>
      <th>Timepoint</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>590</td>
      <td>g989</td>
      <td>20</td>
      <td>2</td>
    </tr>
    <tr>
      <td>589</td>
      <td>g989</td>
      <td>15</td>
      <td>2</td>
    </tr>
    <tr>
      <td>588</td>
      <td>g989</td>
      <td>10</td>
      <td>2</td>
    </tr>
    <tr>
      <td>587</td>
      <td>g989</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <td>586</td>
      <td>g989</td>
      <td>0</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Optional: Get all the data for the duplicate mouse ID. 
duplicates_combined_mouse_df = combined_mouse_df.loc[combined_mouse_df['Mouse ID'] == 'g989']

# Output the records that for mouse that has duplicates
duplicates_combined_mouse_df
```




<div>
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
      <th>Mouse ID</th>
      <th>Drug Regimen</th>
      <th>Sex</th>
      <th>Age_months</th>
      <th>Weight (g)</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>908</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>0</td>
      <td>45.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <td>909</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>0</td>
      <td>45.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <td>910</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>5</td>
      <td>48.786801</td>
      <td>0</td>
    </tr>
    <tr>
      <td>911</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>5</td>
      <td>47.570392</td>
      <td>0</td>
    </tr>
    <tr>
      <td>912</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>10</td>
      <td>51.745156</td>
      <td>0</td>
    </tr>
    <tr>
      <td>913</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>10</td>
      <td>49.880528</td>
      <td>0</td>
    </tr>
    <tr>
      <td>914</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>15</td>
      <td>51.325852</td>
      <td>1</td>
    </tr>
    <tr>
      <td>915</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>15</td>
      <td>53.442020</td>
      <td>0</td>
    </tr>
    <tr>
      <td>916</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>20</td>
      <td>55.326122</td>
      <td>1</td>
    </tr>
    <tr>
      <td>917</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>20</td>
      <td>54.657650</td>
      <td>1</td>
    </tr>
    <tr>
      <td>918</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>25</td>
      <td>56.045564</td>
      <td>1</td>
    </tr>
    <tr>
      <td>919</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>30</td>
      <td>59.082294</td>
      <td>1</td>
    </tr>
    <tr>
      <td>920</td>
      <td>g989</td>
      <td>Propriva</td>
      <td>Female</td>
      <td>21</td>
      <td>26</td>
      <td>35</td>
      <td>62.570880</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a clean DataFrame by dropping the duplicate mouse by its ID.
cleaned_combined_mouse_df = combined_mouse_df[combined_mouse_df['Mouse ID'] != 'g989']

# Preview the cleaned data
cleaned_combined_mouse_df
```




<div>
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
      <th>Mouse ID</th>
      <th>Drug Regimen</th>
      <th>Sex</th>
      <th>Age_months</th>
      <th>Weight (g)</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>0</td>
      <td>45.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>5</td>
      <td>38.825898</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>10</td>
      <td>35.014271</td>
      <td>1</td>
    </tr>
    <tr>
      <td>3</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>15</td>
      <td>34.223992</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4</td>
      <td>k403</td>
      <td>Ramicane</td>
      <td>Male</td>
      <td>21</td>
      <td>16</td>
      <td>20</td>
      <td>32.997729</td>
      <td>1</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>1888</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>25</td>
      <td>63.145652</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1889</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>30</td>
      <td>65.841013</td>
      <td>3</td>
    </tr>
    <tr>
      <td>1890</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>35</td>
      <td>69.176246</td>
      <td>4</td>
    </tr>
    <tr>
      <td>1891</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>40</td>
      <td>70.314904</td>
      <td>4</td>
    </tr>
    <tr>
      <td>1892</td>
      <td>z969</td>
      <td>Naftisol</td>
      <td>Male</td>
      <td>9</td>
      <td>30</td>
      <td>45</td>
      <td>73.867845</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>1880 rows × 8 columns</p>
</div>




```python
# Checking the number of mice in the clean DataFrame.
no_mice = len(pd.unique(cleaned_combined_mouse_df['Mouse ID']))

# Output the new number of mice (should be previous count - 1 (249-1))
no_mice
```




    248



## Summary Statistics


```python
# Generate a summary statistics table of mean, median, variance, standard deviation, and SEM of the tumor volume for each regimen
# This method is the most straighforward, creating multiple series and putting tregimen_grouped = cleaned_combined_mouse_df.groupby(["Drug Regimen"])

# Group the data by Drug Regimen
grouped_mouse_data = cleaned_combined_mouse_df.groupby('Drug Regimen')

# Perform the statistical calculates
tumor_volume_mean = grouped_mouse_data['Tumor Volume (mm3)'].mean()
tumor_volume_median = grouped_mouse_data['Tumor Volume (mm3)'].median()
tumor_volume_variance = grouped_mouse_data['Tumor Volume (mm3)'].var()
tumor_volume_std = grouped_mouse_data['Tumor Volume (mm3)'].std()
tumor_volume_sem = grouped_mouse_data['Tumor Volume (mm3)'].sem()

# Create a dataframe with all of our summary information
summary_statistics_df = pd.DataFrame({"Mean":tumor_volume_mean,
                                "Median":tumor_volume_median,
                                "Variance":tumor_volume_variance,
                                "Standard Deviation":tumor_volume_std,
                                "Standard Error of Measurement":tumor_volume_sem
                                 })
# Preview the statistics summary information
summary_statistics_df
```




<div>
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
      <th>Mean</th>
      <th>Median</th>
      <th>Variance</th>
      <th>Standard Deviation</th>
      <th>Standard Error of Measurement</th>
    </tr>
    <tr>
      <th>Drug Regimen</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Capomulin</td>
      <td>40.675741</td>
      <td>41.557809</td>
      <td>24.947764</td>
      <td>4.994774</td>
      <td>0.329346</td>
    </tr>
    <tr>
      <td>Ceftamin</td>
      <td>52.591172</td>
      <td>51.776157</td>
      <td>39.290177</td>
      <td>6.268188</td>
      <td>0.469821</td>
    </tr>
    <tr>
      <td>Infubinol</td>
      <td>52.884795</td>
      <td>51.820584</td>
      <td>43.128684</td>
      <td>6.567243</td>
      <td>0.492236</td>
    </tr>
    <tr>
      <td>Ketapril</td>
      <td>55.235638</td>
      <td>53.698743</td>
      <td>68.553577</td>
      <td>8.279709</td>
      <td>0.603860</td>
    </tr>
    <tr>
      <td>Naftisol</td>
      <td>54.331565</td>
      <td>52.509285</td>
      <td>66.173479</td>
      <td>8.134708</td>
      <td>0.596466</td>
    </tr>
    <tr>
      <td>Placebo</td>
      <td>54.033581</td>
      <td>52.288934</td>
      <td>61.168083</td>
      <td>7.821003</td>
      <td>0.581331</td>
    </tr>
    <tr>
      <td>Propriva</td>
      <td>52.320930</td>
      <td>50.446266</td>
      <td>43.852013</td>
      <td>6.622085</td>
      <td>0.544332</td>
    </tr>
    <tr>
      <td>Ramicane</td>
      <td>40.216745</td>
      <td>40.673236</td>
      <td>23.486704</td>
      <td>4.846308</td>
      <td>0.320955</td>
    </tr>
    <tr>
      <td>Stelasyn</td>
      <td>54.233149</td>
      <td>52.431737</td>
      <td>59.450562</td>
      <td>7.710419</td>
      <td>0.573111</td>
    </tr>
    <tr>
      <td>Zoniferol</td>
      <td>53.236507</td>
      <td>51.818479</td>
      <td>48.533355</td>
      <td>6.966589</td>
      <td>0.516398</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Generate a summary statistics table of mean, median, variance, standard deviation, and SEM of the tumor volume for each regimen
# This method produces everything in a single groupby function
summary_statistics_df = cleaned_combined_mouse_df.groupby('Drug Regimen') \
                            .agg([('Mean', 'mean'),
                                  ("Median",'median'),
                                  ("Variance",'var'),
                                  ("Standard Deviation",'sem'),
                                  ("Standard Error of Measurement",'sem')
                                 ])

# Preview the summary statistics data
summary_statistics_df

```




<div>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="5" halign="left">Age_months</th>
      <th colspan="5" halign="left">Weight (g)</th>
      <th>...</th>
      <th colspan="5" halign="left">Tumor Volume (mm3)</th>
      <th colspan="5" halign="left">Metastatic Sites</th>
    </tr>
    <tr>
      <th></th>
      <th>Mean</th>
      <th>Median</th>
      <th>Variance</th>
      <th>Standard Deviation</th>
      <th>Standard Error of Measurement</th>
      <th>Mean</th>
      <th>Median</th>
      <th>Variance</th>
      <th>Standard Deviation</th>
      <th>Standard Error of Measurement</th>
      <th>...</th>
      <th>Mean</th>
      <th>Median</th>
      <th>Variance</th>
      <th>Standard Deviation</th>
      <th>Standard Error of Measurement</th>
      <th>Mean</th>
      <th>Median</th>
      <th>Variance</th>
      <th>Standard Deviation</th>
      <th>Standard Error of Measurement</th>
    </tr>
    <tr>
      <th>Drug Regimen</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
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
      <td>Capomulin</td>
      <td>13.456522</td>
      <td>16.5</td>
      <td>59.620372</td>
      <td>0.509136</td>
      <td>0.509136</td>
      <td>19.965217</td>
      <td>20.5</td>
      <td>7.466034</td>
      <td>0.180169</td>
      <td>0.180169</td>
      <td>...</td>
      <td>40.675741</td>
      <td>41.557809</td>
      <td>24.947764</td>
      <td>0.329346</td>
      <td>0.329346</td>
      <td>0.713043</td>
      <td>0</td>
      <td>0.720790</td>
      <td>0.055981</td>
      <td>0.055981</td>
    </tr>
    <tr>
      <td>Ceftamin</td>
      <td>13.247191</td>
      <td>12.0</td>
      <td>65.147591</td>
      <td>0.604977</td>
      <td>0.604977</td>
      <td>27.398876</td>
      <td>28.0</td>
      <td>2.501016</td>
      <td>0.118535</td>
      <td>0.118535</td>
      <td>...</td>
      <td>52.591172</td>
      <td>51.776157</td>
      <td>39.290177</td>
      <td>0.469821</td>
      <td>0.469821</td>
      <td>1.179775</td>
      <td>1</td>
      <td>1.402527</td>
      <td>0.088766</td>
      <td>0.088766</td>
    </tr>
    <tr>
      <td>Infubinol</td>
      <td>16.230337</td>
      <td>20.0</td>
      <td>56.404272</td>
      <td>0.562919</td>
      <td>0.562919</td>
      <td>27.196629</td>
      <td>27.0</td>
      <td>4.769028</td>
      <td>0.163684</td>
      <td>0.163684</td>
      <td>...</td>
      <td>52.884795</td>
      <td>51.820584</td>
      <td>43.128684</td>
      <td>0.492236</td>
      <td>0.492236</td>
      <td>0.960674</td>
      <td>1</td>
      <td>1.054942</td>
      <td>0.076985</td>
      <td>0.076985</td>
    </tr>
    <tr>
      <td>Ketapril</td>
      <td>15.659574</td>
      <td>18.0</td>
      <td>36.236432</td>
      <td>0.439030</td>
      <td>0.439030</td>
      <td>27.861702</td>
      <td>28.0</td>
      <td>3.392536</td>
      <td>0.134333</td>
      <td>0.134333</td>
      <td>...</td>
      <td>55.235638</td>
      <td>53.698743</td>
      <td>68.553577</td>
      <td>0.603860</td>
      <td>0.603860</td>
      <td>1.297872</td>
      <td>1</td>
      <td>1.942883</td>
      <td>0.101659</td>
      <td>0.101659</td>
    </tr>
    <tr>
      <td>Naftisol</td>
      <td>12.000000</td>
      <td>9.0</td>
      <td>45.102703</td>
      <td>0.492430</td>
      <td>0.492430</td>
      <td>27.166667</td>
      <td>27.0</td>
      <td>2.247748</td>
      <td>0.109930</td>
      <td>0.109930</td>
      <td>...</td>
      <td>54.331565</td>
      <td>52.509285</td>
      <td>66.173479</td>
      <td>0.596466</td>
      <td>0.596466</td>
      <td>1.182796</td>
      <td>1</td>
      <td>1.479919</td>
      <td>0.089200</td>
      <td>0.089200</td>
    </tr>
    <tr>
      <td>Placebo</td>
      <td>10.734807</td>
      <td>10.0</td>
      <td>40.384837</td>
      <td>0.472356</td>
      <td>0.472356</td>
      <td>27.928177</td>
      <td>28.0</td>
      <td>3.378146</td>
      <td>0.136615</td>
      <td>0.136615</td>
      <td>...</td>
      <td>54.033581</td>
      <td>52.288934</td>
      <td>61.168083</td>
      <td>0.581331</td>
      <td>0.581331</td>
      <td>1.441989</td>
      <td>1</td>
      <td>1.792449</td>
      <td>0.099514</td>
      <td>0.099514</td>
    </tr>
    <tr>
      <td>Propriva</td>
      <td>10.006757</td>
      <td>7.5</td>
      <td>48.251655</td>
      <td>0.570986</td>
      <td>0.570986</td>
      <td>27.135135</td>
      <td>26.0</td>
      <td>2.933995</td>
      <td>0.140799</td>
      <td>0.140799</td>
      <td>...</td>
      <td>52.320930</td>
      <td>50.446266</td>
      <td>43.852013</td>
      <td>0.544332</td>
      <td>0.544332</td>
      <td>1.013514</td>
      <td>1</td>
      <td>1.224306</td>
      <td>0.090952</td>
      <td>0.090952</td>
    </tr>
    <tr>
      <td>Ramicane</td>
      <td>10.684211</td>
      <td>9.0</td>
      <td>35.362393</td>
      <td>0.393825</td>
      <td>0.393825</td>
      <td>19.679825</td>
      <td>19.0</td>
      <td>10.465318</td>
      <td>0.214244</td>
      <td>0.214244</td>
      <td>...</td>
      <td>40.216745</td>
      <td>40.673236</td>
      <td>23.486704</td>
      <td>0.320955</td>
      <td>0.320955</td>
      <td>0.548246</td>
      <td>0</td>
      <td>0.477838</td>
      <td>0.045780</td>
      <td>0.045780</td>
    </tr>
    <tr>
      <td>Stelasyn</td>
      <td>12.784530</td>
      <td>14.0</td>
      <td>63.036648</td>
      <td>0.590143</td>
      <td>0.590143</td>
      <td>27.856354</td>
      <td>28.0</td>
      <td>2.701473</td>
      <td>0.122169</td>
      <td>0.122169</td>
      <td>...</td>
      <td>54.233149</td>
      <td>52.431737</td>
      <td>59.450562</td>
      <td>0.573111</td>
      <td>0.573111</td>
      <td>0.872928</td>
      <td>1</td>
      <td>0.944874</td>
      <td>0.072252</td>
      <td>0.072252</td>
    </tr>
    <tr>
      <td>Zoniferol</td>
      <td>12.598901</td>
      <td>12.5</td>
      <td>33.479115</td>
      <td>0.428895</td>
      <td>0.428895</td>
      <td>27.692308</td>
      <td>28.0</td>
      <td>2.015300</td>
      <td>0.105229</td>
      <td>0.105229</td>
      <td>...</td>
      <td>53.236507</td>
      <td>51.818479</td>
      <td>48.533355</td>
      <td>0.516398</td>
      <td>0.516398</td>
      <td>1.230769</td>
      <td>1</td>
      <td>1.559711</td>
      <td>0.092573</td>
      <td>0.092573</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 25 columns</p>
</div>



## Bar and Pie Charts


```python
# Generate a bar plot showing the total number of mice for each treatment throughout the course of the study using pandas. 

# Group the data by Drug Regimen and then get the unique mice 
no_mice_data = cleaned_combined_mouse_df.groupby("Drug Regimen").nunique()['Mouse ID']

# Create the bar chart
no_mice_bar = no_mice_data.plot(kind='bar',figsize=(10,6))

# Setup the title and leabels
no_mice_bar.set_title("Number of Mice per Drug Regimen", fontsize=18)
no_mice_bar.set_xlabel("Treatment", fontsize=18)
no_mice_bar.set_ylabel("Number of Mice", fontsize=18);

# Get rid of the numbers along the y axis because we have them on each datapoint
no_mice_bar.set_yticklabels([])

# Set the y limit
no_mice_bar.set_ylim(0,28)

# Place the value on top of each Drug Regimen
for index,value in enumerate(no_mice_data):
    no_mice_bar.annotate(value,(index, value), xytext=(0, 2),textcoords='offset points')



```


![png](Images/output_13_0.png)



```python
# Generate a bar plot showing the total number of mice / timepoint combinations
# for each treatment throughout the course of the study using pandas. 
no_mice_data = cleaned_combined_mouse_df.groupby("Drug Regimen")['Timepoint'].count()

# Create the bar chart
no_mice_bar = no_mice_data.plot(kind='bar',figsize=(10,6))

# Setup title and labels
no_mice_bar.set_title("Number of Data Points per Drug Regimen", fontsize=18)
no_mice_bar.set_xlabel("Treatment", fontsize=18)
no_mice_bar.set_ylabel("Number of Data points", fontsize=18);

# Get rid of the numbers along the y axis because we have them on each datapoint
no_mice_bar.set_yticklabels([])

# Set the y limit
no_mice_bar.set_ylim(0,250)

# Place the value on top of each Drug Regimen
for index,value in enumerate(no_mice_data):
    no_mice_bar.annotate(value,(index, value), xytext=(0, 2),textcoords='offset points')



```


![png](Images/output_14_0.png)



```python
# Generate a bar plot showing the total number of mice for each treatment throughout the course of the study using pyplot.

# Group the data by Drug Regimen and then get the unique mice 
no_mice_data = cleaned_combined_mouse_df.groupby("Drug Regimen").nunique()['Mouse ID']

# Create the bar chart
f, ax = plt.subplots(figsize=(10,6)) 
plt.xdata = no_mice_data.plot.bar()
plt.xlabel('Drug Regimen', fontsize=18)
plt.ylabel('Number of Data Points', fontsize=18)
plt.yticks([])
plt.title('Number of Data Points per Drug Regimen', fontsize=18)
plt.ylim(0,28)

# Place the value on top of each Drug Regimen
for index,value in enumerate(no_mice_data):
    plt.annotate(value,(index, value), xytext=(0, 2),textcoords='offset points')
plt.show()



```


![png](Images/output_15_0.png)



```python
# Generate a bar plot showing number of mouse / timepoint combinations
# for each treatment regimen using pyplot
no_mice_data = cleaned_combined_mouse_df.groupby("Drug Regimen")['Timepoint'].count()

# Create the bar chart
f, ax = plt.subplots(figsize=(10,6)) 
plt.xdata = no_mice_data.plot.bar()

# Setup the title and labels
plt.xlabel('Drug Regimen', fontsize=18)
plt.ylabel('Number of Data Points', fontsize=18)

plt.yticks([])
plt.title('Number of Data Points per Drug Regimen', fontsize=18)

# Set the y limit
plt.ylim(0,250)
# Place the value on top of each Drug Regimen
for index,value in enumerate(no_mice_data):
    plt.annotate(value,(index, value), xytext=(0, 2),textcoords='offset points')

# Show the chart
plt.show()
```


![png](Images/output_16_0.png)



```python
# Generate a pie plot showing the distribution of female versus male mice using pandas

# Group the data by sex and then get the unique Mouse IDs
gender_data = cleaned_combined_mouse_df.groupby('Sex').nunique()['Mouse ID']

# Clear the name created by the groupby
gender_data.name = ''

# Create the pie chart
gender_data_pie = gender_data.plot(kind='pie',autopct='%.2f%%')
gender_data_pie.set_title('Mouse Gender Percents', fontsize=18)

```




    Text(0.5, 1.0, 'Mouse Gender Percents')




![png](Images/output_17_1.png)



```python
# Generate a pie plot showing the distribution of female versus male mice using pyplot

# Group the data by sex and then get the unique Mouse IDs
gender_data = cleaned_combined_mouse_df.groupby('Sex').nunique()['Mouse ID']


# Create the pie chart
plt.pie(gender_data,labels=gender_data.index,autopct='%.2f%%')
plt.title('Mouse Gender Percents', fontsize=18)

# Show the pie chart
plt.show()
```


![png](output_18_0.png)


## Quartiles, Outliers and Boxplots


```python
# Calculate the final tumor volume of each mouse across four of the treatment regimens:  
# Capomulin, Ramicane, Infubinol, and Ceftamin
```


```python
# Start by getting the last (greatest) timepoint for each mouse
last_records_df = cleaned_combined_mouse_df.groupby("Mouse ID").max()['Timepoint']

# Merge this group df with the original dataframe to get the tumor volume at the last timepoint
last_full_data_df = pd.merge(cleaned_combined_mouse_df,last_records_df,how='inner',on=('Mouse ID','Timepoint'))
```


```python
# Put treatments into a list for for loop (and later for plot labels)
drugs = cleaned_combined_mouse_df.groupby("Drug Regimen").nunique()['Drug Regimen'].index.tolist()
box_drugs = ['Capomulin','Ramicane','Infubinol', 'Ceftamin']
```


```python
# Locate the rows which contain mice on each drug and get the tumor volumes
tumor_vols =  last_full_data_df['Tumor Volume (mm3)']
   
# Create dataframes for the 4 identified Drug Regimens 
capomulin_records_df = last_full_data_df.loc[last_full_data_df['Drug Regimen'] == "Capomulin"]
ramicane_records_df = last_full_data_df.loc[last_full_data_df['Drug Regimen'] == "Ramicane"]
infubinol_records_df = last_full_data_df.loc[last_full_data_df['Drug Regimen'] == "Infubinol"]
ceftamin_records_df = last_full_data_df.loc[last_full_data_df['Drug Regimen'] == "Ceftamin"]

# Create the tumor vols data for each of the 4 identified Drug Regimens
capomulin_tumor_vols = capomulin_records_df['Tumor Volume (mm3)']
ramicane_tumor_vols = ramicane_records_df['Tumor Volume (mm3)']
infubinol_tumor_vols = infubinol_records_df['Tumor Volume (mm3)']
ceftamin_tumor_vols = ceftamin_records_df['Tumor Volume (mm3)']
```

# Create a function to analyze the last tumor volume for all drugs and for individual drugs


```python
def drug_regimen_analysis(drug='',type='average'):
    '''
    This funtion accepts a Drug Regimen (can accept null to perform for all Drug Regimens)
    and a type (average, outlier).  The outliers or the average are calculated and the output
    of the results are then performed.
    
    '''
    if drug == '':
        analysis_df = last_full_data_df
    else:
        analysis_df = last_full_data_df.loc[last_full_data_df['Drug Regimen'] == drug]
    
    # Calculate the average tumor volume
    last_average_tumor_volume = analysis_df['Tumor Volume (mm3)'].mean()    
        
    # Calculate the IQR and quantitatively determine if there are any potential outliers.
    quartiles = analysis_df['Tumor Volume (mm3)'].quantile([.25,.5,.75])
    lowerq = quartiles[0.25]
    upperq = quartiles[0.75]
    iqr = upperq-lowerq    
  
    # Calculate the lower and upper bounds
    lower_bound = lowerq - (1.5*iqr)
    upper_bound = upperq + (1.5*iqr)

    # See if there are any outliers
    lower_outliers_df = analysis_df.loc[analysis_df['Tumor Volume (mm3)'] < lower_bound]
    upper_outliers_df = analysis_df.loc[analysis_df['Tumor Volume (mm3)'] > upper_bound]

    # Output the results for all drug regimens
    if type == 'average':
        if drug == '':
            print(f'The average last tumor volume for all drug regimens is {round(last_average_tumor_volume,2)}')
        else:
            print(f'The average last tumor volume for {drug} is {round(last_average_tumor_volume,2)}')                  
    
    else:
        if drug == '':
            print('All Drug Regimens')
        else:
            print(drug)
        print('-----------------')
        print(f"Values below {round(lower_bound,2)} could be outliers.")
        print(f"Values above {round(upper_bound,2)} could be outliers.")
        print("\n")
        print(f'The rows that are lower outliers:')
        print('----------------------------------')
        if len(lower_outliers_df) == 0:
            print('No outliers found')
        else:
            print(lower_outliers_df)
        print("\n")
        print(f'The rows that are upper outliers:')
        print('----------------------------------')
        if len(upper_outliers_df) == 0:
            print('No outliers found')
        else:
            print(upper_outliers_df)       
    
```


```python
# Get the average last tumor volume for all Drug Regimens
drug_regimen_analysis(drug='',type='average')
```

    The average last tumor volume for all drug regimens is 54.97
    


```python
# Get the average last tumor volume for all Drug Regimens
for drug in drugs:
    drug_regimen_analysis(drug,'average')
```

    The average last tumor volume for Capomulin is 36.67
    The average last tumor volume for Ceftamin is 57.75
    The average last tumor volume for Infubinol is 58.18
    The average last tumor volume for Ketapril is 62.81
    The average last tumor volume for Naftisol is 61.21
    The average last tumor volume for Placebo is 60.51
    The average last tumor volume for Propriva is 56.49
    The average last tumor volume for Ramicane is 36.19
    The average last tumor volume for Stelasyn is 61.0
    The average last tumor volume for Zoniferol is 59.18
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers for 
# all Drug Regimens
drug_regimen_analysis(drug='',type='outliers')
```

    All Drug Regimens
    -----------------
    Values below 17.11 could be outliers.
    Values above 93.82 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Capomulin
drug = drugs[0]
drug_regimen_analysis(drug,type='outliers')

```

    Capomulin
    -----------------
    Values below 20.7 could be outliers.
    Values above 51.83 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Ceftamin
drug = drugs[1]
drug_regimen_analysis(drug,type='outliers')

```

    Ceftamin
    -----------------
    Values below 25.36 could be outliers.
    Values above 87.67 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Infubinol
drug = drugs[2]
drug_regimen_analysis(drug,type='outliers')
```

    Infubinol
    -----------------
    Values below 36.83 could be outliers.
    Values above 82.74 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
       Mouse ID Drug Regimen     Sex  Age_months  Weight (g)  Timepoint  \
    74     c326    Infubinol  Female          18          25          5   
    
        Tumor Volume (mm3)  Metastatic Sites  
    74           36.321346                 0  
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Ketapril
drug = drugs[3]
drug_regimen_analysis(drug,type='outliers')
```

    Ketapril
    -----------------
    Values below 36.99 could be outliers.
    Values above 89.6 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Naftisol
drug = drugs[4]
drug_regimen_analysis(drug,type='outliers')
```

    Naftisol
    -----------------
    Values below 25.85 could be outliers.
    Values above 95.79 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Placebo
drug = drugs[5]
drug_regimen_analysis(drug,type='outliers')
```

    Placebo
    -----------------
    Values below 30.16 could be outliers.
    Values above 90.92 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Propriva
drug_regimen_analysis(drug,type='outliers')
```

    Placebo
    -----------------
    Values below 30.16 could be outliers.
    Values above 90.92 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Ramicane
drug = drugs[7]
drug_regimen_analysis(drug,type='outliers')
```

    Ramicane
    -----------------
    Values below 17.91 could be outliers.
    Values above 54.31 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Stelasyn
drug = drugs[8]
drug_regimen_analysis(drug,type='outliers')
```

    Stelasyn
    -----------------
    Values below 27.54 could be outliers.
    Values above 94.04 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Calculate the IQR and quantitatively determine if there are any potential outliers 
# for Zoniferol
drug = drugs[9]
drug_regimen_analysis(drug,type='outliers')
```

    Zoniferol
    -----------------
    Values below 24.78 could be outliers.
    Values above 92.0 could be outliers.
    
    
    The rows that are lower outliers:
    ----------------------------------
    No outliers found
    
    
    The rows that are upper outliers:
    ----------------------------------
    No outliers found
    


```python
# Generate a box plot of the final tumor volume of each mouse across four Drug Regimens of interest

# Setup the marker
flierprops = dict(marker='D', markerfacecolor='green', markersize=12, linestyle='none')


# Create the box plot
box_plot_data=[capomulin_tumor_vols,ramicane_tumor_vols,infubinol_tumor_vols,ceftamin_tumor_vols]
f, ax = plt.subplots(figsize=(12,5))
drugs_box_plot = plt.boxplot(box_plot_data,labels=box_drugs,flierprops=flierprops, patch_artist=True)

# Set colors for each boxplot
colors = ['blue','red','orange','yellow']
for patch, color in zip(drugs_box_plot['boxes'], colors):
    patch.set_facecolor(color)

#'Capomulin', 'Ramicane', 'Infubinol', 'Ceftamin    
capomulin_patch = mpatches.Patch(color='blue', label='Capomulin')
ramicane_patch = mpatches.Patch(color='red', label='Ramicane')
infubinol_patch = mpatches.Patch(color='orange', label='Infubinol')
ceftamin_patch = mpatches.Patch(color='yellow', label='Ceftamin')

plt.legend(handles=[capomulin_patch,ramicane_patch,infubinol_patch,ceftamin_patch],edgecolor='blue',bbox_to_anchor=(1.05, 1), loc='upper left', borderaxespad=0.)    
# leg = plt.legend(box_drugs,edgecolor='blue',c=colors,bbox_to_anchor=(1.05, 1), loc='upper left', borderaxespad=0.)
# leg_set_labelcolor = colors

# Setup title and lables
plt.title('Final Tumor Volume for Capomulin, Ramicane, Infubinol, and Ceftamin\n', fontsize=18) 
plt.ylabel('Tumor Volume (mm3)', fontsize=18)

# Limit the chart
plt.ylim(10, 80)

# Show the box plot
plt.show()

```


![png](Images/output_39_0.png)



```python
box_drugs
```




    ['Capomulin', 'Ramicane', 'Infubinol', 'Ceftamin']



## Line and Scatter Plots


```python
# Generate a line plot of time point versus tumor volume for a mouse treated with Capomulin
# using a randomly selected mouse id
chosen_mouse = 'y793'
chosen_mouse_df = cleaned_combined_mouse_df.loc[cleaned_combined_mouse_df['Mouse ID'] == chosen_mouse]

# Setup the x and y axis
x_axis = chosen_mouse_df['Timepoint']
y_axis = chosen_mouse_df['Tumor Volume (mm3)']

# Create the line plot
plt.plot(x_axis, y_axis)

# Setup the title and lables
plt.title('Timepoint versus Tumor Volume for Mouse y793 (Capomulin)\n', fontsize=18)
plt.xlabel('Timepoint', fontsize=18)
plt.ylabel('Tumor Volume (mm3)', fontsize=18)

plt.show()
```


![png](Images/output_42_0.png)



```python
# Generate a line plot of time point versus tumor volume for a mouse treated with Ramicane
# using a randomly selected mouse id
chosen_mouse = 'j989'
chosen_mouse_df = cleaned_combined_mouse_df.loc[cleaned_combined_mouse_df['Mouse ID'] == chosen_mouse]

# Setup the x and y axis
x_axis = chosen_mouse_df['Timepoint']
y_axis = chosen_mouse_df['Tumor Volume (mm3)']

# Create the line plot
plt.plot(x_axis, y_axis)

# Setup the title and labels
plt.title('Timepoint versus Tumor Volume for Mouse j989 (Ramicane)\n', fontsize=18)
plt.xlabel('Timepoint', fontsize=18)
plt.ylabel('Tumor Volume (mm3)', fontsize=18)

plt.show()
```


![png](Images/output_43_0.png)



```python
# Generate a line plot of time point versus tumor volume for a mouse treated with Infubinol
# using a randomly selected mouse id
chosen_mouse = 'k804'
chosen_mouse_df = cleaned_combined_mouse_df.loc[cleaned_combined_mouse_df['Mouse ID'] == chosen_mouse]

# Setup the x and y axis
x_axis = chosen_mouse_df['Timepoint']
y_axis = chosen_mouse_df['Tumor Volume (mm3)']

# Create the line plot
plt.plot(x_axis, y_axis)

# Setup title and labels
plt.title('Timepoint versus Tumor Volume for Mouse k804 (Infubinol)\n', fontsize=18)
plt.xlabel('Timepoint', fontsize=18)
plt.ylabel('Tumor Volume (mm3)', fontsize=18)

plt.show()
```


![png](Images/output_44_0.png)



```python
# Generate a line plot of time point versus tumor volume for a mouse treated with Infubinol
# using a randomly selected mouse id
chosen_mouse = 'b759'
chosen_mouse_df = cleaned_combined_mouse_df.loc[cleaned_combined_mouse_df['Mouse ID'] == chosen_mouse]
x_axis = chosen_mouse_df['Timepoint']
y_axis = chosen_mouse_df['Tumor Volume (mm3)']

# Create the line plot
plt.plot(x_axis, y_axis)

# Setup the title and labels
plt.title('Timepoint versus Tumor Volume for Mouse b759 (Ceftamin)\n', fontsize=18)
plt.xlabel('Timepoint', fontsize=18)
plt.ylabel('Tumor Volume (mm3)', fontsize=18)

# Show the chart
plt.show()
```


![png](Images/output_45_0.png)



```python
# Generate a scatter plot of mouse weight versus average tumor volume for the Capomulin regimen

# Get the data for the Drug Regimen Capomulin
capomulin_scatter_df = cleaned_combined_mouse_df.loc[cleaned_combined_mouse_df['Drug Regimen'] == "Capomulin"]

# Calculate the average
avg_capomulin_scatter_df = capomulin_scatter_df.groupby('Mouse ID').mean()

# Setup the x and y axis
x_axis = avg_capomulin_scatter_df['Weight (g)']
y_axis = avg_capomulin_scatter_df['Tumor Volume (mm3)']

# Create the scatter plot
plt.scatter(x_axis, y_axis)

# Setup the title and labels
plt.title('Average Mouse Weight versus Average Tumor Volume for Capomulin\n\n', fontsize=18)
plt.xlabel('Average Mouse Weight (g)', fontsize=18)
plt.ylabel('Average Tumor Volume (mm3)', fontsize=18)

# Show the scatter plot
plt.show()
```


![png](Images/output_46_0.png)


## Correlation and Regression


```python
# Calculate the correlation coefficient and linear regression model 
# for mouse weight and average tumor volume for the Capomulin regimen

# Get the dat afor the Drug Regimen Capomulin 
capomulin_scatter_df = cleaned_combined_mouse_df.loc[cleaned_combined_mouse_df['Drug Regimen'] == "Capomulin"]

# Calculate the average
avg_capomulin_scatter_df = capomulin_scatter_df.groupby('Mouse ID').mean()

# Set the x and y axis
x_axis = avg_capomulin_scatter_df['Weight (g)']
y_axis = avg_capomulin_scatter_df['Tumor Volume (mm3)']

# Perform the linear regression
(slope, intercept, rvalue, pvalue, stderr) = linregress(x_axis, y_axis)

# Output the R value
print(f"The R-Value between Average Mouse Weights and Average Tumor Volumes is {round(rvalue,2)} for Capomulin Mice.")

# Get the regress values
regress_values = x_axis * slope + intercept

# Setup the line equation
line_eq = "y = " + str(round(slope,2)) + "x + " + str(round(intercept,2))

# Generate the scatter plot for the line regression
plt.scatter(x_axis,y_axis)

# Inclue the line
plt.plot(x_axis,regress_values,"r-")

# Setup the labels and title
plt.xlabel('Average Mouse Weight (g)', fontsize=18)
plt.ylabel('Average Tumor Volume (mm3)', fontsize=18)
plt.title('Linear Regression for Average Mouse Weight (g) versus Average Tumor Volume (mm3) for Capomulin\n\n', fontsize=18)

# Add the line equation
plt.annotate(line_eq,(20,35),fontsize=15,color="red")

# Show the chart
plt.show()
```

    The R-Value between Average Mouse Weights and Average Tumor Volumes is 0.84 for Capomulin Mice.
    


![png](Images/output_48_1.png)

