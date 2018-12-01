ST_Crosses

1) Implemented ST_Crosses function in Predicates.scala.
As per the standards this function takes sequence of expression as input and returns if the left geometry crosses right geometry or not.
Initially the input expressions are coonverted into geometry type.
Then 'crosses' function of geometry class is used to check if the left geometry crosses the right geometry.
The logic for ST_Crosses can be found here https://postgis.net/docs/ST_Crosses.html

2) Implemented the "RangeJoinExec" for ST_Crosses in JoinQueryDetector.scala
Here the function checks if a planned spatial join can be applied for the given SQL function or not.
If the category ie fuction name ST_Crosses satisfies then "RangeJoinExec" is applied for a optimized join.

3) Added ST_Crosses in Catalog.scala for a user defined function
4) Added various test cases in predicateJoinTestScala.scala for ST_Crosses. These test cases utilizes "RangeJoinExec" for join plan

a) testing if a single point crosses a polygon
Here we are using testpoint.csv and testenvelope.csv for point and polygon dataframe.
Ideally a point cannot cross a polygon, so no rows are selected for ST_Crosses

b)Testing if a line crosses a given polygon
As there was no data with line crossing a polygon, I added a line in primaryroads.csv to test if the crosses function is working properly
For the given polygon the line does cross it so one row is selected for ST_Crosses

c)Testing if a given line crosses any polygon in testenvelope.csv
The line is formatted so that it crosses the first polygon of the dataset, hence one row is returned

d)Testing when line only touches a polygon but not crosses it
Ideally if a line just touches a polygon the crosses function should return false.
This is working in this test case and no rows are returned

e)Testing if a line crosses a line
The line is formatted such that it crosses the first line in primaryroads.csv
The output is one row returned

f)Testing if line touches a line
Ideally when a line touches a line the result of ST_Crosses should be false, hence no rows are returned

5)Added a test case for point and polygon in predicateTestScala.scala to directly test ST_Crosses without utilizing "RangeJoinExec"
As a point cannot cross polygon no rows are selected as output