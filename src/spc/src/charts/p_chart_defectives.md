# Chapter: Implementing P-Chart Analysis in Rust

In this chapter, we delve into the implementation of p-chart analysis using Rust. A p-chart, or proportion chart, is a type of control chart used in statistical quality control to monitor the proportion of nonconforming units in a sample. We will develop two Rust functions: one to generate a dataset suitable for a p-chart and another to process this dataset for visualization.
## Generating a Dataset for P-Chart Analysis

The first step in p-chart analysis is to generate a dataset that represents the process we are monitoring. This dataset typically consists of samples, each with a number of trials and the count of successes (or failures) in those trials.
### Function: generate_p_chart_data

```rust

use rand::Rng;

pub fn generate_p_chart_data(
    num_samples: usize,
    sample_size: usize,
    base_success_rate: f64,
    variation: f64,
) -> Vec<(usize, usize)> {
    let mut rng = rand::thread_rng();
    let mut data = Vec::new();

    for _ in 0..num_samples {
        let success_rate = base_success_rate + rng.gen_range(-variation..=variation);
        let successes = (sample_size as f64 * success_rate).round() as usize;
        data.push((sample_size, successes));
    }

    data
}
```
## Explanation:

    Purpose: This function generates a dataset where each entry represents a sample in the process. Each sample includes the total number of trials and the number of successes.
    Parameters:
        num_samples: The number of samples to generate.
        sample_size: The number of trials in each sample.
        base_success_rate: The average rate of success across all samples.
        variation: The maximum variation in the success rate to simulate natural process variability.
    Implementation Details: The function uses the rand crate to introduce randomness in the success rate for each sample, simulating a real-world scenario where process performance varies over time.

## Preparing the Dataset for Visualization

Once we have our dataset, the next step is to process it for visualization in a p-chart. This involves calculating the proportion of successes in each sample and determining the control limits.
Function: calculate_p_chart_values

```rust

pub fn calculate_p_chart_values(data: &[(usize, usize)]) -> (Vec<f64>, f64, f64) {
    let total_samples = data.len();
    let total_successes: usize = data.iter().map(|&(_, successes)| successes).sum();
    let total_size: usize = data.iter().map(|&(size, _)| size).sum();

    let mean_proportion = total_successes as f64 / total_size as f64;
    let standard_deviation = (mean_proportion * (1.0 - mean_proportion) / total_size as f64).sqrt();

    let upper_control_limit = (mean_proportion + 3.0 * standard_deviation).min(1.0);
    let lower_control_limit = (mean_proportion - 3.0 * standard_deviation).max(0.0);

    let proportions: Vec<f64> = data
        .iter()
        .map(|&(size, successes)| successes as f64 / size as f64)
        .collect();

    (proportions, upper_control_limit, lower_control_limit)
}
```
## Explanation:

    Purpose: This function processes the dataset to calculate the proportion of successes in each sample and the control limits for the p-chart.
    Control Limits: These are calculated using the mean proportion of successes and its standard deviation. The upper and lower control limits are set at three standard deviations from the mean, a common practice in statistical process control.
    Output: The function returns the proportions of successes for each sample and the upper and lower control limits, which are essential for plotting a p-chart.

## Conclusion

By implementing these two functions in Rust, we can simulate a process, generate relevant data, and prmpare it for p-chart analysis. This approach demonstrates the power of Rust in handling statistical data and its application in quality control processes.

<hr />
This chapter provides a comprehensive guide to the implementation and purpose of each function, suitable for a professional audience familiar with Rust and statistical process control concepts. You can include this chapter in your mdBook by adding the provided Markdown content to your book's source files.
