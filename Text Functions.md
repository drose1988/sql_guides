

#### Text Functions



LENGTH() :: useful to find the distribution of text lengths for all values in a column

```sql
select
    length(sighting_report),
    count(*) as records
from ufo
group by 1
order by 1;
```



LEFT() and RIGHT () :: returns the characters on the left of right of a string, in the argument, specify the point in the string you are starting from

- use the two functions together to return middle pieces of a text you want



```sql
select
    right(left(sighting_report, 25), 14) as occured
from ufo;
```



SPLIT_PART(string or field name, delimiter, index)

- so the function looks for a delimiter you specify (can be anything)

- if your index is 1, the function looks for everything before the first delimiter found

- if you index is 2, the function looks for the second time the delimiter is found and returns everything between the 1st and 2nd delimiter

- this chunk text between the delimiter is what gets returned as you change the index

- nesting multiple split_parts together is useful for extracting a particular middle part of the string you want

- in the case there are particular words between the text you want, those those are used as delimiters 



```sql
select 
    split_part(
        split_part(
            split_part(sighting_report, ' (Entered', 1),
         'Occurred : ', 2), 
    'Reported', 1) as occurred
from ufo
```

```sql
select
  split_part(split_part(split_part(sighting_report,' (Entered',1),'Occurred : ',2),'Reported',1) as occurred,  
  split_part(split_part(sighting_report,')Reported:',1),'(Entered as :',2) as entered,    
  split_part(split_part(sighting_report,'Posted:',1),'Reported: ',2) as reported,  
  split_part(split_part(sighting_report,'Location: ',1),'Posted: ',2) as posted,  
  split_part(split_part(sighting_report,'Shape: ',1),'Location: ',2) as location,  
  split_part(split_part(sighting_report,'Duration:',1),'Shape: ',2) as shape,  
  split_part(sighting_report,'Duration:',2) as duration 
from ufo
```


