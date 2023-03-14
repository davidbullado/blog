# Proposal for the introduction of a new clause in whatever-SQL

Sometimes when I write a query, I care very little about its optimization, especially when it comes to disposable queries. But I like to see some results just to reassure myself.

`SELECT my_big_function(name) FROM dummy LIMIT 10`
=> 10 rows, 1 second

OK, let's try a little more to see:

`SELECT my_big_function(name) FROM dummy LIMIT 50`
=> 50 rows, 6 seconds

OK, I can go a little further:

`SELECT my_big_function(name) FROM dummy LIMIT 1000`
=> 1000 rows, 2 hours

Oops, this time my_big_function encountered a parameter that caused the calculation time to explode. It is impossible to determine exactly when I should put my limit to return the maximum number of rows in a minute.

## Solution
In reality, the number of rows is not important to me; all I want is to have the maximum number of rows in a given time interval. That's why as a user it would make sense to have a clause LIMIT INTERVAL 60 SECONDS, which would return the maximum number of rows in a minute.

`SELECT my_big_function(name) FROM dummy LIMIT INTERVAL 60 SECONDS`
=> 425 rows, 1 minute

Et voilà !
