# Specification for NiceTime

An APL namespace with a function `offset` to express a timestamp as a text description of the offset from the present, such as:

```
just now
10 minutes ago
an hour ago
4pm
yesterday
3 days ago
last week
...
```

## Example

```
      2025 5 19 14 14 52 offset 2025 5 19 14 14 21
just now
```

Left argument is optional and defaults to the system clock `⎕TS`.

## Timestamps

Timestamps are 7-element integer vectors of the form `YYYY MM DD hh mm ss ms`. Only the first 6 elements are used. Both `now` (left argument) and `then` (right argument) are assumed to be in the same timezone.

## To do later

* The first version requires the right argument to be earlier than the left. A later version might remove this constraint and interpret timestamps in the future.
* A later version will use the `sjt-translate` package and supply it with an English - French - German dictionary.

## Debug mode

Flag `DEBUG` indicates whether Debug mode is on.

`DEBUG` is a public scalar boolean variable. When 1, `offset` performs argument validation and error signaling. Defaults to 0.

## Test suite

Niladic function `_testSuite` reports errors in the session log and returns a count of the number found, much as in the `sjt-translate` package.

## Localisation

NiceTime will use the `sjt-translate` package to localise output strings. For example, `{N} minutes ago` would be composed as:

```
(⍕N), ' ', ##.translate.text 'minutes ago'
```

## Thresholds

The string returned depends first on identifying the most recent of a series of intervals (today, current week, etc) and then the magnitude of the offset.

### Same day

If `then` is the previous day but `6 > 4⊃now` (i.e. before 6am), the same-day rules still apply.

| offset  | text            |
| ------- | --------------- |
| ≤1 min  | just now        |
| <1 hour | {X} minutes ago |
| else    | {X} hours ago   |

### Current week

The week starts on Monday by default. If variable `sundayStart` (default 0) is set to 1, the week starts on Sunday.

| offset | text      |
| ------ | --------- |
| -1 day | yesterday |
| else   | {weekday} |

### Previous week

| offset  | text                |
| ------- | ------------------- |
| -7 days | a week ago          |
| else    | {weekday} last week |

### Earlier

| offset        | text              |
| ------------- | ----------------- |
| current month | {X} weeks ago     |
| current year  | {month}           |
| previous year | {month} last year |
| else          | {month} {year}    |

## Exports

The namespace will publicly expose:

* `offset`
* `DEBUG`
* `sundayStart`

## Notes

Above, `string` refers to a character vector.

`{weekday}` is a day name such as "Monday".
`{month}` is a month name such as "March".

[Revised with ChatGPT](https://chatgpt.com/canvas/shared/682c47bea790819180c5aa8edb50a04f)
