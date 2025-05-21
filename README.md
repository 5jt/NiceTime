NiceTime
========

![have a nice time](banner.png)

An APL namespace to express a timestamp as a text description of the offset from now, such as

	just now
	10 minutes ago
	an hour ago
	4pm
	yesterday
	3 days ago
	last week
	...

Example
-------
	      2025 5 19 14 14 52 nicetime.was 2025 5 19 14 14 21
	just now

Left argument is optional and defaults to `⎕TS`.

First version requires the right argument to be earlier than the left.


Tests
-----
Niladic function `tests` reports errors in the session log and  returns a count of the number found.

Dependencies
------------
Testing requires package `sjt-tinytest`.

Thresholds
--------

### Same day

offset   | text
---------|-----
≤1 min   | just now
<1 hour  | X minutes ago
<6 hours | X hours ago
else     | X[a|p]m

### Current week

offset | text
-------|-------
-1 day | yesterday
else   | {day}

### Previous week

offset  | text
--------|-------
-7 days | a week ago
else    | \[Mon|Tues|Wednes|Thurs|Fri|Satur|Sun\]day last week

### Earlier

offset | text
-------|-------
current month | X weeks ago
current year  | {month}
previous year | {month} last year
else          | {month} {year}

To do
-----

-   support multiple languages with packages `sjt-text` and `sjt-translate`
-   flag `sundayStart` to shift week boundary