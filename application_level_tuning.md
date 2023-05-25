# Bind params and performance
1. Avoids sql injection
2. Avoids re compilation of query and cached execution plan is used
3. But if any parameter can strongly change the execution plan and query speed, it should not be a bind parameter(skewed/unbalanced histogram of values of a column
    1. For example - table has status column (majority of data will be of status = completed and minimal no of data will be “todo”). In this case status column where clause should not be a bind param, so that mysql can choose right index
4. Refer: https://jonathanlewis.wordpress.com/2009/05/06/philosophy-1/
