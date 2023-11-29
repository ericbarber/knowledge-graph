# Charts

## Shewhart Control Charts Overview

Shewhart control charts, named after Walter A. Shewhart, are essential tools in statistical process control (SPC) for monitoring process variability. These charts are designed for various types of data and process characteristics. Below is a list of the primary types of Shewhart control charts and their applications.

## Types of Shewhart Control Charts

### X̄ and R Chart (Mean and Range Chart)

- **Purpose:** Monitors the process mean and variability for subgrouped samples.
- **Data Type:** Continuous data.

### X̄ and S Chart (Mean and Standard Deviation Chart)

- **Purpose:** Similar to the X̄ and R chart, but uses standard deviation to measure variability.
- **Ideal For:** Larger sample sizes (typically n > 10).

### I-MR Chart (Individuals and Moving Range Chart)

- **Purpose:** Monitors individual observations and the moving range between two successive observations.
- **Suitable For:** Data collected individually, not in subgroups.

### P-Chart (Proportion Chart)

- **Purpose:** Monitors the proportion of defective items in a process.
- **Data Type:** Attribute data (e.g., defective or not defective).
- **Example:** [Number of defective items in a batch](./p_chart_defectives.html)

### NP-Chart (Number Defective Chart)

- **Purpose:** Similar to the P-chart but tracks the count of defective items instead of the proportion.
- **Ideal For:** Constant sample sizes.

### C-Chart (Count Chart)

- **Purpose:** Monitors the count of defects per unit when the number of opportunities for defects is constant.
- **Data Type:** Nonconformities (defects) in a process.

### U-Chart (Defects per Unit Chart)

- **Purpose:** Similar to the C-chart but used when the number of opportunities for defects varies.
- **Application:** Monitors defects per unit of measure (e.g., per item, per square meter).

### EWMA Chart (Exponentially Weighted Moving Average Chart)

- **Purpose:** Detects small shifts in the process mean.
- **Feature:** Places more weight on recent data points.

### CUSUM Chart (Cumulative Sum Control Chart)

- **Purpose:** Focuses on the cumulative sum of deviations from the target.
- **Sensitivity:** To small and persistent shifts in the process level.

## Conclusion

Each Shewhart chart type is tailored for specific data types and process monitoring needs. The choice of chart depends on the data's nature (continuous or attribute), the sample size, and the specific process aspect to be monitored (e.g., process mean, variability, proportion of defects). Understanding each chart's characteristics is key to effective process monitoring and quality control.

