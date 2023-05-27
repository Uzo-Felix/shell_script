# INSTRUCTIONS

The first thing you need to do is start the terminal. Do that by clicking the "hamburger" menu at the top left of the screen, going to the "terminal" section, and clicking "new terminal". Once you open a new one, type echo hello SQL into the terminal and press enter.

In Part 1 of this tutorial, you created a students database and then a script to insert information about your computer science students into it. Log into the psql interactive terminal with psql --username=freecodecamp --dbname=postgres to see if it's here.

List the databases.

Your database isn't here. You can use the .sql file you created at the end of Part 1 to rebuild it. I recommend "splitting" the terminal. You can do that by clicking the "hamburger" menu at the top left of the window, going to the "Terminal" menu, and clicking "Split Terminal". Once you've done that, enter psql -U postgres < students.sql in it to rebuild the database.

A lot of stuff happened in the terminal. That looks promising. In the psql prompt, view the databases again.

There's your students database. Connect to it.

Now that you're connected. Display the tables and relations that are here to see if it's all correct.

That all looks right. View the details of the students table to make sure the stucture is right.

Looks good. Make sure all the data is in the table, as well.

The data is all there. You should take a look at the details of the other tables and the data in them to make sure they look good. When you are done, use touch in the bash terminal to create student_info.sh. You are going to make a script to print info about your students.

Give your new file executable permissions.

Add a shebang that uses bash at the top of your new script.

Below the shebang, add a comment that says Info about my computer science students from students database.

In the new script, use echo to print ~~ My Computer Science Students ~~. Use the -e flag with it to put a new line at the beginning and end of the text.

Run the script to make sure it's working.

You will want to query the database again to get info about the students to display. Add the same PSQL variable you use in your insert_data.sh script. It looked like this: PSQL="psql -X --username=freecodecamp --dbname=students --no-align --tuples-only -c"

Below the PSQL variable you just added, use echo to print First name, last name, and GPA of students with a 4.0 GPA:. Use the -e flag to put a new line at the beginning of the sentence.

You will want to print what that sentence is asking for. You should know how to make that query, but lets practice a little first. SQL stands for "Structured Query Language". It's the language you have been using to manage your relational databases. In the psql prompt, view all the data in the students table like you have done many times.

You should look at the column titles that were returned. The * gets all columns in a table with your query. You can return specific columns by putting the column name in the query instead of *. In the psql prompt, view just the first_name column from the students table.

Just the first_name column was returned that time. You can specify as many columns you want returned by separating them with commas. View the first_name, last_name and gpa columns from the students table.

You can return only rows you want by adding WHERE <condition> to your query. A condition can consist of a column, an operator, and a value. Use one of these to view the same columns as before but only rows WHERE gpa < 2.5.

The < only return rows where the gpa column was less than 2.5. Some other operators are: <, >, <=, >=. View the same columns, but only rows for students with a gpa greater than or equal to 3.8.

That only returned students with a GPA of 3.8 or better. There's equal (=) and not equal (!=) operators as well. View the same columns for students that don't have a 4.0 gpa.

The right query will get you only the data you are looking for. Back in your student_info.sh file, add an echo command to the bottom that prints what the sentence above it asks for. Place double quotes around it like this: echo "$($PSQL "<query_here>")". This will make it so the output isn't all on one line.

Run the script to see your students with the highest GPA's.

Add another echo statement at the bottom of the script. Make it print All course names whose first letter is before 'D' in the alphabet:. Put a new line in front of it like the first sentence.

Practice first. In the psql prompt, view all the data in the majors table.

The operators you used with numbers in the last section can be used on text as well. Use the = to view all majors named Game Design. Don't forget that You need single quotes around text values.

Next, view all the rows not equal to Game Design.

Use the greater than operator to see majors that come after it alphabetically.

Game Design was not included in the results because it is not > 'Game Design'. Try it with the greater than or equal to operator.

It included Game Design in the results that time. So if you want to see results that start with a G or after, you could use major >= 'G'. View the majors that come before G.

In your script, add an echo at the bottom to print the suggested info like you did before. Make sure to use double quotes where needed.

Run the script to see what course names come before the letter D.

Looks like there is five of them. Add another sentence like the others that says: First name, last name, and GPA of students whose last name begins with an 'R' or after and have a GPA greater than 3.8 or less than 2.0:

To find that, start by using the psql prompt to view all the data in the students table.

It returned 31 rows. Use the same command, but only return the rows for students whose last name comes before M in the alphabet.

That returned 18 rows. You can use multiple conditions after WHERE with AND or OR, among others. Just add the keyword and another condition. In the psql prompt, use the same command as before, but add an OR to also return rows of students with a 3.9 GPA.

It showed rows where one of the conditions was true, there was one more than last time. Enter the previous command, but use AND to view only students that meet both conditions.

Now it only shows rows where both conditions are true, one person. Enter the previous command, but add a third condition of OR gpa < 2.3.

This showed all students whose GPA is less than 2.3 because the final OR condition was true for them. It didn't matter what their last name started with. You can group conditions together with parenthesis like this: WHERE <condition_1> AND (<condition_2> OR <condition_2>). This would only return rows where <condition_1> is true and one of the others is true. View students whose last name is before M that have a GPA of 3.9 or less than 2.3.

Two students meet those conditions. Back in the student info file, add an echo command at the bottom to print the suggested rows.

Run the script to see the results.

Moving along. Add another echo command, like the others, with a sentence that says: Last name of students whose last name contains a case insensitive 'sa' or have an 'r' as the second to last letter:

Start by viewing everything from the courses table in the psql prompt to see how you might be able to find this out.

There's a few that contain the word Algorithms. You can use LIKE to find patterns in text like this: WHERE <column> LIKE '<pattern>'. An underscore (_) in a pattern will return rows that have any character in that spot. View the rows in this table with a course name that matches the pattern '_lgorithms'.

That pattern matched only rows that had exactly one character, followed by lgorithms. Another pattern character is %. It means anything can be there. To find names that start with W, you could use W%. View the courses that end in lgorithms.

It found two that time. Try viewing courses that start with Web.

Combine the two pattern matching characters to show courses that have a second letter of e.

Nice job! Try viewing the courses with a space in their names.

There they are. You can use NOT LIKE to find things that don't match a pattern. View courses that don't contain a space.

Five courses without a space. Try finding the ones that contain an A.

6 rows. This showed all the courses with a capital A. ILIKE will ignore the case of the letters when matching. Use it to see the courses with an A or a.

It found 11 rows that time. You can put NOT in front of ILIKE as well. Use it to see the courses that don't contain an A or a.

You combine these like any other conditions. View the courses that don't have a capital or lowercase A and have a space.

In your student info script, add an echo statement at the bottom like the other to print the results of the suggested query.

Run the script to see the results.

Looks like five students meet those conditions. Add another echo command at the bottom, like the others. Make this one say: First name, last name, and GPA of students who have not selected a major and either their first name begins with 'D' or they have a GPA greater than 3.0:

Start by looking at all the data in the students table.

All the fields that are empty or blank are null. You can access them using IS NULL as a condition like this: WHERE <column> IS NULL. View the students who don't have a GPA.

Inversely, you can use IS NOT NULL to see rows that aren't null. View all the info on students that do have a GPA.

View all the info on students who haven't chosen a major.

View the students who don't have a major, but don't include students without a GPA.

One more. View the students who don't have a major and gpa.

In your script, add an echo command at the bottom to print the results the sentence is looking for.

Run the script to see the students that meet those conditions.

There's three of them. Add another sentence, like the others that says Course name of the first five courses, in reverse alphabetical order, that have an 'e' as the second letter or end with an 's':

You can specify the order you want your results to be in by adding ORDER BY <column_name> at the end of a query. In the psql prompt, view all the info in the students table in order by the GPA's.

That put the lowest GPA's at the top. When using ORDER BY, it will be in ascending (ASC) order by default. Add DESC (descending) at the end of the last query to put the highest ones at the top.

Now, the highest GPA's are at the top. You can add more columns to the order by separating them with a comma like this: ORDER BY <column_1>, <column_2>. Any matching values in the first ordered column will then be ordered by the next. View all the student info with the highest GPA's at the top, and in alphabetical order by first_name if the GPA's match.

Many times, you only want to return a certain number of rows. You can add LIMIT <number> at the end of the query to only get the amount you want. View the students in the same order as the last command, but only return the first 10 rows.

The order of the keywords in your query matters. You cannot put LIMIT before ORDER BY, or either of them before WHERE. View the same number of students, in the same order, but don't get the ones who don't have a GPA.

In your script, add the echo command to print the rows the sentence is asking for.

Run the script to see the courses.

😎 Add another echo command at the bottom of the script like the others. Make this one say, Average GPA of all students rounded to two decimal places:

There's a number of mathematic functions to use with numerical columns. One of them is MIN, you can use it when selecting a column like this: SELECT MIN(<column>) FROM <table>. It will find the lowest value in the column. In the psql prompt, view the lowest value in the gpa column of the students table.

Another one is MAX, use it to see the largest gpa of the same table.

In the same fashion, use a SUM function find out what all the values of the major_id column in the students table add up to.

AVG will give you the average of all the values in a column. Use it to see the average of the same column.

You can round decimals up or down to the nearest whole number with CEIL and FLOOR, respectively. Use CEIL to round the average major_id up to the nearest whole number. Here's an example: CEIL(<number_to_round>).

Or, you can round a number to the nearest whole number with ROUND. Use it to round the average of the major_id column to the nearest whole number.

You can round to a specific number of decimal places by adding a comma and number to ROUND, like this: ROUND(<number_to_round>, <decimals_places>). Round the average of the major_id to five decimal places.

You should be able to find what your script is asking for now. Add the command to print it.

Run the script to see the average GPA of all your students.

They're doing pretty good. Add another command to print Major ID, total number of students in a column named 'number_of_students', and average GPA rounded to two decimal places in a column name 'average_gpa', for each major ID in the students table having a student count greater than 1:

Another function is COUNT. You can use it like this: COUNT(<column>). It will tell you how many entries are in a table for the column. Try it out in the psql prompt by using COUNT(*) to see how many majors there are.

Using the same method, check how many students you have.

Using * like that told you how many total rows are in the table. View the count of the major_id column in the students table to see how many of your students have picked a major.

Using major_id didn't count the null values in that column. 23 students have a major. DISTINCT is a function that will show you only unique values. You can use it like this: DISTINCT(<column>). View the unique major_id values in the students table.

There's six unique major_id values in the students table. You can get the same results with GROUP BY. Here's an example of how to use it: SELECT <column> FROM <table> GROUP BY <column>. Use this method to view the unique major_id values in the students table again.

The output was the same as DISTINCT, but with GROUP BY you can add any of the aggregate functions (MIN, MAX, COUNT, etc) to it to find more information. For instance, if you wanted to see how many students were in each major you could use SELECT COUNT(*) FROM students GROUP BY major_id. View the major_id column and number of students in each major_id.

When using GROUP BY, any columns in the SELECT area must be included in the GROUP BY area. Other columns must be used with any of the aggregate functions (MAX, AVG, COUNT, etc). View the unique major_id values with GROUP BY again, but see what the lowest GPA is in each of them.

Nice job. Enter the same query, but add a column that shows you the highest GPA in each major as well.

Another option with GROUP BY is HAVING. You can add it at the end like this: SELECT <column> FROM <table> GROUP BY <column> HAVING <condition>. The condition must be an aggregate function with a test. An example to might be to use HAVING COUNT(*) > 0 to only show what whatever column is grouped that have at least one row. Use HAVING to only show rows from the last query that have a maximum GPA of 4.0.

Two of your majors have at least one student with a 4.0 GPA. Looking at the results, the column is named min. You can rename a column with AS like this: SELECT <column> AS <new_column_name> Enter the same command, but rename the min column to min_gpa.

Now the column has a better name. Enter the same command, but rename the max column to max_gpa as well.

That's more descriptive. View the major_id and number of students in each major_id in a column named number_of_students.

Use HAVING with the last query to only show the rows with less than eight students in the major.

Well done. Back in your script, add the command the print the suggested results.

Run the script to see the output.

Add an echo command to your script like the others that prints List of majors, in alphabetical order, that either no student is taking or has a student whose first name contains a case insensitive 'ma':

The majors and students table are linked with the major_id foreign key. If you want to see the name of a major that a student is taking, you need to JOIN the two tables into one. Here's an example of how to do that:
SELECT * FROM <table_1> FULL JOIN <table_2> ON <table_1>.<foreign_key_column> = <table_2>.<foreign_key_column>;

In the psql prompt, join the two tables together with the above method.

It's showing all the columns from both tables, the two major_id columns are the same in each row for the ones that have it. You can see that there are some students without a major, and some majors without any students. The FULL JOIN you used will include all rows from both tables, whether or not they have a row using that foreign key in the other. From there, you could use any of the previous methods to narrow down, group, order, etc. Use a LEFT JOIN to join the same two tables in the same way.

There's a few less rows than the last query. In the LEFT JOIN you used, the students table was the left table since it was on the left side of the JOIN. majors was the right table. A LEFT JOIN gets all rows from the left table, but only rows from the right table that are linked to from the left one. Looking at the data, you can see that every student was returned, but the majors without any students were not. Join the same two tables with a RIGHT JOIN this time.

The right join showed all the rows from the right table (majors), but only rows from the left table (students) if they have a major. There's one more type you should know about. Join the two tables with an INNER JOIN.

The INNER JOIN only returned students if they have a major and majors that have a student. In other words, it only returned rows if they have a value in the foreign key column (major_id) of the opposite table. You should know a little about the four main types of joins now. Try using a LEFT JOIN to show all the majors but only students that have a major.

Excellent. All the majors are there. Next, use the appropriate join to show only students that are enrolled in a major, and only majors that have a student enrolled in it.

👍 Try using a right join to show all students but only majors if a student is enrolled in it.

That showed all the students since it was the right table of the RIGHT JOIN. Use the appropriate join with the same two table to show all rows in both tables whether they have a value in the foreign key column or not.

Lets do some more experiments with joins. Say you wanted to find a list of majors that students are taking. Use the most efficient JOIN to join the two tables you need. Only join the tables for now, don't use any other conditions.

Good. To get the list, you don't need all the columns, though. Enter the same command, but just get the column you need.

You also don't want any duplicates. Use DISTINCT to only return the unique ones to see the list of majors who have students.

There's your list of majors that students are taking 😄 Next, say you wanted a list of majors that students aren't taking. Use the most efficient JOIN to join the two tables you need. Only join the tables for now, don't use any other conditions.

That got you all the majors, you can see the ones that don't have any students. Add a WHERE condition to only see the majors without students, use student_id in it's condition.

Now you only have the rows you need. Only get the columns you need with it to see the list of majors without students.

You're doing great. Next, use the most efficient 'JOIN' to join the tables you would need if you were asked to get the first name, last name, major, and GPA of students who are taking Data Science or have a gpa of 3.8 or greater. Only join the tables for now, don't use any other conditions.

Enter the same command, but use WHERE to only get the students that meet the requirements. As a reminder, the goal was to find students who are taking Data Science or have a gpa of 3.8 or greater.

Now, you have narrowed it down the rows you are looking for. Enter the same command, but only get the columns you need. There was four of them, the students first name, last name, their major, and GPA. Get them in that order.

From there, you could put them in a specific order if you wanted or limit the results to a certain number among other things. Lastly, use the most efficient 'JOIN' to join the tables you would need if you were asked to get the first name and major for students whose first_name, or the major, contains ri. Only join the tables for now, don't use any other conditions.

Add a WHERE to the previous query so you only get the rows you need. The rows you wanted were the ones with a first name or major containing ri.

Finally, you only wanted to display the first_name and major columns. Enter the previous query, but only get the columns you need.

In your script, add the command to print what the sentence is asking for.

Run the script to see the majors described.

😄 Almost done. In your script, add a command to print this sentence like the others: List of unique courses, in reverse alphabetical order, that no student or 'Obie Hilpert' is taking:

Lets go over a few more things before you figure out how to see the courses a student is taking. Start by doing a FULL JOIN on your students and majors tables.

If you look at the column names, it shows two major_id columns. One from the students table and one from the majors table. If you were to try and query it using major_id, you would get an error. You would need to specify what table you want the column from like this: <table>.<column>. Enter the same join but only get the major_id column from the students table.

Earlier, you used AS to rename columns. You can use it to rename tables, or give them aliases, as well. Here's an example: SELECT * FROM <table> AS <new_name>;. Enter the same query you just entered, but rename the majors table to m. Anywhere the majors table is referenced, you will need to use m instead of majors.

This doesn't affect the output. It can just make some queries easier to read. Enter the same query, but rename the students table to s as well.

There's a shortcut keyword, USING to join tables if the foreign key column has the same name in both tables. Here's an example: SELECT * FROM <table_1> FULL JOIN <table_2> USING(<column>);. Use this method to see all the columns in the students and majors table. Don't use any aliases.

Note that the two major_id columns were turned into one with USING. In order to find out what courses a student is taking, you will need to join all the tables together. You can add a third table to a join like this: SELECT * FROM <table_1> FULL JOIN <table_2> USING(<column>) FULL JOIN <table_3> USING(<column>). This example will join the first two tables into one, turning it into the left table for the second join. Use this method to join the two tables from the previous query with the majors_courses table.

You may need to adjust the terminal size to align the output. What you're seeing is every unique combination of rows in the database. Students with a major are listed multiple times, one for each course included in the major. The majors without any students are there along with the courses for them. The students without a major are included, they have no courses and are only listed once. You can join as many tables together as you want. Join the last table to the previous command to get the names of the courses with all this info.

Same amount of rows, but you get the course names now. In your script, add the command to print the suggested info.

Run the script to see courses described.

Last one. Add a command that prints List of courses, in alphabetical order, with only one student enrolled:.

Go for it.

This is the last step, you have done really well. Run the script one last time. 👋