
**Union**
This is used to combine result of two statement into a single result, provided 
- The number and the order of the columns must be the same in both queries
- The data types of the corresponding columns must be the same or compatible.
By default union removes the duplicate rown but if you want the duplicate rows too use union_all.


**Intersect**
Used to get the common rown from two tables. and the conditions applied are same as Union.

**Except**
Used to get the result that is not in the other result set.
```kotlin
SELECT columns_names FROM table1 EXCEPT SELECT column_name FROM table2
```

Join
Used to combind the result from two or more tables based on the common column value between them.

Types :
Inner
Outer
Left
Right
Example Table -


| Roll Number | Name | Address | Phone | age |
| ---- | ---- | ---- | ---- | ---- |
| 1 | Harsh | Delhi |  | 18 |
| 2 | Pratik | Bihar |  | 19 |
| 3 | Riyanka | Siliguri |  | 20 |
| 4 | Deep | Ramnagar |  | 18 |
| 5 | Saptarhi | Kolkata |  | 19 |
| 6 | Dhanraj | Barabajar |  | 20 |
| 7 | Rohit | Belurghat |  | 18 |
| 8 | Niraj | Alipore |  | 19 |
|  |  |  |  |  |

Student Course

Course Id	Roll Number
1                  	1
2	                  2
2	                  3
3	                     4
1                     	5
4                      	9
5               	10
4	           11


**Inner**
Shows the data which is common between both the tables.

```sql
SELECT StudentCourse.COURSE_ID, Student.NAME, Student.AGE FROM Student
INNER JOIN StudentCourse
ON Student.ROLL_NO = StudentCourse.ROLL_NO;
```

Course Id	Name	Age
1	             Harsh	      18
2	            Pratik         	19
2             	Riyanka     	20
3	           Deep	     18
1          	Saptarhi	  19

**Left Join**
This join returns all the rows of the table on the left side of the join and matching rows for the table on the right side of join.

If there is no matching rows then the result set will be null.

```sql
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
LEFT JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```
Name	Course Id
Harsh	     1
Pratik	      2
Riyanka	 2
Deep	    3
Saptarhi	 1
Dhanraj	 null
Rohit     	null
Niraj	   null

Right Join
This join returns all the rows of the table on the right side of the join and matching rows for the table on the left side of join.

If there is no matching rows then the result set will be null.

```sql
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
RIGHT JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```

Name	Course Id
Harsh     	1
Pratik	    2
Riyanka	  2
Deep    	3
Saptarhi  	1
null	       4
null       	5
null	     4

**Full Join**
FULL JOIN creates the result-set by combining result of both LEFT JOIN and RIGHT JOIN. The result-set will contain all the rows from both the tables. The rows for which there is no matching, the result-set will contain NULL values.

```sql
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
FULL JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```

Name	Course Id
Harsh	        1
Pratik	        2
Riyanka	     2
Deep	        3
Saptarhi	    1
null	            4
null	           5
null	            4
Dhanraj	  null
Rohit	     null
Niraj     	null

