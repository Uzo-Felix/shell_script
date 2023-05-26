use "TRUNCATE <table> CASCADE" to delete recursively

instructions

Review
The first thing you need to do is start the terminal. Do that by clicking the "hamburger" menu at the top left of the screen, going to the "terminal" section, and clicking "new terminal". Once you open a new one, type echo hello SQL into the terminal and press enter.

You are started with two .csv files with info about your computer science students. You should take a look at them. The top row in each file has titles, and the rest are values for those titles. You will be adding all that info to a PostgreSQL database. Log into the psql interactive terminal with psql --username=freecodecamp --dbname=postgres to get started.

View the existing databases with the \l shortcut command to see what's here.

All the info from the CSV files will go into a single database. Create a new database named students.

View the databases again to make sure it got created.

There it is. Connect to your new database so you can start adding tables.

The CSV files have a bunch of students with info about them, and some courses and majors. You will have four tables. One for the students and their info, one for each major, another for each course, and a final one for showing what courses are included in each major. First, create the students table.

The second table will be for each unique major that appears in the data. Create a table named majors.

The third table is for each unique course in the data. Create another table named courses.

The final table will be a junction table for the majors and courses. Create it with the name majors_courses.

Use the display shortcut command to view your tables to make sure your satisfied with them.

Onto the columns. The students.csv file has four fields, you will make a column for each of those as well as an ID column. Add a column to your students table named student_id. Give it a type of SERIAL so it automatically increments and make it a PRIMARY KEY

The first column in students.csv is first_name. Add a column to the students table with that name. Make it a type of VARCHAR(50) and give it the NOT NULL constraint.

The next column in the data is last_name. Add it to the students table. Give it the same data type and max-length as first_name and make sure it has the NOT NULL constraint.

The next column is for the major. Since you will have each major in another table this column will be a foreign key that references it. Create a column in the students table named major_id, give it a data type of INT for now. You will come back and set the foreign key later.

Create the last column, gpa. The data in the CSV shows that they are decimals with a length of 2 and 1 number is to the right of the decimal. So give it a data type of NUMERIC(2,1).

Use the shortcut command to display the details of the students table to make sure you like it.

The foreign key is still missing. Let's fill in the majors table next. Add a major_id column to it. Make it a type of SERIAL and the PRIMARY KEY for this table.

This table will only have one other column for the name of the major. Add a column to it named major. Make it a VARCHAR with a max-length of 50 and give it the NOT NULL constraint.

View the details of the majors table to make sure you like it.

This table looks good. Now, set the major_id column from the students table as a foreign key that references the major_id column from the majors table. Here's an example of how to do that: ALTER TABLE <table_name> ADD FOREIGN KEY(<column_name>) REFERENCES <referenced_table_name>(<referenced_column_name>);

View the details of the students table again to make sure the key is there.

Next, is the courses table. Add a course_id column to it. Give it a type of SERIAL and make it the primary key.

Add a course column to the courses table that's a type of VARCHAR. The course names are a little longer, so give them a max-length of 100. Also, make sure it can't accept null values.

View the details of the courses table to make sure it looks good.

One more table to go. The majors_courses junction table will have two columns, each referencing the primary key from two related table. First, add a major_id column to it. Just give it a type of INT for now.

Set the major_id column you just created as a foreign key that references the major_id column from the majors table.

Next, add a course_id column to the same table. Just give it a type of INT again for now.

Set your new course_id column as a foreign key that references the other course_id column.

View the details of the table you just worked on to make sure the structure is finished.

There's one thing missing. This table doesn't have a primary key. The data from courses.csv will go in this table. A single major will be in it multiple times, and same with a course. So neither of them can be a primary key. But there will never be a row with the same two values as another row. So the two columns together, are unique. You can create a composite primary key that uses more than one column as a unique pair like this: ALTER TABLE <table_name> ADD PRIMARY KEY(<column_name>, <column_name>); Add a composite primary key to the table using the two columns.

View the details of the table again.

Okay, now it's finished. View all the tables you ended up with.

Next, you can start adding some info. Since the students table needs a major_id, you can add a major first. View the details of the majors table to see what info it expects.

It only needs the name of a major. The ID will be added automatically. Add the first major from the courses.csv file into the majors table. It's a VARCHAR, so make sure to put the value in single quotes.

Use SELECT to view all the data in the majors table to make sure it got inserted correctly.

Next, insert the first course from courses.csv into the courses table.

View all the data in the courses table to make sure it got added.

Next, you can add a row into the junction table. View the details of it to see what it expects.

It wants a major_id and course_id. Add a row to majors_courses for the first entry in courses.csv.

View all the data in the table you just added to.

Looks like the row got added. View the details of the students table to remind yourself what it expects so you can add the first student to the database.

The output shows what the table needs. Insert the first person from students.csv into the students table.

Looks like it worked. View all the data in the students table to make sure.

Okay, you added a row into each table. It might be wise to review the data and the database structure. Adding the rest of the info one at a time would be tedious. You are going to make a script to do it for you. I recommend "splitting" the terminal for this part. You can do that by clicking the "hamburger" menu at the top left of the window, going to the "Terminal" menu, and clicking "Split Terminal". Once you've done that, use the touch command to create a file named insert_data.sh in your project folder.

You should have two terminals open. One connected to PostgreSQL, and one for entering terminal commands. In the one for terminal commands, use the chmod command with the +x flag to give you new script executable permissions.

Open your new file and add a "shebang" that uses bash at the top. It looks like this: #!/bin/bash.

Below that, add a single line comment with the text, Script to insert data from courses.csv and students.csv into students database.

First, you should add all the info from the courses.csv file since you need the major_id for inserting the student info. cat is a terminal command for printing the contents of a file. Here's an example: cat <filename>. Below the comment you added, use it to print courses.csv.

Run your script to see if the file contents get printed.

It worked. Instead of printing the content, you can pipe that output into a while loop so you can go through the rows one at a time. It looks like this:

cat courses.csv | while read MAJOR COURSE
do
  <STATEMENTS>
done
Each new line will be read into the variables, MAJOR and COURSE. Add the above to your cat command. In the STATEMENTS area, use echo to print the MAJOR variable.

Run the script to see if it worked.

It's looping, but the MAJOR variable is only being set to the first word. There's a default IFS variable in bash. IFS stands for "Internal Field Separator". View it with declare -p IFS.

This variable is used to determine word boundaries. It defaults to spaces, tabs, and new lines. This is why the MAJOR variable was set to only the first word on each line from the data. Between the while and read commands, set the IFS to a comma like this: IFS=","

Now, it should use the comma in the data to separate words instead of spaces. Run the script again to see if it's working.

Looks like that worked. It prints the whole major, including the space. Print the COURSE variable on the same line as where you print MAJOR to make sure it's all working.

Run the script again to check.

Okay, your loop is working. You can use the MAJOR and COURSE variables to access the major or course when you need to insert data or query the database. Delete the echo line so you can figure out what to do next.

It helps to plan out what you want to happen. For each loop, you will want to add the major to the database if it isn't in there yet. Same for the course. Then add a row to the majors_courses table. Add these single line comments in your loop in this order: get major_id, if not found, insert major, get new major_id, get course_id, if not found, insert course, get new course_id, insert into majors_courses.

You used the psql command to log in and interact with the database. You can use it to just run a single command and exit. Above your loop, add a PSQL variable that looks like this: PSQL="psql -X --username=freecodecamp --dbname=students --no-align --tuples-only -c". This will allow you to query your database from your script. The important parts are the username, dbname, and the -c flag that is for running a single command and exiting. The rest of the flags are for formatting.

Now, you can query your database using the PSQL variable like this: $($PSQL "<query_here>"). The code in the parenthesis will run in a subshell, which is a separate bash process. Below the get major_id comment in your loop, create a MAJOR_ID variable. Set it equal to the result of a query that gets the major_id of the current MAJOR in the loop. Make sure to put your MAJOR variable in single quotes.

Below the variable you just created, use echo to print it so you can see it's value when you run the script.

Run the script to see what happens.

So it went through each major from the CSV file and tried to find major_id for each one from the database. Looks like it only found the one you manually inserted earlier. The rest were empty. Below your first if not found comment, add an if condition that checks if the MAJOR_ID variable is empty. You can do that with this test: [[ -z $MAJOR_ID ]]. Place the next two comments in the statements area of the if.

The loop will go into this if whenever a major isn't found. Here, you will want to insert the major and then get the new id. You will need the ID for inserting data into the majors_courses table later. Below your insert major comment, create an INSERT_MAJOR_RESULT variable. Set it's value to a query that inserts the current major into the database. Don't forget to use single quotes around the value.

Below the variable you just created, use echo to print it.

Instead of running through all the data in the CSV file, you should make some test data. In the terminal, use the copy (cp) command to copy the courses.csv into a new file named courses_test.csv.

In your new file, remove all the data except for the first five lines. Make sure there's a single empty line at the bottom.

Back in the insert_data.sh script, change your cat command to loop through the test file instead of the full one.

Run the script. It will go through the test data and insert a major into the database each time it doesn't find one already there and print the MAJOR_ID and INSERT_MAJOR_RESULT variables.

Looks like it found an ID that was already in the database twice and inserted three new items into the database. You don't need to print the ID anymore so delete the echo $MAJOR_ID line.

In the psql prompt, use SELECT to view all the data from the majors table to see what the script added.

I forgot you inserted Database Administration earlier. The script ran and inserted major from the top line of the file. Then it added the other two that weren't already in there. You can use TRUNCATE to delete all data from a table. In the psql prompt, try to delete all the data in the majors table by entering TRUNCATE majors;

It says you "cannot truncate a table referenced in a foreign key constraint." The students and majors_courses tables use the major_id from majors as a foreign key. So if you want to delete the data from majors, you need to delete the data from those two tables at the same time. Use TRUNCATE to delete the data from those three tables. Separate the tables with commas.

View all the data in the majors table to make sure it's empty.

Looks like it worked. View all the data in the majors_courses table to see if that one is empty.

It is, check the students table.

Last, check the courses table.

There should still be one entry in there. Use TRUNCATE to delete all the data from the courses table. You will need to truncate any tables that use a column from it as a foreign key at the same time.

View all the data in the courses table again.

Now the database is completely empty. Run the script again to see what gets inserted when the database is empty.

It inserted four that time. In the psql prompt, view all the data in the majors table.

You won't want to add the first line from the CSV file to the database since those are just titles. In your script, add an if condition at the top of your loop that checks if $MAJOR != major. Put all the existing code and comments in your loop in it's statements area so it only does any of it if it's not the first line.

In the psql prompt, use TRUNCATE to delete all the data in the majors table.

View all the data in majors table to make sure it's empty.

Run the script to make sure it's not adding the first line anymore.

It only showed three inserts, that's a good sign. View all the data in majors table to make sure it's three you want.

There's three unique majors in your test data. Those were the three added to the database, so it looks like it's working. Delete the line where you print INSERT_MAJOR_RESULT.

You want a nicer message when something get's inserted so it's more informative. Below your INSERT_MAJOR_RESULT variable, add an if statement that checks if the variable is equal to INSERT 0 1, which was what it was printing. Use echo to print Inserted into majors, $MAJOR in the statements area of the if.

In the psql prompt, truncate the majors table again so you can run the script and see the output.

Check to make sure the table is empty. Then, run the script.

It's starting to come together. Below your get new major_id comment, set the MAJOR_ID variable to a query that gets the new major_id from the database.

So the script will insert the majors correctly. Next are the courses. It will be the same steps as for the majors. Below your get course_id comment, add a COURSE_ID variable that gets the course_id from the database. Remember that your COURSE variable will have the current course in the loop.

It's the same as the majors, so below the second if not found comment, add an if statement that checks if the query was empty so you can insert the course if needed. Place the existing insert course and get new course_id comments in the statements area of the if.

Below the insert course comment, create an INSERT_COURSE_RESULT variable that inserts the course into the database.

The variable should be INSERT 0 1 again if something gets inserted. Below the variable you just created, add an if condition that checks if it is and print Inserted into courses, $COURSE using echo in it's statements area.

In the psql prompt, truncate the data from the majors table so you can run the script again.

Run the script to see if the courses get inserted into the database.

It looks like it worked. The test data has three unique courses, and three got added to the database. View the data in the courses table to make sure they are correct.

Excellent. Instead of manually deleting the data each time you want to run the script, add the command to do it for you. Near the top of the file below your PSQL variable, use echo to query the database. In the query, truncate your four tables in this order: students, majors, courses, majors_courses.

Run the script to see if it works.

Awesome. That makes it easier. Below your get new course_id comment, set the COURSE_ID to the newly inserted course_id.

One more thing to add for this file. Below the insert into majors_courses courses comment, create a INSERT_MAJORS_COURSES_RESULT variable. Use it and the MAJOR_ID and COURSE_ID variables you created to insert a row into the majors_courses table. Make sure the query has the major_id column first. Also, you won't need any quotes around the values for the ID's.

Below the variable you just created, add an if condition that checks if it's equal to INSERT 0 1 like the others. In it's statements area, use echo to print Inserted into majors_courses, $MAJOR : $COURSE.

Run the script. Your tables should get truncated and then it should go through the loop and add all the data from the courses_test.csv into the three tables of the database.

Looks like it works. You better look around to make sure. View the data in the majors table.

Cool, check the courses table.

Lastly, view the data in the majors_courses table. There should be four rows.

Alright, that part of the script is done. Next, you need to add everything from the students.csv file. Make some test data again. In the terminal, use the copy command to copy students.csv into a file named students_test.csv.

In the students_test.csv file, remove everything but the first five lines like you did for the other test file. Make sure there's an empty line at the bottom again.

You want to loop through all this info like you did for the other CSV file. The process is the same. Below your existing loop, use cat to print your new test file. Pipe the results into a while loop, setting the IFS to a comma again, and then use read to create FIRST, LAST, MAJOR and GPA variables from the data. In the loop, use echo to print the FIRST variable.

Run the script to see if it prints the FIRST (first_name) variable correctly. It will take a second since it has to go through the first loop.

It works 😅 It printed the first item in each row of the CSV file. It's printing the first line again, you will have to take care of that. First, delete the echo line.

Add an if condition to the loop that checks if the FIRST variable is not equal to first_name so it doesn't do anything for the first line of the file. Don't put anything in the statements area for now.

All the columns in the CSV file can be inserted directly into the database except for the major. You will need to get the major_id again for that. There's some null values in there as well, so you will need to use null if the major_id isn't found. Add four single line comments in your loop; get major_id, if not found, set to null, and insert student in that order.

Below the new get major_id comment, set the MAJOR_ID variable to a query that gets the major_id for the current students major.

Below that, use echo to print the variable so you can see if it's working.

Run the script to see what happens.

Looking at the test data, it found the ID for all of it except the null value. Below the newest if not found comment, add an if that checks if the variable is empty. Put the set to null comment in its statements area.

When you go to insert the student data, you want to use the MAJOR_ID if it's found, or null if not. Below the set to null comment, set the MAJOR_ID variable to null so you can use it to insert the data.

Move the echo $MAJOR_ID line to below the if statement so you can run the script and see the value of the variable if the major_id is or isn't found.

Run the script.

Okay, that should work for inserting the student. Delete the echo $MAJOR_ID line.

One last thing to add. In the psql prompt, view the details of the students table so you can see what columns to add.

You will need to set the four columns when adding the student info. All of them except student_id. Below the insert student comment, create an INSERT_STUDENT_RESULT variable that adds the student to the database. Add the columns in the order they appear in the data, and make sure to only put the two VARCHAR columns in single quotes.

Below the variable you just created, add an if statement that checks if it's equal to INSERT 0 1 like the others. If it is, use echo to print Inserted into students, <first_name> <last_name>.

Run the script to see if the students are getting added.

I think it's working. View all the data in the students table to make sure it matches the CSV file.

Excellent. It added all the students from the test data. Time to try it with the original files. Change the cat courses_test.csv line to use the original file again.

Next, change the cat students_test.csv line to use the original file as well.

Time for the moment of truth. Run the script and see if it works.

That was cool. View all the data in the students table to see what you ended up with.

31 rows. That's how many are in the CSV file. Perfect. Next, check the majors table.

7 rows. There must be 7 unique majors in the CSV file. View what's in the courses table.

Looks like there's 17 unique courses in the CSV file. Last, view the data in majors_courses. This should have the same number of rows at the CSV file.

28 rows, same as the CSV file. I think all the data got added correctly. You don't need your test files anymore. In the terminal, use the list command to check what files are in your project folder.

Use the remove command (rm) to delete the students_test.csv file.

Use the same command to delete the courses_test.csv file.

List the contents of the folder again to make sure they're gone.

The database is finished for now. The last thing you are going to do is make a "dump" of it. The pg_dump command can do that for you. Use the --help flag with the command to see what it can do.

This is the last step. There's quite a few options there. Enter pg_dump --clean --create --inserts --username=freecodecamp students > students.sql in the terminal to dump the database into a students.sql file. It will save all the commands needed to rebuild it. Take a quick look at the file when you are done.

