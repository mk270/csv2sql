csv2sql
=======

This tool is misnamed: it emits a MySQL CREATE TABLE statement
to fit a CSV file.

Usage: csv2sql -t name_of_table -s '\t' < filename.csv

The `-t' option specifies the name of the table to create.

The `-s' option specifies the character to use as a field
separator. This may be done in Python's string representation
format, which basically means that you can use \t to make
a tab character, but beware that backslashes must be quoted
to protect them from bash.

The '-i' option allows you to specify that a column should be
an integer not a string. You can use this option multiple
times, .e.g, -i Age_at_birth -i Age_at_death

csv2sql reads its input from stdin. This means you can do
things like:

 head -10000 example.csv | csv2sql -t example -s , -i f1 -i f2


Note that MySQL's table/column name quoting rules differ from
standard SQL; MySQL (and this tool) by default uses the
backtick character:  `   to quote table names, not the
double quote used in other SQL systems. Use the --quote option
to fix this.


Headerless CSV files
--------------------

If you have a long CSV file which does not have a header line, you
can prepend one in bash with (e.g.):

 $ (echo "firstname,surname,date_of_birth,image_id,etc" ; cat path_to_file.csv) | ./csv2sql --options

Remember to verify your MySQL LOAD statement when trying to be
clever with headers; check that "IGNORE 1 LINES" is present or
absent as it needs to be.
