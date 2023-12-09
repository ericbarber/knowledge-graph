# Spark Code of SPC
# Introduction to Statistical Process Control (SPC)
Statistical Process Control (SPC) is a method of quality control which employs statistical methods to monitor and control a process. This helps ensure the process operates efficiently, producing more specification-conforming products with less waste (rework
or scrap). SPC can be applied to any process where the "conforming product" (product meeting specifications) output can be measured.

## Key benefits include:

Early Detection of Issues: Identify problems before they become critical.
Reduced Process Variability: Consistency in process outputs.
Improved Quality: Enhanced control over the production process.
Cost Efficiency: Reduction in waste and rework.
In this presentation, we will explore how SPC can be integrated into your process management and reporting.

# Process
In the context of Shewhart control charts and statistical process control (SPC), a "process" refers to a sequence of actions or steps taken to achieve a particular end in a production or business environment. This definition encompasses a wide range of activ
ities and can be applied to virtually any operation where inputs are transformed into outputs. Key aspects of a process in this context include:

Transformation: A process involves transforming inputs (which could be materials, information, or labor) into outputs (products, services, or results). This transformation is governed by a set of defined steps or operations.
Repeatability: The process is typically repeatable and consistent. It's not a one-time event but a series of actions that are performed regularly, often under similar conditions.
Measurable Outputs: In the context of SPC, the outputs of the process or its characteristics must be measurable. This is crucial because SPC relies on quantitative data to monitor process performance and control.
Control and Variation: Every process has inherent variability, but within a controlled process, this variability is predictable, within limits. The aim of using Shewhart control charts is to distinguish between common cause variation (inherent to the process
) and special cause variation (due to specific, identifiable factors).
Goal of Improvement: While a process implies a certain level of standardization, it is also subject to continuous monitoring and improvement. The objective is to enhance efficiency, quality, and predictability.
Cross-Functional: Processes often span multiple functions or departments within an organization. They can be internal (focused on organizational operations) or external (focused on delivering a product or service to customers).
In summary, in the realm of Shewhart control charts and SPC, a process is a systematic series of actions taken to convert inputs into outputs, characterized by measurable outcomes and inherent variability. The primary focus is on understanding, controlling,
and improving this variability to enhance the process's efficiency and effectiveness.

Basic Concepts of SPC
SPC involves using control charts to monitor the performance of a process.

These are the key concepts:

Control Limits: These are the boundaries of acceptable variation in the process.
Process Variation: Understanding natural (common cause) and special (assignable cause) variations.
Control Charts: Tools used to determine whether a manufacturing or business process is in a state of control.
Let's start by loading some example process data.

from pyspark.sql.types import StructType, StructField, StringType, FloatType, DateType
from pyspark.sql.window import Window

adjust_value = 3000

def deep_copy_dataframe(df):
    return df.select("*")

def year_month_data(years: list) -> list:
    year_month_record = []

    StructField('CONTROL_NAME', StringType(), True)
])

# Dataframe
c_chart_sdf = spark.createDataFrame(c_chart_pdf, schema)

display(c_chart_sdf)
Table
Visualization 1
CONTROL_NAME
MockData
value_20230901 = c_chart_sdf.select('feature').where('DATE = "2021-08-01"').collect()[0][0]
c_chart_sdf = c_chart_sdf.na.replace([value_20230901], [590 + adjust_value], 'FEATURE')

Plotting a Basic Control Chart
We will now plot a basic control chart. For demonstration, we'll use a simple process metric from our dataset.


display(c_chart_sdf)
Table
Visualization 1
SUM(FEATURE)
SUM(MEAN)
SUM(UPPER_CONTROL_LIMIT)
Interpreting Control Charts
The control chart is a key tool in SPC. Here's how to interpret it:

Within Limits: If points are within limits, the process is assumed to be in control.
Outside Limits: Points outside the limits indicate a potential issue or a shift in the process.
Patterns: Certain patterns can also indicate process issues, even if all points are within control limits.
Common Cause and Special Cause Variation
Understanding the concepts of common cause variation and special cause (or assignable cause) variation is fundamental in the field of statistical process control (SPC) and quality management. These concepts were popularized by Dr. W. Edwards Deming, a key fi
gure in the development of modern quality control. Common Cause Variation

Definition:
Common cause variation, also known as "random cause variation," is the natural and inherent variation that occurs in a process over time. This type of variation is expected, predictable (within certain limits), and due to the system's inherent design and fun
ctioning.
Characteristics:
Inherent to the Process: It is a part of the process and exists under stable and normal operating conditions.
Predictable: It is possible to predict statistically the range of variation that will occur due to common causes.
Systemic Factors: Often due to a combination of many small, random, and unidentifiable factors that are built into the system.
Management Responsibility: Since it is inherent to the system, addressing common cause variation typically requires systematic changes by management. This might include redesigning the process, investing in new equipment, or updating policies.
Example:
In a manufacturing process, common cause variation might be the slight differences in product dimensions due to tool wear, temperature fluctuations, or material density variations.
In Online Transaction Processing application, a common cause variation might be due to network or server capacity
Special Cause Variation
Definition:
Special cause variation, also known as "assignable cause variation," refers to variation that can be traced to a specific, identifiable cause. This type of variation is sporadic and unpredictable, occurring outside of the system's regular operating condition
s.
Characteristics:
Not Inherent to the Process: It arises from external factors and is not a natural part of the process.
Unpredictable: It cannot be predicted statistically and often appears as a surprise or anomaly.
Identifiable Causes: The causes, once identified, can often be specifically addressed and corrected.
Operator Responsibility: Frontline workers and supervisors can often identify and rectify these causes through immediate corrective actions.
Example:
In the same manufacturing process, a special cause variation might be a sudden spike in product dimension errors due to a malfunctioning machine or an incorrect batch of raw material.
Importance in SPC:
In statistical process control, distinguishing between these two types of variation is crucial:

Control Charts: These tools are used to monitor process variation. If a process only exhibits common cause variation, it is considered to be in control. However, if special cause variation is detected, the process is considered to be out of control, signaling the
 need for investigation and corrective action.
Process Improvement: Understanding the type of variation influencing a process is key to selecting the right approach for improvements. While common cause variation requires systemic changes, special cause variation can often be resolved with specific, targeted a
ctions.
In summary, common cause variation is the inherent variability in a stable process, while special cause variation results from specific, identifiable factors that disrupt the normal functioning of the process. Identifying and addressing these variations appropria
tely is a cornerstone of effective process control and continuous improvement initiatives.

Signal Processing
When a Shewhart chart, or any statistical process control (SPC) chart, detects a signal (i.e., a data point or a pattern indicating a potential issue), it's important to follow a structured approach to investigate and address the issue. Here's a general guideline
 on what to do:

Verify the Signal

Data Verification: First, ensure that the signal is not due to a data recording or entry error. Verify the accuracy of the data point in question. Rule Check: Confirm that the signal indeed violates the control rules (e.g., a point outside the control limits, a r
un of points on one side of the mean, etc.).

Pause and Investigate

Process Interruption (if necessary): Depending on the severity or nature of the signal, you may need to temporarily halt the process to prevent further issues. Root Cause Analysis: Conduct a thorough investigation to determine the cause of the signal. This might
involve checking machinery, processes, materials, environmental conditions, or human factors.

Take Corrective Action

Immediate Action: If the cause is identified and can be corrected immediately, do so. This might involve adjusting a machine, replacing faulty materials, or correcting a procedural error. Document the Issue and Action: Record the occurrence and the actions taken.
 This documentation is crucial for future reference and analysis.

Review and Adjust the Process

Process Improvement: If the signal indicates a deeper issue with the process, use this as an opportunity for process improvement. Update Procedures or Training: Sometimes, a signal might indicate that procedures need updating or staff need additional training.

Monitor Post-Correction

Intensive Monitoring: After addressing the issue, monitor the process closely to ensure that the corrective action has been effective. Adjust Control Limits (if necessary): If the process change is permanent, recalculate and adjust the control limits accordingly.

Communication

Inform Relevant Parties: Keep team members, management, and other relevant stakeholders informed about the issue and the steps taken to address it. Feedback Loop: Encourage feedback and discussion about the process and its control to foster a culture of continuou
s improvement.

Preventive Measures

Analyze Trends: Use the data collected over time to identify any trends or recurring issues. Proactive Improvement: Implement preventive measures to avoid similar issues in the future.

Documentation and Learning

Record Keeping: Ensure all steps from detection to resolution are well documented. Learning and Training: Use the experience as a learning opportunity for the team. Share insights and lessons learned to prevent future occurrences.

Remember, the goal of SPC and control charts is not just to detect problems but to drive process improvement and maintain process control. Each signal is an opportunity to learn more about the process and make it more robust.

Integration into Business Processes
Integrating SPC into your business processes involves:

Regular Monitoring: Establish a routine for regular monitoring of key metrics using control charts.
Training: Ensure that staff are trained in interpreting control charts.
Response Plans: Develop plans for responding to signals from control charts.
Continuous Improvement: Use insights from SPC to drive process improvements.
In conclusion, SPC offers a powerful set of tools for improving and maintaining process quality. By integrating control charts into your process management and reporting, you can gain valuable insights into your process performance and make more informed decision
s.

Conclusion and Next Steps
In this presentation, we have introduced the basics of Statistical Process Control and demonstrated how to create and interpret a control chart using Python. The next steps involve:

Identifying Key Processes: Determine which processes in your organization could benefit most from SPC.
Data Collection: Ensure you have reliable data collection mechanisms for these processes.
Implementation: Start with a pilot program to integrate SPC into these processes.
Review and Adapt: Regularly review the effectiveness and adapt your approach as necessary.
Thank you for your attention, and I look forward to any questions you may have.Introduction to Statistical Process Control (SPC)
Statistical Process Control (SPC) is a method of quality control which employs statistical methods to monitor and control a process. This helps ensure the process operates efficiently, producing more specification-conforming products with less waste (rework or sc
rap). SPC can be applied to any process where the "conforming product" (product meeting specifications) output can be measured.
Key benefits include:

Early Detection of Issues: Identify problems before they become critical.
Reduced Process Variability: Consistency in process outputs.
Improved Quality: Enhanced control over the production process.
Cost Efficiency: Reduction in waste and rework.
In this presentation, we will explore how SPC can be integrated into your process management and reporting.

Process
In the context of Shewhart control charts and statistical process control (SPC), a "process" refers to a sequence of actions or steps taken to achieve a particular end in a production or business environment. This definition encompasses a wide range of activities
 and can be applied to virtually any operation where inputs are transformed into outputs. Key aspects of a process in this context include:

Transformation: A process involves transforming inputs (which could be materials, information, or labor) into outputs (products, services, or results). This transformation is governed by a set of defined steps or operations.
Repeatability: The process is typically repeatable and consistent. It's not a one-time event but a series of actions that are performed regularly, often under similar conditions.
Measurable Outputs: In the context of SPC, the outputs of the process or its characteristics must be measurable. This is crucial because SPC relies on quantitative data to monitor process performance and control.
Control and Variation: Every process has inherent variability, but within a controlled process, this variability is predictable, within limits. The aim of using Shewhart control charts is to distinguish between common cause variation (inherent to the process) and
 special cause variation (due to specific, identifiable factors).
Goal of Improvement: While a process implies a certain level of standardization, it is also subject to continuous monitoring and improvement. The objective is to enhance efficiency, quality, and predictability.
Cross-Functional: Processes often span multiple functions or departments within an organization. They can be internal (focused on organizational operations) or external (focused on delivering a product or service to customers).
In summary, in the realm of Shewhart control charts and SPC, a process is a systematic series of actions taken to convert inputs into outputs, characterized by measurable outcomes and inherent variability. The primary focus is on understanding, controlling, and i
mproving this variability to enhance the process's efficiency and effectiveness.

Basic Concepts of SPC
SPC involves using control charts to monitor the performance of a process.

These are the key concepts:

Control Limits: These are the boundaries of acceptable variation in the process.
Process Variation: Understanding natural (common cause) and special (assignable cause) variations.
Control Charts: Tools used to determine whether a manufacturing or business process is in a state of control.
Let's start by loading some example process data.

from pyspark.sql.types import StructType, StructField, StringType, FloatType, DateType
from pyspark.sql.window import Window

adjust_value = 3000

def deep_copy_dataframe(df):
    return df.select("*")

def year_month_data(years: list) -> list:
    year_month_record = []

    StructField('CONTROL_NAME', StringType(), True)
])

# Dataframe
c_chart_sdf = spark.createDataFrame(c_chart_pdf, schema)

display(c_chart_sdf)
c_chart_sdf:pyspark.sql.dataframe.DataFrame = [DATE: date, FEATURE: float ... 1 more field]
Table
Visualization 1
CONTROL_NAME
MockData
value_20230901 = c_chart_sdf.select('feature').where('DATE = "2021-08-01"').collect()[0][0]
c_chart_sdf = c_chart_sdf.na.replace([value_20230901], [590 + adjust_value], 'FEATURE')

display(c_chart_sdf)
c_chart_sdf:pyspark.sql.dataframe.DataFrame = [DATE: date, FEATURE: float ... 5 more fields]
Table
Visualization 1
48 rows
Plotting a Basic Control Chart
We will now plot a basic control chart. For demonstration, we'll use a simple process metric from our dataset.


display(c_chart_sdf)
Table
SUM(FEATURE)
SUM(MEAN)
SUM(UPPER_CONTROL_LIMIT)
SUM(LOWER_CONTROL_LIMIT)
Interpreting Control Charts
The control chart is a key tool in SPC. Here's how to interpret it:

Within Limits: If points are within limits, the process is assumed to be in control.
Outside Limits: Points outside the limits indicate a potential issue or a shift in the process.
Patterns: Certain patterns can also indicate process issues, even if all points are within control limits.
Common Cause and Special Cause Variation
Understanding the concepts of common cause variation and special cause (or assignable cause) variation is fundamental in the field of statistical process control (SPC) and quality management. These concepts were popularized by Dr. W. Edwards Deming, a key figure
in the development of modern quality control. Common Cause Variation

Definition:
Common cause variation, also known as "random cause variation," is the natural and inherent variation that occurs in a process over time. This type of variation is expected, predictable (within certain limits), and due to the system's inherent design and function
ing.
Characteristics:
Inherent to the Process: It is a part of the process and exists under stable and normal operating conditions.
Predictable: It is possible to predict statistically the range of variation that will occur due to common causes.
Systemic Factors: Often due to a combination of many small, random, and unidentifiable factors that are built into the system.
Management Responsibility: Since it is inherent to the system, addressing common cause variation typically requires systematic changes by management. This might include redesigning the process, investing in new equipment, or updating policies.
Example:
In a manufacturing process, common cause variation might be the slight differences in product dimensions due to tool wear, temperature fluctuations, or material density variations.
In Online Transaction Processing application, a common cause variation might be due to network or server capacity
Special Cause Variation
Definition:
Special cause variation, also known as "assignable cause variation," refers to variation that can be traced to a specific, identifiable cause. This type of variation is sporadic and unpredictable, occurring outside of the system's regular operating conditions.
Characteristics:
Not Inherent to the Process: It arises from external factors and is not a natural part of the process.
Unpredictable: It cannot be predicted statistically and often appears as a surprise or anomaly.
Identifiable Causes: The causes, once identified, can often be specifically addressed and corrected.
Operator Responsibility: Frontline workers and supervisors can often identify and rectify these causes through immediate corrective actions.
Example:
In the same manufacturing process, a special cause variation might be a sudden spike in product dimension errors due to a malfunctioning machine or an incorrect batch of raw material.

In statistical process control, distinguishing between these two types of variation is crucial:                                                                                                                                                                [34/347]

Control Charts: These tools are used to monitor process variation. If a process only exhibits common cause variation, it is considered to be in control. However, if special cause variation is detected, the process is considered to be out of control, signaling the
 need for investigation and corrective action.
Process Improvement: Understanding the type of variation influencing a process is key to selecting the right approach for improvements. While common cause variation requires systemic changes, special cause variation can often be resolved with specific, targeted a
ctions.
In summary, common cause variation is the inherent variability in a stable process, while special cause variation results from specific, identifiable factors that disrupt the normal functioning of the process. Identifying and addressing these variations appropria
tely is a cornerstone of effective process control and continuous improvement initiatives.

Signal Processing
When a Shewhart chart, or any statistical process control (SPC) chart, detects a signal (i.e., a data point or a pattern indicating a potential issue), it's important to follow a structured approach to investigate and address the issue. Here's a general guideline
 on what to do:

Verify the Signal

Data Verification: First, ensure that the signal is not due to a data recording or entry error. Verify the accuracy of the data point in question. Rule Check: Confirm that the signal indeed violates the control rules (e.g., a point outside the control limits, a r
un of points on one side of the mean, etc.).

Pause and Investigate

Process Interruption (if necessary): Depending on the severity or nature of the signal, you may need to temporarily halt the process to prevent further issues. Root Cause Analysis: Conduct a thorough investigation to determine the cause of the signal. This might
involve checking machinery, processes, materials, environmental conditions, or human factors.

Take Corrective Action

Immediate Action: If the cause is identified and can be corrected immediately, do so. This might involve adjusting a machine, replacing faulty materials, or correcting a procedural error. Document the Issue and Action: Record the occurrence and the actions taken.
 This documentation is crucial for future reference and analysis.

Review and Adjust the Process

Process Improvement: If the signal indicates a deeper issue with the process, use this as an opportunity for process improvement. Update Procedures or Training: Sometimes, a signal might indicate that procedures need updating or staff need additional training.

Monitor Post-Correction

Intensive Monitoring: After addressing the issue, monitor the process closely to ensure that the corrective action has been effective. Adjust Control Limits (if necessary): If the process change is permanent, recalculate and adjust the control limits accordingly.


Communication

Inform Relevant Parties: Keep team members, management, and other relevant stakeholders informed about the issue and the steps taken to address it. Feedback Loop: Encourage feedback and discussion about the process and its control to foster a culture of continuou
s improvement.

Preventive Measures

Analyze Trends: Use the data collected over time to identify any trends or recurring issues. Proactive Improvement: Implement preventive measures to avoid similar issues in the future.

Documentation and Learning

Record Keeping: Ensure all steps from detection to resolution are well documented. Learning and Training: Use the experience as a learning opportunity for the team. Share insights and lessons learned to prevent future occurrences.

Remember, the goal of SPC and control charts is not just to detect problems but to drive process improvement and maintain process control. Each signal is an opportunity to learn more about the process and make it more robust.

Integration into Business Processes
Integrating SPC into your business processes involves:

Regular Monitoring: Establish a routine for regular monitoring of key metrics using control charts.
Training: Ensure that staff are trained in interpreting control charts.
Response Plans: Develop plans for responding to signals from control charts.
Continuous Improvement: Use insights from SPC to drive process improvements.
In conclusion, SPC offers a powerful set of tools for improving and maintaining process quality. By integrating control charts into your process management and reporting, you can gain valuable insights into your process performance and make more informed decision
s.

Conclusion and Next Steps
In this presentation, we have introduced the basics of Statistical Process Control and demonstrated how to create and interpret a control chart using Python. The next steps involve:

Identifying Key Processes: Determine which processes in your organization could benefit most from SPC.
Data Collection: Ensure you have reliable data collection mechanisms for these processes.
Implementation: Start with a pilot program to integrate SPC into these processes.
Review and Adapt: Regularly review the effectiveness and adapt your approach as necessary.
Thank you for your attention, and I look forward to any questions you may have.
