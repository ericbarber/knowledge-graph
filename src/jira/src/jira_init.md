# Code to ensure atlassian-python-api is installed
```python

# Install atlassian-python-api
pip install atlassian-python-api
```
# Configure the user data to create Jira object

```python

# Initialize Jira object with user details
jira_user = "<username>"
jira_host = "<hostname>"
jira_api_key = "<api_key>"

jira = JIRA(server=jira_host, basic_auth=(jira_user, jira_api_key))
```

# Scope the Jira parameters for a specific project 
```python

# Set the project name
project_name = "<project_name>"

# Get the project object
project = jira.project(project_name)

# Get the list of issues for the project
issues = jira.search_issues('project={}'.format(project_name))
```

# Call the Jira API to get the issue data and store in spark dataframe
```python
# Create a spark dataframe to store the issue data
df = spark.createDataFrame([], schema=StructType([
    StructField("key", StringType(), True),
    StructField("summary", StringType(), True),
    StructField("status", StringType(), True),
    StructField("assignee", StringType(), True)
]))

# Iterate through the list of issues and add the data to the dataframe
for issue in issues:
    df = df.union(spark.createDataFrame([(issue.key, issue.fields.summary, issue.fields.status.name, issue.fields.assignee.displayName)], schema=df.schema))

# Show the dataframe
df.show()
```

# Load data into persisted table CDO.<project_name>_jira_issues_table
```python

# Create a table in CDO
df.write.format("org.apache.spark.sql.cassandra").options(table="<project_name>_jira_issues_table", keyspace="CDO").save()
```
