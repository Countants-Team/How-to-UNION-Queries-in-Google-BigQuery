# How-to-UNION-Queries-in-Google-BigQuery
#####  In most relational databases, there may be a situations where you need to combine the results of multiple queries into one single dataset.In BigQuery, this occurs when you are fetching data from multiple tables or even across datasets, and this is where the power of using a Union comes into play.


### Now, we want the Final Out Put In this format:-

| Metrics | TY | LY | Comp |
| :---: | :---: | :---: | :---: |
| Sales | $125,256 | $101,253 | 24% |
| Visits | 4,251 | 3,892 | 9% |
| Orders | 1,532 | 1,203 | 27% |

### Where :
TY: This Year

LY: Last Year

Comp= (TY-LY)/LY. 

### Implementation:
Assuming all data sources contain different columns, we can query three different tables in the dummy dataset and combine the result set using the Standard SQL option with Google BigQuery requires a more redundant method when combining result sets.

### For example:

```
Select 
TY_Sales ,
LY_Sales ,
Comp
From
[project.dataset.table]

UNION ALL

TY_Visits ,
LY_Visits ,
Comp
From
[project.dataset.table]

UNION ALL

Select 
TY_Orders ,
LY_Orders ,
Comp
From
[project.dataset.table]
```
The query is invalid since all the queries contain the same number of columns  but column is of different data type(first two are integer and comp as string). So we have to convert it in to same data type.


The Cast syntax is used in a query to indicate that the result type of an expression should be converted to some other type.

### Syntax:-

#### CAST(expr as typename)


### For Example:

```
Select 
CAST(TY_Sales as string) As TY,
CAST(LY_Sales as string)  As LY,
Comp
From
[project.dataset.table]

UNION ALL

CAST(TY_Visits as string)  As TY,
CAST(LY_Visits as string)  As LY,
Comp
From
[project.dataset.table]

UNION ALL

Select 
CAST(TY_Orders as string) As TY,
CAST(LY_Orders as string) As LY,
Comp
From
[project.dataset.table]
```

Now, Suppose we want the above in specific order.
  
Create new dummy column.

###  For example:

```
Select 
‘A’ as ID,
CAST(TY_Sales as string) As TY,
CAST(LY_Sales as string)  As LY,
Comp
From
[project.dataset.table]

UNION ALL
select
‘B’ as ID,
CAST(TY_Visits as string)  As TY,
CAST(LY_Visits as string)  As LY,
Comp
From
[project.dataset.table]

UNION ALL

Select 
‘C’ as ID,
CAST(TY_Orders as string) As TY,
CAST(LY_Orders as string) As LY,
Comp
From
[project.dataset.table]

order by 1
```


Now, We want to print TY,LY,comp only.

A Select * EXCEPT( Column Name) statement specifies the names of one or more columns to exclude from the result. All matching column names are omitted from the output.

### For example:

```
Select * Except(ID) 
from

(Select 
‘A’ as ID,
CAST(TY_Sales as string) As TY,
CAST(LY_Sales as string)  As LY,
Comp

From
[project.dataset.table]

UNION ALL
select
‘B’ as ID,
CAST(TY_Visits as string)  As TY,
CAST(LY_Visits as string)  As LY,
Comp
From
[project.dataset.table]

UNION ALL

Select 
‘C’ as ID,
CAST(TY_Orders as string) As TY,
CAST(LY_Orders as string) As LY,
Comp
From
[project.dataset.table]

order by 1)
```
