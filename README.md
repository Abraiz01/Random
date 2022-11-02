[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-c66648af7eb3fe8bc4f294546bfd86ef473780cde1dea487d3c4ff354943c9ae.svg)](https://classroom.github.com/online_ide?assignment_repo_id=9158670&assignment_repo_type=AssignmentRepo)
# Exam #1

The solutions content of this file below will be updated according to [the instructions](instructions/instructions.md).

## Solutions

The following sections contain a report on the solutions to each of the required components of this exam.

### Data munging

The code in the Python program, [solution.py](solution.py), contains the solutions to the **data munging** part of this exam.

### Spreadsheet analysis

The spreadsheet file, [users.xlsx](./data/users.xlsx), contains the solutions to the **spreadsheet analysis** part of this exam. In addition, the formulas used in that spreadsheet are indicated below:

- abide by [the instructions](./instructions/instructions.md#entering-respones-into-the-readme-file) for how to enter responses into this file correctly.
- **Make sure that all spreadsheet formulae you enter into this document work exactly as written.**

1. Total number of users of the social network

```
=COUNT(A:A)
```

2. Number of users in each of the states in the Pacific sub-region, which includes Alaska, California, Hawaii, Oregon and Washington.

```
=COUNTIF(H:H,N5)
=COUNTIF(H:H,N6)
=COUNTIF(H:H,N7)
=COUNTIF(H:H,N8)
=COUNTIF(H:H,N9)
```

3. Number of users in each of the given 5 cities of the USA: Nashville, Tennessee; San Diego, California; New York City, New York; Dallas, Texas; and Seattle, Washington.

```
=COUNTIFS(G:G,N13,H:H,O13)
=COUNTIFS(G:G,N14,H:H,O14)
=COUNTIFS(G:G,N15,H:H,O15)
=COUNTIFS(G:G,N16,H:H,O16)
=COUNTIFS(G:G,N17,H:H,O17)
```

4. The average affinity category IDs of all users in New York for each of the content types.

```
=AVERAGEIF(H:H,O20,I:I)
=AVERAGEIF(H:H,O20,J:J)
=AVERAGEIF(H:H,O20,K:K)
=AVERAGEIF(H:H,O20,L:L)
```

### SQL queries

This section shows the SQL queries that you determined solved each of the given problems.

- abide by [the instructions](./instructions/instructions.md#entering-respones-into-the-readme-file) for how to enter responses into this file correctly.
- **Make sure that all SQL commands you enter into this document work exactly as written, including semi-colons, where necessary.**

1. Write two SQL commands to create two tables named `users` and `affinity_categories` within the given database file.

```sql
CREATE table users (id INTEGER PRIMARY KEY, handle TEXT, first_name TEXT, last_name TEXT, email TEXT, street TEXT, city TEXT, state TEXT, state_animal TEXT, real_food_affinity_category_id TEXT, luxury_brand_affinity_category_id TEXT, tech_gadget_affinity_category_id TEXT, travel_affinity_category_id TEXT);
```

```sql
CREATE table affinity_categories (id INTEGER PRIMARY KEY, affinity_type TEXT, affinity REAL, cost_per_impression REAL, cost_per_thousand REAL);
```

2. Import the data in the `users.csv` and `affinity_categories.csv` CSV files into these two tables.

```sql
.import --csv --skip 1 data/users.csv users
```

```sql
.import --csv --skip 1 data/affinity_categories.csv affinity_categories
```

3. Display the state name and the number of users in that state for each of the states for which we have users.

```sql
select state, count (id) from users group by state;
```

4. Display the state name, the number of users in that state, and the average `travel_affinity_category_id` for each of the states for which we have users.

```sql
select state, count (id), avg (travel_affinity_category_id) from users group by state;
```

5. Display only the handles and last names of all users residing in Pittsburgh, Pennsylvania.

```sql
select handle, last_name from users where city = "Pittsburgh" and state = "Pennsylvania";
```

6. Display the email addresses of all users residing in Pittsburgh, Pennsylvania, along with the price the social network would charge an advertiser to show one advertisement to each of them, based on their `travel_affinity` level.

```sql
select users.email, affinity_categories.cost_per_impression from users inner join affinity_categories on users.travel_affinity_category_id=affinity_categories.id where city = "Pittsburgh" and state = "Pennsylvania";
```

7. Display the amount the social network would charge an advertiser to show two advertisement to three thousand users with a `real_food_affinity` level of `0.75`.

```sql
select cost_per_thousand * 2 * 3 from affinity_categories where affinity_type = "real_food_affinity" and affinity = 0.75;
```

8. Show all the users for whom the `tech_gadget_affinity_category_id` field contains an invalid foreign key.

```sql
select handle from users where tech_gadget_affinity_category_id == "NULL" or NOT (tech_gadget_affinity_category_id >= 1 or users.tech_gadget_affinity_category_id <= 16);
```

9. Write an additional SQL query of your choice using SQL with this table; then describe the results

An SQL query that shows handles of all users whose state animal is a Giant otter.

```sql
select handle from users where state_animal == "Giant otter";
```

### Normalization and Entity-relationship diagramming

This section contains responses to the questions on normalization and entity-relationship diagramming.

- abide by [the instructions](./instructions/instructions.md#entering-respones-into-the-readme-file) for how to enter responses into this file correctly.

1. Is the data in `users.csv` in fourth normal form?

```
No.
```

2. Explain why or why not the `users.csv` data meets 4NF.

```
it cant have different values for one category although the categories are independent of each other. the rest of the fields are not multi-valued for the same entity. so this does satisfy the 4th normal form. 
now does it satisfy 2 and 3?
is state animal a fact about state? yes. each state has its own state animal (ignoring the inconsistencies as specified in the question), then the state animal is a fact about the state and not about the user. so it violates the 3rd normal form.
```

3. Is the data in `affinity_categories.csv` in fourth normal form?

```
Enter your response here
```

4. Explain why or why not the `affinity_categories.csv` data meets 4NF.

```
Enter your response here
```

5. Use [draw.io](https://draw.io) to draw an Entity-Relationship Diagram showing a 4NF-compliant form of this data, including primary key field(s), relationship(s), and cardinality.

![Placeholder E-R Diagram](./images/placeholder-er-diagram.svg)
