# learn-colemak
My word lists for learning Colemak

Includes lists that are built for Unix commands and for PHP coding. Lots of WordPress/Elasticsearch terms so your mileage may vary here and you may want to build your own.

The directories contain the word lists while the files at the top level have terms duplicated and broken into sets to make them better for practicing.

## Building the lists

Repeating 3 lines 3 times:
```
cat file.txt | awk '{getline b; getline c;printf("%s %s %s %s %s %s %s %s %s\n",$0,b,c,$0,b,c,$0,b,c)}' | sed -n 's/  */ /gp' | awk ' {print;} NR % 2 == 0 { print ""; }' > file.learn
```

Add new lines to split lessons into 5 sequences:
```
awk ' {print;} NR % 5 == 0 { print ""; }' file.learn
```

Build top word list from arbitrary files (eg for the command line)
```
history > cmds.hist
cat cmds.hist | tr ' ' '\n' | grep -v '^[[:space:]]*$' | sed 's/^[[:space:]]*//' | sort | uniq -c | sort -rn | head -n 30 | awk '{print $2}'
```

Building 3/4/5 grams from arbitrary files (eg from code):
```
cat *.php | code
cat code | grep -o . > code.chars
tail -n+2 code.chars > code.chars2
tail -n+2 code.chars2 > code.chars3
tail -n+2 code.chars3 > code.chars4
tail -n+2 code.chars4 > code.chars5
paste -d '\0' code.chars code.chars2 code.chars3 | grep -v '^[[:space:]]*$' | sed 's/^[[:space:]]*//' | sed 's/[[:space:]]*$//' | awk 'length($0)==3' | sort | uniq -c | sort -rn | head -n 30 | awk '{$1="";print}' > top-30-3-graph.txt
paste -d '\0' code.chars code.chars2 code.chars3 code.chars4 | grep -v '^[[:space:]]*$' | sed 's/^[[:space:]]*//' | sed 's/[[:space:]]*$//' | awk 'length($0)==4' | sort | uniq -c | sort -rn | head -n 30 | awk '{$1="";print}' > top-30-4-graph.txt
paste -d '\0' code.chars code.chars2 code.chars3 code.chars4 code.chars5 | grep -v '^[[:space:]]*$' | sed 's/^[[:space:]]*//' | sed 's/[[:space:]]*$//' | awk 'length($0)==5' | sort | uniq -c | sort -rn | head -n 30 | awk '{$1="";print}' > top-30-5-graph.txt
```

Grab only lines with punctuation:
```
paste -d '\0' code.chars code.chars2 code.chars3 | grep -v '^[[:space:]]*$' | sed 's/^[[:space:]]*//' | sed 's/[[:space:]]*$//' | grep '[[:punct:]]' | awk 'length($0)==3' | sort | uniq -c | sort -rn | head -n 30 | awk '{$1="";print}' > top-30-3-graph.txt
paste -d '\0' code.chars code.chars2 code.chars3 code.chars4 | grep -v '^[[:space:]]*$' | sed 's/^[[:space:]]*//' | sed 's/[[:space:]]*$//' | grep '[[:punct:]]' | awk 'length($0)==4' | sort | uniq -c | sort -rn | head -n 30 | awk '{$1="";print}' > top-30-4-graph.txt
paste -d '\0' code.chars code.chars2 code.chars3 code.chars4 code.chars5 | grep -v '^[[:space:]]*$' | sed 's/^[[:space:]]*//' | sed 's/[[:space:]]*$//' | grep '[[:punct:]]' | awk 'length($0)==5' | sort | uniq -c | sort -rn | head -n 30 | awk '{$1="";print}' > top-30-5-graph.txt
```
