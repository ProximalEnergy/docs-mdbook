# Combiner Field Health KPI

## Description

The **Combiner Field Health KPI** measures the DC health of a project based on combiner current data. Each combiner receives a daily health score ranging from 0 to 1, where a score of 1 indicates the healthiest combiners on the project.

## Methodology

1. **Retrieve DC combiner current data** for each combiner box on a 5-minute interval. The data is filtered to focus on the time window from 11:30 AM to 12:30 PM to capture peak irradiance conditions.  
   ![Combiner Field Health Methodology](../assets/kpi/combiner_fuse_health/img_1.png)
2. **Normalize each combiner's current** against its own DC capacity to account for differences in combiner size.  
   ![alt text](../assets/kpi/combiner_fuse_health/img_2.png)
3. **Identify the "ideal" combiner** by calculating the 99th percentile of all normalized combiner currents. The 99% threshold is chosen to exclude outliers and focus on typical combiner performance.  
   ![alt text](../assets/kpi/combiner_fuse_health/img_3.png)
4. **Normalize all combiners' ratios** against the ideal combiner. This gives the ideal combiner a score of 1.0, with all other combiners having a score ranging from 0 to 1. Outliers may exceed a score of 1.0.  
   ![alt text](../assets/kpi/combiner_fuse_health/img_4.png)
5. **Calculate the daily mean score** of all combiner health scores to derive the overall DC health for the project on that day.  
   ![alt text](../assets/kpi/combiner_fuse_health/img_5.png)
