# Linux_csv_commands
csv handle by Linux


## Create csv
```sh
echo "column_a,column_b,column_c,column_d
1,foo,10,bar
2,baz,20,qux
3,quux,30,corge
4,grault,40,garply
" > output.csv
```

## Visualizing
```sh
column --separator "," --table output.csv
```

## Process
**Command Pipeline**
```sh
# sum the values in column_a and column_c, exclude column_b, and output the result.
# The pipeline reads the CSV file, processes it to sum `column_a` and `column_c`, excludes `column_b`, and adds a new header row to the output. This approach is efficient for processing large CSV files as it uses stream processing without creating intermediate files.
tail --lines=+2 output.csv | awk --field-separator ',' '{sum=$1+$3; print sum","$4}' OFS="," | sed '1i\sum_a_plus_c,column_d' 
```
**Pretty**
```sh
tail --lines=+2 output.csv | awk --field-separator ',' '{sum=$1+$3; print sum","$4}' OFS="," | sed '1i\sum_a_plus_c,column_d' | column --separator "," --table
```

   Here's a breakdown of what each command does:

   - **`tail -n +2 data.csv`**: This command skips the header row of the CSV file. `tail -n +2` starts reading from the second line, effectively removing the header.

   - **`awk -F, '{sum=$1+$3; print sum","$4}' OFS=,`**: This command uses `awk` to process each line of the CSV file.
     - `-F,` sets the field separator to a comma.
     - `{sum=$1+$3; print sum","$4}` calculates the sum of `column_a` (`$1`) and `column_c` (`$3`), and prints the result along with `column_d` (`$4`).
     - `OFS=,` sets the ***Output Field Separator*** to a comma, ensuring the output remains in CSV format.

   - **`sed '1i\sum_a_plus_c,column_d'`**: This command uses `sed` to insert a new header row. The `1i` indicates that the line should be inserted before the first line.
