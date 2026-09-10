# Data-Cleaning-formatting-visualization
Data cleaning, standardizing and analysis in Excel. Dashboard development in Tableau.
#### Methods

- Exploratory Data Analysis
- Data cleaning, formatting & standardization
- Aggregations
- Calculated fields and formulas
- Data visualization

#### Tools

- Excel: data cleaning, formatting & preparation
- Tableau: analysis and dashboard development

#### **Data Source**

- Kaggle 

# Process Documentation

#### **Exploratory Data Analysis**

Initial review of the dataset showed several issues in quality and formatting that needed to be fixed before analysis and visualization:

- Errors, blanks and unknowns values
- Inconsistent or missing data types, fields that were not appropriately formatted
- Location pertains to order type rather than a geo-location which misleads.

### 2. Cleaning & formatting

Checked dataset for duplicate records.

- **0 duplicate records were found**, so no fields were removed.

!Missing item.png

### 3. Handling Missing Values and Errors

Noted several blank fields, `UNKNOWN` and `ERROR` values. Since we don’t actually know why a field is error vs unknown, it’s best not to remove or make guesses. Instead, I standardized them using a conditional logic:

- Blank or `UNKNOWN` values → `Unknown`
- `ERROR` values → `Needs Review`
- Valid values → No change needed
    
    ```json
    =IF(OR(C2="",UPPER(C2)="UNKNOWN"),"Unknown",IF(UPPER(C2)="ERROR","Needs Review",C2))
    ```
    

### 4.  Renaming Columns

Renamed **Location** column to **Order Type** since values represent the type of order (like takeout vs dining in-store) rather than a geographical location. This prevents the field from being misinterpreted as geographic data during analysis or visualization.

### 5. Formatting and Validating Price Data

**Price** column contained numerical values *and* non-numerical values. Formatted valid numerical values as **currency** to make the field easier to interpret while preserving numerical data type for future calculations.

Created a *Price Issue* column to document why a value was excluded from the cleaned dataset. 

```json
=IF(OR(D2="",UPPER(D2)="UNKNOWN",UPPER(D2)="ERROR"),"",D2)

//This formula checks if the original value is blank, "UNKNOWN", or "ERROR". 
//It leaves the cell blank if the value is invalid, otherwise fills with the 
//original numeric value.
```

```json
=IF(D2="","Unknown",IF(UPPER(D2)="ERROR","Price issue",""))

//This formula flags missing values as Unknown, invalid values like error 
// as Price Issue, and leaves the cell blank when the price is Valid.
```

**5. Validating Total Spent and Quantity**
Updated *Total Spent* column as some fields show errors but can actually be easily calculated by multiplying the item quantity with price per unit *if* there is data in both fields. Updated Quantity column for same reason. 

```json
Total Spent = Quantity × Price per Unit
//shows how much customer spent on order
```

**Some blanks intentionally left in case we want to do calculations, we won’t have columns mixed with integers and strings, which may cause errors later on. 

Highly recommend digging into unknowns or *Needs Review* to find appropriate the data.

#### Result:

- Duplicate records were checked and confirmed.
- Column names were standardized and clarified.
- **Location** was renamed to **Order Type** to accurately represent the underlying data.
- Missing and invalid values were consistently identified.
- Conditional formulas and logical functions were used to standardize and flag data-quality issues.
- Numerical fields were preserved as numerical values where possible.
- Price values were formatted as currency.
- Data-quality issues documented rather than removed.


# Dashboard visualization
#### Findings

1. **Which order types generate the most business?**
- Both takeout and in-store orders generate about the same revenue of ~ $24,000 in the calendar year.
1. **Are sales increasing or decreasing over x amount of time?**
- Sales were generally steady throughout the year.
1. **Which menu items contribute the most to total sales?**
- Salads are the highest-selling menu item by revenue, with about $17.3K in total sales, with sandwiches and smoothies following.

### Recommendation:

- Revisit missing data, errors or unknowns for a more precise overview.
- Salads are the highest priced and highest revenue item. This tells us that our customers are more likely to opt for healthy food options. Consider introducing similar healthy options.

## Findings

#### 1. Which order types generate the most revenue?
- Takeout and in-store orders generated about the same revenue around **$24,000** each over the calendar year. Neither order type significantly outperformed the other in terms of total revenue.

#### 2. Are sales increasing or decreasing over time?
- Sales remained relatively steady and consistent throughout the year, with no clear trends.

#### 3. Which menu items contribute the most to total revenue?
- Salads generated the highest total revenue at **$17.3K.** Sandwiches and smoothies followed salads as the next highest-revenue menu categories.
  
1. Which menu items have the highest sales volume?
Coffee actually had the highest sales volume at 3,534 units. This shows that the menu item generating the most revenue is not necessarily the item with the highest sales volume. 

##### Recommendations:
The business seems to be performing well and consistently, but here are some areas worth investigating:

- Missing, unknown, and error values to improve the accuracy of findings and provide a more complete picture of performance.
- People buy coffee the most but salads make the most money, so monitoring both items and comparing their prices and sales volume will help us better understand what is driving revenue.
- Monitor sale trends and performance throughout the year.

