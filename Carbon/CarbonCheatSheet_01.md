

[Source](http://thisinterestsme.com/php-get-first-monday-of-month/ "Permalink to Get the first Monday of a month (and more)")

# Get the first Monday of a month (and more)

This is a small cheat sheet for PHP's strtotime function, which can be used to convert textual sentences such as "next Friday" and "last Monday" into UNIX timestamps and formatted dates.

In the examples below, I've used the format "j, d-M-Y", which will give you something like: Monday, 17-Aug-2015. You can change this to your format of choice

### Get next Monday.

Get next Monday (the result will be relative to today's date, of course):

```php
//Get next Monday 
echo date("j, d-M-Y", strtotime("next monday")); 
//Result is: Monday, 17-Aug-2015
```
| ----- |
|   | 
```php
//Get next Monday

echo date("j, d-M-Y", strtotime("next monday"));

//Result is: Monday, 17-Aug-2015
```
 | 

### Get the first day of a given month.

In this example, I am getting the first Wednesday in December, 2015:

//Get the first Wednesday of December, 2015 echo date("j, d-M-Y", strtotime("first wednesday 2015-12")); //Result is: Wednesday, 02-Dec-2015

| ----- |
|   | 
```php
//Get the first Wednesday of December, 2015

echo date("j, d-M-Y", strtotime("first wednesday 2015-12"));

//Result is: Wednesday, 02-Dec-2015
```
 | 

As you can see, the strtotime function is more than capable of converting English sentences into dates and times.

### Get next Friday.

If we want to get the date of next Friday:
```php
//Get next Friday echo date("j, d-M-Y", strtotime("last friday")); //Result is: Friday, 07-Aug-2015
```
| ----- |
|   | 
```php
//Get next Friday

echo date("j, d-M-Y", strtotime("last friday"));

//Result is: Friday, 07-Aug-2015
```
 | 

### Get the first day of next month.

Here, we get the first day of next month (relative to today's date):
```php
//Get the first day of next month echo date("j, d-M-Y", strtotime("first day of next month")); //Result is: Tuesday, 01-Sep-2015
```
| ----- |
|   | 
```php
//Get the first day of next month

echo date("j, d-M-Y", strtotime("first day of next month"));

//Result is: Tuesday, 01-Sep-2015
```
 | 

Get next Thursday.
```php
//Get next Thursday echo date("j, d-M-Y", strtotime("next thursday")); //Result is: Thursday, 13-Aug-2015
```
| ----- |
|   | 
```php
//Get next Thursday

echo date("j, d-M-Y", strtotime("next thursday"));

//Result is: Thursday, 13-Aug-2015
```
 | 

### Get the first Monday in January.

If we need to get the first Monday in January:

//Get the first Monday in January echo date("j, d-M-Y", strtotime("first monday 2015-01")); //Result is: Monday, 05-Jan-2015

| ----- |
|   | 
```php
//Get the first Monday in January

echo date("j, d-M-Y", strtotime("first monday 2015-01"));

//Result is: Monday, 05-Jan-2015
```
 | 

Obviously, you will need to change 2015 to match the year that is relevant to your requirements. For example, if we want the first Monday in January 2016:

//Get the first Monday in January, 2016 echo date("j, d-M-Y", strtotime("first monday 2016-01")); //Result is: Monday, 04-Jan-2016

| ----- |
|   | 
```php
//Get the first Monday in January, 2016

echo date("j, d-M-Y", strtotime("first monday 2016-01"));

//Result is: Monday, 04-Jan-2016
```
 | 

### First Monday of next month.

Obviously, this is relative to today's date:

//Get the first Monday of next month echo date("j, d-M-Y", strtotime("first monday of next month")); //Result is: Monday, 07-Sep-2015

| ----- |
|   | 
```php
//Get the first Monday of next month

echo date("j, d-M-Y", strtotime("first monday of next month"));

//Result is: Monday, 07-Sep-2015
```
 | 

### Get the last day of this month.

Need to get the last date of this month? You could even use this to figure out what day of the week it falls on, or how many days are in this month:

//Get the last day of this / current month echo date("j, d-M-Y", strtotime("last day of this month")); //Result is: Monday, 31-Aug-2015

| ----- |
|   | 
```php
//Get the last day of this / current month

echo date("j, d-M-Y", strtotime("last day of this month"));

//Result is: Monday, 31-Aug-2015
```
 | 

### Get the last date of last month.

Want to know when last month ended?

//Get the last day of last month echo date("j, d-M-Y", strtotime("last day of last month")); //Result is: Friday, 31-Jul-2015

| ----- |
|   | 
```php
//Get the last day of last month

echo date("j, d-M-Y", strtotime("last day of last month"));

//Result is: Friday, 31-Jul-2015
```
 | 

### Get the second Friday of last month.

Want to know when the second Friday of last month fell on?

//Get the second Friday of last month echo date("j, d-M-Y", strtotime("second friday of last month")); //Result is: Friday, 10-Jul-2015

| ----- |
|   | 
```php
//Get the second Friday of last month

echo date("j, d-M-Y", strtotime("second friday of last month"));

//Result is: Friday, 10-Jul-2015
```
 | 

As you can see, PHP's strtotime function is pretty flexible when it comes to taking in text.

### Get the last Friday of this month.

The last Friday of a given month can be pretty important to businesses:

//Get the last Friday of this month echo date("j, d-M-Y", strtotime("last friday of this month")); //Result is: Friday, 28-Aug-2015

| ----- |
|   | 
```php
//Get the last Friday of this month

echo date("j, d-M-Y", strtotime("last friday of this month"));

//Result is: Friday, 28-Aug-2015
```
 | 

Hopefully, you found this cheat sheet to be useful! Be sure to test strtotime out by supplying it with different sentences and whatnot! You can start off by changing words such as "last" to "first" and "second last", etc.

### Comments

comments

  
