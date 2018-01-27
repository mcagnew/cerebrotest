<!-- UI Docs draft. 
- TK - Images to be updated once 0.8 UI is finalized
- TD - Please Review --> 

# Cerebro Web UI
The Cerebro Web UI provides a **read only** user interface to: view your account information, find and understand datasets, understand your access to datasets and columns, and inspect group permissions on any dataset for which you are an admin (you have all privileges).

- [Accessing the UI](#accessing-the-ui)
- [Supported Browsers](#supported-browsers)
- [Logging in](#logging-in)
- [Account Details](#account-details)
- [About Dialog](#about-dialog)
- [Copying your token](#copying-your-access-token)
- [Datasets Page](#datasets-page)
  - [Searching and Filtering](#searching-and-filtering-datasets)
  - [Dataset details](#dataset-details)
  - [Preview a dataset](#dataset-preview)
  - [Get started snippets](#get-started-snippets)
- [Datasets with Errors](#datasets-with-errors)
- [Admin Features](#admin-features)
  -  [Review Group Access on Datasets](#review-group-access-on-datasets)
  -  [Review Group Access on a Dataset Schema](#review-group-access-on-a-dataset-schema)
- [Troubleshooting](#troubleshooting)

## Accessing the UI
To access the Web UI, first set up a Cerebro cluster. Once the cluster is set up, determine the endpoints that have been created, for example by running `cerebro_cli clusters endpoints <cluster_id>`. Find the host and port of the component named `cerebro_web:webui` and access `http://<host>:<port>` (or `https://<host>:<port>` if using SSL) in a web browser.

## Supported Browsers
The Cerebro UI is supported on the following browsers:

<!-- TD Mike and Marshall to fill out
Windows: Chrome, Firefox 17+, Internet Explorer Edge+, Safari 5+
Linux: Chrome, Firefox 17+
Mac: Chrome, Firefox 17+, Safari 5+ -->

## Logging in 
You will see a login screen that can be configured to provide various login options depending on your cluster configuration, including:
- **Cerebro token login**: this option is always available for log in as long as the user can get a Cerebro token.
<!-- TD Other tokens besides Cerebro are accepted (JWT, SSO) should we update to reflect this? -->
- **username/password login**: if the cluster is configured to authenticate with LDAP, username and password login is available
- **OAuth login**: OAuth login is available if the cluster is configured for it and there is an auth server set up to handle it.

## Using the UI 

### Account Details 

After you login, you will land on the **Home Page**. From this page, you can see your account information including: 
  - Your Cerebro username
  - The groups you belong to
  - The roles you have been granted, and the groups granting you those roles
  - Your access token, including when it will expire

*Tip: you can use CTRL+F or Cmd+F(Mac) to quickly find a group or role name*

![Cerebro Account Details][accountdetails]

### About Dialog 
You will find the **About** dialog in the top right hand corner of the navigation next to your username. Clicking on it will bring up some handy info for diagnostics as well as the REST API and Planner endpoints. 
- UI Version
- Build Hash
- Planner Version 
- Planner Host/Port
- REST API Endpoint
- Authentication Methods Available

### Copying your Access token 

**Home Page**

![Cerebro Home Page][token-home]

**Quick Access Menu option** 
Click your username in the top left > 'Copy Access Token'

![Cerebro Copy Token in Menu][token-menu]

### Datasets Page

From the navigation tabs in the upper part of the page, you can access the Datasets page.

The Datasets page allows you to browse, search for, filter, and preview the datasets you have access to in the Cerebro deployment. For users who have admin access to some number of datasets, the Datasets page also enables checking which groups have access to those datasets and which fields users in those groups can read.

**Any dataset where you have any level of access will appear in this list.**

>:heavy_exclamation_mark: This means there could be datasets in the catalog that are not shown if you have no read access to any field in them. 

### Searching and Filtering Datasets

<!-- TK Insert Image of datasets search + filter -->

At the top of the page contains search and filter options for the list of datasets, including a:

- **Search box** where you can search by dataset name. Any dataset whose name contains your input as a substring will be shown. 
*Tip: You can quickly clear this box with the ESC key.*
- **'Filter by database' multi-select box** that allows you to filter the list to only datasets in a particular database or set of databases. 

### Dataset Details
Clicking on a dataset card in the datasets list will display that dataset's details page on the right hand side.

![Cerebro Datasets Details Page][dataset-details]

This page contains:
  - The dataset's **metadata**, including
    - Database name
    - Dataset name
    - Owner
    - Location
    - Description
    - Created - Date and time the dataset was created  
    - Metadata Changed - Date and time the metadata was last changed
    - View Definition (if dataset is an external view)
  - The dataset schema described in full
    - Total number of columns in the schema (next to 'Schema' tab)
    - Each column in the dataset is shown, including
     - Name
     - Type
     - Access (whether you have read-access to view the cells in this column)
    - If a column's name has a gray background, it is a **partitioning column**
    - ![Cerebro paritioning field][partioning-field]
      
### If you do not have access to a column
You may notice that your schema looks like below. This means that you do not have access to every column in the schema 

![Cerebro See Groups][schema-access]

**To see which groups have access to that column**
- Click "See Groups" in the row
- You will see a list of groups; you will need to be added to at least one of these groups to gain access to the column

### Dataset preview
To preview a dataset, click on the **Show Preview** button in the upper right part of the details page. 

![Cerebro Preview Dataset][preview]

- No more than 200 rows will be shown (to view more, see "Dataset usage" below)
- You will not see information for columns for which you have no access
- The dialog can scroll vertically *as well as* horizontally

### Get Started snippets
For a quick way to start using this dataset, click on the **Get Started** button in the upper right part of the details page.

![Cerebro Get Started snippets][get-started]

<!-- TD Check with Mike these snippets are working -->

   - Sections of different sample code will be shown (Spark, Hive, Python, R and CURL)
   - Click on the tab of your choice and copy/paste the code into the application of your choice to start using this dataset

### Datasets with Errors 
<!-- TK Image of Datasets with Errors image showing filter + error message highlighted -->
In the UI **Datasets with errors** are those that failed to load due to a problem with their metdata (e.g unsupported datatype). Datasets with errors will always be at the bottom of your datasets list. You can use the filter on top of the datasets list to view **only** Datasets with Errors. Clicking on a dataset with an error will reveal the error message on the details page on the right. 

<!-- TD review this to check wording on disclaimer --> 
>:heavy_exclamation_mark:There may still be failing datasets in the Cerebro deployment that the UI does not detect as having errors, due to the errors being for other reasons than problems with metadata (e.g create view statement).

## Admin Features
Within the Cerebro UI, you have access to Admin features if **you have ALL access on a particular object (database or dataset)**.

### Review Group Access on Datasets

If there are *ANY* databases or datasets to which you have all access, you will see an **Admin** Tab on the datasets page. 

This will: 
- Filter the datasets list to only datasets that you have all access on
- Reveal the **"Review Access"** feature for datasets that you have Admin Access

<!-- TK IMAGE of review access--> 

When you click **Admin**, you will see a multi-select box under **Review Access** where you can select a group or set of groups to see which datasets those groups have access to. You can also filter by database to review group access to datasets **only** inside that database.

  >:heavy_exclamation_mark:Only groups with some level of access to the datasets you are admin on will be shown in this multi-select; any group not in the available list has no access to any of the datasets that are visible.
- A column is added to every dataset showing whether the group or groups being inspected can access any field in the dataset. 
  - ![Cerebro lock icon][lock] - Selected Group, or set of groups, has no access to this dataset
  - ![Cerebro check icon][check] - Selected Group, or set of groups, has *some level of access* to this dataset
  
  *Note that access is treated as a logical OR of the groups' individual access--if any of the selected groups have access to a dataset, you will see a checkmark for that group.*

### Review Group Access on Dataset Schema
If you have all access to a particular dataset, you will also see an 'Access' tab on the dataset details page where you can inspect which groups have access to which fields in the dataset (A similar experience to the dataset page). You will see:

![Cerebro Datasets Details Page][review-schema]

<!-- TK image of compare access -->

- A **multi-select box** where you can select a group or set of groups to see which fields in that dataset those groups have access to. Like the Reviewing access on the datasets page, only groups with some level of access to the dataset will be shown in this multi-select.
>:heavy_exclamation_mark:Any group not shown has no access to this dataset.
- **checkmark** or **lock** icons indicating whether the currently selected groups
have access to the field or not. 
  - ![Cerebro Lock icon][lock] - Selected Group or groups has no access to this field
  - ![Cerebro check icon][check]- Selected Group or groups has *some level of access* (Select or ALL) to this field. 

Additionally, the access tab lets you compare two different sets of groups' access at the same time by selecting different sets of groups in the two different access columns.
<!-- TK image of comparing access on schemas --> 

### Logging Out 
To logout, click your username in the upper right part of the screen, then click "Logout".

>:heavy_exclamation_mark:Your credentials are saved in the browser until either your token expires, or you explicitly log out. Be aware of this when sharing access to your computer.

### Troubleshooting
The Cerebro team really values your feedback. If you experience a problem with a particular aspect of the Web UI, or notice that features do not work as described or expected, please take a screenshot or video capture.

If possible, click the "About" link in the upper right part of the page and take note (e.g. another screenshot) of the details in the dialog that pops up. These version numbers will help to diagnose and fix issues that arise.

<!-- internal link references -->
[accountdetails]: resources/account-details.png
[token-home]: resources/3425CC6F6F5BD3429E4EEB5A5A2DB47E.png
[token-menu]: resources/FE79DD2E0FE4901DA2C5E7A34B1C66BA.png
[token-menu]: resources/FE79DD2E0FE4901DA2C5E7A34B1C66BA.png
[dataset-details]: resources/6518533AA6A7DD1767FEA685E56ABF28.png
[partioning-field]: resources/C7C78B02F6E027B8F69C4C2BC17BEB04.png
[schema-access]: resources/867E30D8DBBBF1095B3C5A9808D68482.png
[preview]: resources/5DB6CC99DA0D20DEAF9C7CAB895D7868.png
[get-started]: resources/03CC8BB78247AEEAB3968F71EE7FD8C1.png
[lock]: resources/3537D4FC730C7731B52560C6E5538F69.png
[check]: resources/CD56C394990B38B28A65CBCBAEC6805F.png
[review-schema]: resources/CE04A7DE2D59D4C8A48B843EFB4A02AE.png
