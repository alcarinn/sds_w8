## Solutions

### Question 1 

```
tail -n +2 clients.txt | awk -F ',' '{print $2 }' > names.txt
```

### Question 2

```
tail -n +2 clients.txt | awk -F',' '{print $3}' | grep -v nil > emails.txt
``` 

### Question 3

```
# Simply count the number of uniq clientID in orders, and compare to clients.txt size.
> tail -n +2 orders.txt | awk -F ',' '{print $3}' | sort | uniq | grep -c .  

# Client size.
> tail -n +2 clients.txt | grep -c . 
```

### Question 4


```
SELECT  
  b.TransactionID,
  b.ProdID,
  a.Name, 
  b.DatePurchased
FROM clients a 
INNER JOIN orders b ON a.ID = b.ClientID;

```

### Question 5

Create a python file that reads our dates from stdin:

```
#!/usr/bin/python3

# myPlot.py
import fileinput
import matplotlib.pyplot as plt

vals = []

for line in fileinput.input():
    vals.append(int(line[4:6]))

plt.hist(vals, bins = 12)
plt.show()
```

Then parse the dates using unix commands and feed it to python:

```
tail -n +2 orders.txt | awk -F ',' '{print $4}' | ./myPlot.py 
```

### Question 6

Create a python script `replace.py`:

```
#!/usr/bin/python3

import fileinput

def convert(s):
    return s[4:6] + "/" + s[6:] + "/" +s[0:4]
i = 0
for line in fileinput.input():
    if i == 0:
        i+= 1
        print(line.rstrip("\n\r"))
        continue
    s = line.split(",")
    s[3] = convert(s[3].rstrip("\n\r"))
    print(",".join(s))
```
And run it like this:

```
cat orders.txt | ./replace.py > orders2.txt

```

### Question 7

SQLite: 

```
SELECT ProdID, MAX(mcount)
FROM (SELECT ProdID,COUNT(ProdID) mcount FROM orders GROUP BY ProdID);
```

Python using a dict for each client ID and finding the max.

```
#!/usr/bin/python3

import fileinput

x = {}

for l in fileinput.input():
    if l.strip() in x:
        x[l.strip()]=x[l.strip()]+1
    else:
        x[l.strip()] = 1

mkey = ""
mmax = -1
for k, v in x.items():
    if v > mmax:
        mkey = k
        mmax = int(v)


print(mkey+"->"+str(mmax))
```

```

tail -n +2 orders.txt | awk -F ',' '{print $2}' | ./mostPopular.py 

```

### Question 8

```
CREATE VIEW manager
AS
SELECT  
  b.TransactionID,
  b.ProdID,
  a.Name as Client, 
  b.DatePurchased
FROM clients a 
INNER JOIN orders b ON a.ID = b.ClientID;

```

### Question 9

The unix solution:

```
# That shows the highest id
> tail -n +2 orders.txt | awk -F ',' '{print $1}' | sort |uniq | tail -n -1

# That counts the numbers of transactions
> tail -n +2 orders.txt | awk -F ',' '{print $1}' | sort| uniq | grep -c .
```

The python solution: 

```
#!/usr/bin/python3

import fileinput

vals = {}

for line in fileinput.input():
    id = line.split(",")[0]
    vals[int(id)] = 1

if len(vals) == 1+max(vals.keys()):
    print("No missing")
else:
    print("Missing")
```

That you run with 

```
tail -n +2 orders.txt| ./q9.py
```

### Question 10

Spark, lots of Spark
