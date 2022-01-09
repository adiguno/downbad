# downbad
- fix yo damn habits, one at a time

## Step 1 track it
- drinking, smoking, biting your nails... how many times do you do it everyday?
    - what is the name of your habit?                               name        : str
    - why is it bad for you or why do you want to get rid of it     intent      : str
    - what time do you usually do this?                             timestamp   : long/date
    - have you fixed this habit?                                    reformed    : bool

### Design Doc  
    1. Naive - SQL
        | name              | intent                           | timestamp     | reformed      | 
        |-------------------|----------------------------------|---------------|---------------|
        | picking yo nose   | don't want boogers everywhere    | 2am 1-2-2022  | false         | 
        | picking yo nose   | don't want boogers everywhere    | 7am 1-2-2022  | false         | 
        | picking yo nose   | don't want boogers everywhere    | 11am 1-2-2022 | false         | 
    - 1 row for every time it occurs is not efficient  
        - although... it is relational, and subscribe to '[first normal form](https://stackoverflow.com/questions/3070384/how-to-store-a-list-in-a-column-of-a-database-table)'
    - name and intent is duplicated, reformed is mostly dupliated
        - needs a map <day: count>

    2.  With occurence mapping - SQL
        | name              | intent                           | day        | count | reformed      | 
        |-------------------|----------------------------------|------------|-------|---------------|
        | picking yo nose   | don't want boogers everywhere    | 1-2-2022   | 5     | false         | 
        | picking yo nose   | don't want boogers everywhere    | 1-3-2022   | 4     | false         | 
    - scales linearly with time lol
    - loses nuance of when the habit is triggered, what time of day

    3. Hybrid
        | name              | intent                           | occurences: jsonb                      | reformed  | 
        |-------------------|----------------------------------|----------------------------------------|-----------|
        | picking yo nose   | don't want boogers everywhere    | {"timestamps": [2am 1-2-2022, ...]}    | false     | 
    - why not just NoSQL at this point?
    
    4. Postgres Arrays
        | name              | intent                           | occurences: long[]     | reformed  | 
        |-------------------|----------------------------------|------------------------|-----------|
        | picking yo nose   | don't want boogers everywhere    | [2am 1-2-2022, ...]    | false     | 
