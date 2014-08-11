# Notes for Awk

Awk is a tool for writing simple scripts to do text processing. The scope of it's
usage sits between _sed_/_grep_ and more general scripting language like _python_/_perl_, etc.

* For simple queries like find a word in a file, use _grep_.
* For single replacement, use _sed_.
* For deep analysing text file, (e.g., need map/set, etc.), use _python_.
* Awk is best suited for relatively complex text processing. E.g., if you want to reorder the 
columns in a row, using awk will be very easy, but complex in _sed_.

## Awk language spec:

```
BEGIN {ACTION}
pattern {ACTION}
END {ACTION}
```

Any part is optional, in the following section, I'll show examples of each.


## Awk examples:


- Print Banana's Flavoryness
```
awk '/Banana/ { print $3 }' file1
```
> `/Banana/` is a regular expression pattern. You can use normal expression syntax
> here.

- Print average flavoryness
```
awk 'BEGIN {sum = 0.0; count = 0}; {sum += $3; count++}; END { print sum / count}' file1
```
> Actually we can ommit `BEGIN {sum = 0.0; count = 0};`. The default value is 0 and awk
> is very dynamic to allow you to use a value without declare it.
> Also note that we don't provide _pattern_ in this example, which means it will process
> every line.

- Use your own separator, default is whitespace. Find Achira's scientific family.
Note that if there is no simple way to deal with quotes in csv files. Use a csv parser instead.
```
awk 'BEGIN {FS=","}; /Achira/ {print $4};' file2
```
> `FS` is the builtin separator variable. Set this variable to match the ones used in your file.

- Reorder file1 to have the format "Flavoryness,Name,Color"
```
awk '{printf "%s,%s,%s\n", $3, $1, $2}' file1
```
> `printf` is similar to that in other languages.

- Print every 3rd lines in file2.
```
awk 'NR % 3 == 0' file2
```
> `NR % 3` is _pattern_. If `{ACTION}` is ommited, the default action is to print the line.
> `NR` is the builtin varialbe line number.

- Calculate word frequency
```
BEGIN {
    FS="[^a-zA-Z]+"
}
{
     for (i=1; i<=NF; i++)
          words[tolower($i)]++
}
END {
    for (i in words)
         print i, words[i]
}
```
> `NF` is the builtin variable for number of fields in current line. For this example
> it's better to store this script in a file like word_frequency.awk. And execute the
> command: `awk -f word_frequency.awk file2`

- Criteria on certain field. Find the lines with nid.
```
awk -F, 'length($2) > 0' file2
```
> `-F,` is a succinct way to specify separator, which is equivalent to `BEGIN {FS=","}`.

- regex matching on certain field. Find fruit names that start with African
```
awk -F, '$1 ~ /^African/' file2
```
> `~` indicates a regular expression match on field _$1_.

- Multiple patterns. You can use logic operations on the pattern.
```
awk -F, '($1 !~ /^African/ && length($2) > 0) || NR == 1' file2
```
> This example find the lines that don't start with African and have an nid, as well
> as the first line.

- Use map to remove duplicate.
```
awk -F, '{family[$4]++;} END { for (f in family) print f ", " family[f];}' file2
```
> `for (f in family)` enumerate through the scientific families stored in the map.
> `family[$4]` increment for each family encountered in the line.


## Example file

File1:
"Name Color Flavoryness"
```
Banana Yellow 1.3
Apple Red 1.6
Rutabaga Brown 0.2
```

File2: "Common Name,nid,scientific-name,scientific-family"

```
Abaca,,Musa textilis,Musaceae
Abiu,9313,Pouteria caimito,Sapotaceae
Abyssinian Banana,,Ensete ventricosum,Musaceae
Abyssinian Gooseberry,,Dovyalis abyssinica,Flacourtiaceae
Acerola,9314,Malpighia punicifolia,Malpighiaceae
Achiote,9315,Bixa orellana,Bixaceae
Achira,3254,Canna edulis,Cannaceae
African Apricot,,Mamumea africana,Guttiterae
African Breadfruit,,Treculia africana,Moraceae
African Gooseberry,,Dovyalis abyssinica,Flacourtiaceae
African Honeysuckle,,Halleria lucida,Scrophulariaceae
African Horned Cucumber,,Cucumis metuliferus,Curcurbitaceae
```
