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
```sh
# sum the values in column_a and column_c, exclude column_b, and output the result.
tail -n +2 data.csv | awk -F, '{sum=$1+$3; print sum","$4}' OFS=, | sed '1icolumn_a_plus_column_c,column_d'
```
