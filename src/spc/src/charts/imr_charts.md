# I-MR Chart: Individuals and Moving Range Chart

## Implementing I-MR Chart Analysis in Rust

In this chapter, we explore the implementation of an I-MR (Individuals and Moving Range) Chart using Rust and PySoark. The I-MR chart is a tool used in statistical process control for monitoring process behavior over time, where the I-Chart tracks individual measurements and the MR-Chart tracks the moving range between measurements.
## Generating a Dataset for I-MR Chart Analysis

The first step in I-MR chart analysis is to generate a dataset representing individual measurements of a process. This dataset typically consists of a sequence of continuous data points.
Function: generate_imr_data

```rust

use rand::distributions::{Distribution, Normal};

pub fn generate_imr_data(num_points: usize, mean: f64, std_dev: f64) -> Vec<f64> {
    let normal = Normal::new(mean, std_dev);
    let mut rng = rand::thread_rng();
    (0..num_points).map(|_| normal.sample(&mut rng)).collect()
}
```
### Explanation:

    Purpose: This function generates a dataset of individual measurements modeled as a normal distribution.
    Parameters:
        num_points: The number of data points to generate.
        mean: The mean of the normal distribution.
        std_dev: The standard deviation of the normal distribution.
    Implementation Details: The function uses the rand crate's Normal distribution to simulate a process where each measurement is a random variable following a normal distribution.

## Preparing the Dataset for Visualization

Once we have our dataset, the next step is to process it for visualization in an I-MR chart. This involves calculating individual values and the moving ranges between them.
Function: calculate_imr_values

```rust

pub fn calculate_imr_values(data: &[f64]) -> (Vec<f64>, Vec<f64>) {
    let individual_values = data.to_vec();
    let moving_ranges: Vec<f64> = data
        .windows(2)
        .map(|pair| (pair[1] - pair[0]).abs())
        .collect();

    (individual_values, moving_ranges)
}
```
### Explanation:

    Purpose: This function processes the dataset to calculate individual values and their moving ranges, which are essential for plotting an I-MR chart.
    Moving Ranges: The moving range is calculated as the absolute difference between consecutive measurements.
    Output: The function returns two vectors: one for the individual values and another for the moving ranges.

## Conclusion

Implementing I-MR chart analysis in Rust allows us to monitor and analyze process behavior over time with precision and efficiency. By generating and processing data in Rust, we can leverage its performance and safety features, making it an excellent choice for statistical process control applications.

<hr />

## Rust Demonstration
```rust

use plotters::prelude::*;
use std::error::Error;
use rand_distr::{Normal, Distribution};

// Function to generate IMR data
pub fn generate_imr_data(num_points: usize, mean: f64, std_dev: f64) -> Vec<f64> {
    let normal = Normal::new(mean, std_dev).expect("Standard deviation must be positive");
    let mut rng = rand::thread_rng();
    (0..num_points).map(|_| normal.sample(&mut rng)).collect()
}

// Function to calculate IMR values
pub fn calculate_imr_values(data: &[f64]) -> (Vec<f64>, Vec<f64>) {
    let individual_values = data.to_vec();
    let moving_ranges: Vec<f64> = data
        .windows(2)
        .map(|pair| (pair[1] - pair[0]).abs())
        .collect();

    (individual_values, moving_ranges)
}

// Function to create the I-Chart and MR-Chart with control limits
pub fn create_imr_charts(data: &[f64]) -> Result<(), Box<dyn Error>> {
    let (individual_values, moving_ranges) = calculate_imr_values(data);

    // Calculate control limits for I-Chart
    let i_mean = individual_values.iter().sum::<f64>() / individual_values.len() as f64;
    let i_std_dev = (individual_values.iter().map(|&x| (x - i_mean).powi(2)).sum::<f64>() / (individual_values.len() - 1) as f64).sqrt();
    let i_upper_control_limit = i_mean + 3.0 * i_std_dev;
    let i_lower_control_limit = i_mean - 3.0 * i_std_dev;

    // Calculate control limits for MR-Chart
    let mr_mean = moving_ranges.iter().sum::<f64>() / moving_ranges.len() as f64;
    let mr_std_dev = (moving_ranges.iter().map(|&x| (x - mr_mean).powi(2)).sum::<f64>() / (moving_ranges.len() - 1) as f64).sqrt();
    let mr_upper_control_limit = mr_mean + 3.0 * mr_std_dev;
    let mr_lower_control_limit = mr_mean - 3.0 * mr_std_dev;

    // Create I-Chart with control limits
    create_chart(
        "i_chart.png",
        &individual_values,
        "I-Chart",
        "Individual Values",
        i_upper_control_limit,
        i_lower_control_limit,
    )?;

    // Create MR-Chart with control limits
    create_chart(
        "mr_chart.png",
        &moving_ranges,
        "MR-Chart",
        "Moving Ranges",
        mr_upper_control_limit,
        mr_lower_control_limit,
    )?;

    Ok(())
}

// Helper function to create a chart with control limits
fn create_chart(
    file_name: &str,
    values: &[f64],
    title: &str,
    _y_label: &str,
    upper_control_limit: f64,
    lower_control_limit: f64,
) -> Result<(), Box<dyn Error>> {
    let root = BitMapBackend::new(file_name, (640, 480)).into_drawing_area();
    root.fill(&WHITE)?;

    let max_value = values.iter().cloned().fold(f64::NAN, f64::max);
    let min_value = values.iter().cloned().fold(f64::NAN, f64::min);

    let y_range = min_value.min(lower_control_limit)..max_value.max(upper_control_limit);

    let mut chart = ChartBuilder::on(&root)
        .caption(title, ("sans-serif", 40))
        .margin(5)
        .x_label_area_size(30)
        .y_label_area_size(30)
        .build_cartesian_2d(0..values.len(), y_range)?;

    chart.configure_mesh().draw()?;

    // Draw control limits
    chart.draw_series(LineSeries::new(
        (0..values.len()).map(|x| (x, upper_control_limit)),
        ShapeStyle {
            color: RED.mix(0.3).to_rgba(),
            filled: true,
            stroke_width: 3, // Set the stroke width here
        },
    ))?;
    chart.draw_series(LineSeries::new(
        (0..values.len()).map(|x| (x, lower_control_limit)),
        ShapeStyle {
            color: RED.mix(0.3).to_rgba(),
            filled: true,
            stroke_width: 3, // Set the stroke width here
        },
    ))?;

    // Draw individual values or moving ranges
    chart.draw_series(LineSeries::new(
        values.iter().enumerate().map(|(i, &v)| (i, v)),
        &BLUE,
    ))?;
    
    // Draw individual values or moving ranges as dots
    chart.draw_series(PointSeries::of_element(
        values.iter().enumerate().map(|(i, &v)| (i, v)),
        3, // Size of the dot
        &BLUE, // Color of the dot
        &|coord, size, style| {
            EmptyElement::at(coord) + Circle::new((0, 0), size, style.filled())
        },
    ))?;
    

    root.present()?;
    Ok(())
}

fn main() -> Result<(), Box<dyn Error>> {
    let data = generate_imr_data(50, 10.0, 2.0);
    create_imr_charts(&data)
}
```

<hr />

Creating an I-MR (Individuals and Moving Range) chart using PySpark involves several steps. PySpark is primarily used for data processing in distributed systems and isn't typically used for plotting, but we can use it to calculate the necessary statistics and then use a Python plotting library like matplotlib for the actual chart creation.

Here's a script that demonstrates how you might achieve this. Note that this script assumes you have a Spark session already running. If not, you'll need to set one up.
PySpark Script for I-MR Chart

 ```python

from pyspark.sql import SparkSession
from pyspark.sql.functions import lag, col
from pyspark.sql.window import Window
import matplotlib.pyplot as plt
import numpy as np

# Initialize Spark session
spark = SparkSession.builder.appName("IMRChart").getOrCreate()

# Sample data generation
np.random.seed(0)
data = np.random.normal(10, 2, 100)
df = spark.createDataFrame(data, ["values"])

# Calculate moving range
windowSpec = Window.orderBy("id")
df = df.withColumn("id", monotonically_increasing_id())
df = df.withColumn("prev_value", lag("values").over(windowSpec))
df = df.withColumn("moving_range", abs(col("values") - col("prev_value")))

# Calculate control limits for I-Chart
mean_value = df.select(mean(col("values"))).collect()[0][0]
std_dev_value = df.select(stddev(col("values"))).collect()[0][0]
i_upper_control_limit = mean_value + 3 * std_dev_value
i_lower_control_limit = mean_value - 3 * std_dev_value

# Calculate control limits for MR-Chart
mean_mr = df.select(mean(col("moving_range"))).collect()[0][0]
std_dev_mr = df.select(stddev(col("moving_range"))).collect()[0][0]
mr_upper_control_limit = mean_mr + 3 * std_dev_mr
mr_lower_control_limit = mean_mr - 3 * std_dev_mr

# Collect data for plotting
individual_values = df.select("values").rdd.flatMap(lambda x: x).collect()
moving_ranges = df.select("moving_range").rdd.flatMap(lambda x: x).collect()

# Plotting
fig, axs = plt.subplots(2, 1, figsize=(10, 8))

# I-Chart
axs[0].plot(individual_values, marker='o', color='b')
axs[0].axhline(y=mean_value, color='g', linestyle='--')
axs[0].axhline(y=i_upper_control_limit, color='r', linestyle='--')
axs[0].axhline(y=i_lower_control_limit, color='r', linestyle='--')
axs[0].set_title('I-Chart')
axs[0].set_ylabel('Individual Values')

# MR-Chart
axs[1].plot(moving_ranges, marker='o', color='b')
axs[1].axhline(y=mean_mr, color='g', linestyle='--')
axs[1].axhline(y=mr_upper_control_limit, color='r', linestyle='--')
axs[1].axhline(y=mr_lower_control_limit, color='r', linestyle='--')
axs[1].set_title('MR-Chart')
axs[1].set_ylabel('Moving Range')

plt.tight_layout()
plt.show()
```
How This Script Works:

 - Spark Session: The script starts by creating a Spark session. This is necessary for running any PySpark code.
 - Data Generation: For demonstration purposes, the script generates a sample dataset using NumPy. This dataset is then converted into a Spark DataFrame.
 - Moving Range Calculation: It calculates the moving range for each data point using the lag function in a window specification.
 - Control Limits Calculation: The script calculates the mean and standard deviation for both individual values and moving ranges, and then determines the upper and lower control limits.
 - Data Collection for Plotting: The individual values and moving ranges are collected from the Spark DataFrame into local Python lists for plotting.
 - Plotting: The script uses matplotlib to plot both the I-Chart and the MR-Chart, including the control limits.

Requirements:

 - PySpark: This script requires PySpark to be installed and properly configured.
 - Matplotlib: For plotting, ensure you have matplotlib installed in your Python environment.

Note:

 - The script assumes a local Spark session. If you're running this in a distributed environment, the setup might be different.
 - The control limit calculations here are basic and assume normally distributed data. Depending on your specific requirements, you might need to adjust these calculations.
