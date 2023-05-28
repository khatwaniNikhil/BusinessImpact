# Business Problem
Give support for showing per tenant multiple views of data which is customisable on the fly via config. Avoiding release every time.

# Solution
1. Config:BASE (XML)
    1. Hidden columns for aggregating data
    2. SQL Query related
        1. JOINS(applied on all views, tenant level add-ons possible) 
        2. columns(tenant level customisable)
        3. where clause(tenant level customisable)
    3. Config:VIEW(tenant specific mongo)
        1. resultsPerPage
        2. SQL QUERY related
            1. JOINS
            2. columns: per view customisable(pick subset from base list)
            3. where clause:  per view customisable(pick subset from base list)
            4. optional dynamic filters
            5. optional where clause
            6. sortColumns
