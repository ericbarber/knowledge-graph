# Entity Relationship of Common Jira Object

Creating an Entity-Relationship Diagram (ERD) for a BI Dashboard that integrates with JIRA to display issue details, including issue types, epics, and component details, involves understanding the basic structure of JIRA's data model. 

    Issues Table: Central table for individual issues.
        Issue ID: Primary Key
        Summary
        Status
        Issue Type ID: Foreign Key to Issue Type table
        Epic Link: Foreign Key to Epic table
        Project ID
        Other fields as required

    Issue Type Table: Stores different types of issues.
        Issue Type ID: Primary Key
        Issue Type Name

    Epic Table: Contains details about epics.
        Epic ID: Primary Key
        Epic Name
        Epic Summary
        Project ID

    Components Table: Lists different components.
        Component ID: Primary Key
        Component Name
        Project ID

    Issue-Component Link Table: Junction table for the many-to-many relationship between issues and components.
        Issue ID: Foreign Key
        Component ID: Foreign Key

    Issue Change Log Table: Records changes made to issues.
        Change ID: Primary Key
        Issue ID: Foreign Key to the Issues table
        Changed Field: The name of the field that was changed
        Old Value
        New Value
        Change Date: Timestamp of the change
        Changed By: User who made the change

    Project Table (optional): Stores details about projects.
        Project ID: Primary Key
        Project Name

Relationships:

    Each issue has one issue type (many-to-one between Issues and Issue Type).
    Each issue may be part of an epic (many-to-one between Issues and Epic).
    Many-to-many relationship between Issues and Components, represented by the Issue-Component Link table.
    The Issue Change Log table has a many-to-one relationship with the Issues table, as each issue can have multiple change log entries, but each change log entry is associated with only one issue.
    If included, the Project table has a many-to-one relationship with both the Issues table and the Epic table.

This ERD provides a more comprehensive view of the data model, especially useful for tracking and analyzing the history of issue changes in your BI dashboard. As with the previous model, you may need to adjust this based on your specific JIRA configuration and the data available.
