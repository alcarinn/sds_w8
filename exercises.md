Data wrangling : putting it all together
    - when to use Unix command-line tools, pipes, scripts.
    - when to use Python and siblings
    - when to do it in a Spark job
    - when to import into an SQL database and do it there.
    - how to be productive; case studies


# Data Wrangling

As you have seen throughout your studies, a programmer has at his disposal various tools: command-line tools, shell-scripts, programming languages (e.g., python), database programs, big data frameworks such as Spark, and many others.

While all of you agree on the usefulness of Spark, some might have doubts when it comes to learning more "system"-oriented tools such as command-line tools or SQL databases.
This exercise session's goal is to clear these doubts.
More specifically, we do not say that it is impossible to solve any of these problems in python, we are just saying that it is less efficient and requires more work.

In the words of Frank Bunker Gilbreth: 

```
“I will always choose a lazy person to do a difficult job because a lazy person will find an easy way to do it.”
```

In this exercise session, if you pick the correct tool to do the job, you should be done in no time.
Let's dive into it!

## Setup

### Files

There are two files from `The Fakie Fake Company` on your system: 1) `clients.txt`, and 2) `orders.txt`.
Both files are stored on your local disk and respect the [CSV format](https://en.wikipedia.org/wiki/Comma-separated_values).

The `clients.txt` file has the following format: 

```
ID, Name, Email
```

And `orders.txt` the following one:

```
TransactionID, ProdID, ClientID, DatePurchased
```

With `DatePurchased` in the format `YYYYMMDD`.


### Tools 

For the remainder of this exercise, we consider the following available tools: 

1. Unix command-line tools (grep, sed, awk, uniq, head, tail etc.), including shell-scripts
2. Python
3. SQLite 
4. Spark

We start by creating a SQLite database with the two files, this way we can leave SQLite without losing the tables.

```
> sqlite3 mdb.sl3

$>.mode csv
$>.import clients.txt clients
$>.import orders.txt orders
```

A useful trick with UNIX tools relies on `tail` to remove the header from a file:

```
# removes the first line from clients.txt

> tail -n +2 clients.txt

```

And counting lines can be achieved with:

```
> grep -c . clients.txt
```

Note that people would recommend `wc -l` to do that instead, but this command only counts `\n`'s.

Finally, a neat trick that is used in coding competitions is to redirect stdin to a program (python, java, scala etc.) to avoid having to deal with/create files:

```
#!/usr/bin/python3

import fileinput

for line in fileinput.input():
  print(line)

```

## Questions

### Question 1

We want to extract all the `Name`s from the `clients.txt` file and write them inside a `names.txt` file.
Which tool would you use? Is that the most efficient one you could think of?

Perform that operation on the provides text files.

### Question 2

Now, one of the young managers wants to spam clients with a newsletter e-mail and asks you to provide a file with all the clients' e-mail addresses.
The problem is that some of the entries in the `clients.txt` file have "nil" e-mail addresses.

Which tool would you use to extract all the e-mails, filter the nil entries, and write the result to a file?
Implement your solution.

### Question 3

The same manager now sends you the following slack message: 

```
Do all clients placed at least one order? 
Answer with Yes or No FAST, I don't have time for useless details!
```

Which tool would you use to provide a quick answer to your (very rude) manager?
Once again implement your solution, but know that if it took you more than two minutes, you will probably be fired...

### Question 4

The same manager apologizes, and explains that he is cranky because his neck hurts from having to look back and forth between `clients.txt` and `orders.txt` to find, for each order, the name of the corresponding client.
You are very busy but you want to help him.
Implement your solution.

### Question 5

The manager thanks you very much for all your efforts, but realizes that he is not a "text" person and better understands graphs and pictures.
He is currently working on increasing advertising investments to boost sales during the "slow" months.
He wants to visualize the amount of sales, for each month of the year, as a histogram.  

### Question 6

The manager says that we need to be compatible with American date format.
He wants you to correct all the dates in the `orders.txt` file to be `MM-DD-YYYY`.
How would you proceed?

### Question 7 

The manager says he took a bet with his secretary about which product was the most popular.
Find that result using  using SQLite and then python.
Which one was the easiest?

### Question 8

The manager comes back to you and says that he really likes your solution to question 4, but would like to be able to have an updated version everytime someone enters a new order. 
How would you proceed? 

Note: The manager knows how to 1) run a script (bash or python), 2) read an SQL table.

### Question 9

The manager thinks that someone has been stealing money from the company by not committing transactions to the log.
The `orders.txt` is such that TransactionIDs might not be ordered, but all numbers from 0 to max transactionID should appear somewhere.

How would you check that in python? Do you think you can do it with unix commands?
(Do it in both if possible).

### Question 10

Your manager is very proud of you.
During a business meeting, he talked about how good and fast you were.
He sends you the following slack message: 

```
Hi!
I was talking with the CEO of another company, they actually have the same tables as we do!
Anyway, he wants the same kind of answers that you provided me with, could you do something for him too?
His name is Jeff Bezos.
```

You now have HUGE amounts of data to process.
How do you do it?
You DO NOT have to implement this one.

