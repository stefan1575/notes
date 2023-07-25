# **Date and Time**

The built-in object `Date` stores and provides methods for the date/time management. The time is relative to (UTC+0) and the result is adjusted according to the timezone is run.

## **I. Creation**

### **A. `new Date()`**

It shows the current date/time in local time, if called without arguments:

```js
let currentTime = new Date();
alert(currentTime);
```

### **B. `new Date(milliseconds)`**

Creates a `Date` object equal to the `milliseconds + 1/01/1970`. A negative value subtracts the `milliseconds` from `01/01/1970`:

```js
let Jan_02_1970 = new Date(24 * 3600 * 1000);
alert(Jan_02_1970); // adds one day

let December_31_1969 = new Date(-24 * 3600 * 1000);
alert(December_31_1969); // subtracts one day
```

### **C. `new Date(datestring)`**

If the argument is a string, it is parsed automatically by the `Date.parse` algorithm which uses:

```js
let date = new Date("2017-01-26");
alert(date);
```

### **D. `new Date(year, month, day, hour, minutes, seconds, ms)`**

The first two arguments `(year, month)` are mandatory, the rest are optional. The result is shown in local time.

- `year` - Using four digits is recommended since the result of two digits return `19xx`. Two digits are accepted for compatibility purposes.
- `month` - The count starts as "`0`- (January)" up to "`11` - (December)".
- `date` - If absent, `1` is assumed.
- `hours/minutes/seconds/ms` - If absent, `0` is assumed.

```js
new Date(2000, 0); // January 1, 2000 - 0:00:00
```

- The maximum precision allowed in `ms` is `999` or (1/1000 sec):

```js
let date = new Date(2000, 0, 1, 0, 0, 999);
alert(date); // January 01, 2000 00:00:00.999
```

## **II. Access date components**

These methods return the components relative to the local timezone:

- `getFullYear()` - get the year, **uses 4 digits**.
- `getMonth()` - get month **from `0` to `11`**.
- `getDate()` - get day of the month **from `1` to `31`**.
- `getHours()`, `getMinutes()`, `getSeconds()`, `getMilliseconds()` - get the corresponding time components.
- `getDay()` - get day of the week **from `0` (Sunday) to `6` (Monday)**.

If we want to return the components relative to UTC+0, append `"UTC"` right after `"get"`:

```js
let date = new Date();

alert(getHours(date)); // gets the current date
alert(getUTCHours(date)); // converts the current date in UTC form
```

There are two special methods that don't have a UTC-variant:

- `getTime()` - returns the number of milliseconds past after 01/01/1970 UTC+0.
- `getTimezoneOffset()` - returns the number of minutes between UTC to local time.

## **III. Set date components**

The following methods modifies the date/time relative to the local timezone. Every one of them has a UTC variant except `setTime()`:

- `setFullYear(year, [month, date])`
- `setMonth(month, [date])`
- `setDate(date)`
- `setHours(hour, [min, sec, ms])`
- `setMinutes(min, [sec, ms])`
- `setSeconds(sec, [ms])`
- `setMilliseconds(ms)`
- `setTime(milliseconds)` - sets the time to the number of `milliseconds` since 01/01/1970.

```js
let currentDate = new Date();
// only hour is changed
currentDate.setHours(0);
alert(currentDate); // 00:xx:xx

// hour/min/sec/ms is changed
currentDate.setHours(0, 0, 0, 0);
alert(currentDate); // 00:00:00
```

## **IV. Autocorrection**

### **A. Add date - addition or out of range values**

- When setting out of range values, the date will auto adjust itself:

```js
let date = new Date(2000, 0, 32); // January 31, 2000 + 1
alert(date); // February 1, 2000
```

- When trying to add values to a `Date` object, where a value may be `March 1` or `March 2` in case of a leap-year. The date object will auto adjust itself:

```js
let date = new Date(2000, 2, 28); // leap year
date.setDate(date.getDate() + 2);
alert(date); // March 1, 2000
```

### **B. Subtract date - `0` or negative values**

- We can also set zero or negative values to subtract the date.

For instance, the minimum `setDate()` value is 1, it will start counting in reverse and go back to the last day of the month:

```js
let date = new Date(2000, 0, 30); // January 30, 2000
date.setDate(1); // set day of the month to 1
alert(date); // January 1, 2000

date.setDate(0); // subtracts the current date to -1
alert(date); // December 31, 1999
```

## **V. Time measurement**

### **A. `Date` to `Number`**

When a `Date` object is converted to a `Number`, it becomes _timestamp_ same as `date.getTime()`:

```js
let date = new Date();
alert(+date); // converted to the number of milliseconds
```

As a side effect, two dates can be subtracted where the difference can be used for time measurement:

```js
let startCount = new Date(); // time before loop
// loop until i reaches 100k
for (let i = 0; i < 100000; i++) {
	let doSomething = i * i * i;
}
let endCount = new Date(); // time elapsed after loop

// amount of ms the loop took
alert(`Loop time: ${endCount - startCount} ms`);
```

### **B. `Date.now()`**

`Date.now()` is a faster way to to measure time. It returns the millisecond count after `01/01/1970`. It's mostly used when performance matters or convenience:

```js
let startCount = Date.now();
for (let i = 0; i < 100000; i++) {
	let doSomething = i * i * i;
}
let endCount = Date.now();

// It subtracts numbers instead of dates
alert(`Loop time: ${endCount - startCount} ms`);
```

## **VI. `Date.parse(str)`**

`Date.parse()` parses a string representation of a date.

- It returns the a _timestamp_ or number of milliseconds since `01/01/1970`.
- If the string contains illegal date values or an invalid format, it returns `NaN`.

The string format is Date.parse(YYYY-MM-DDTHH:mm:ss:sssZ):

- `YYYY-MM-DD` - year, month, day.
- `T` - is used as a delimiter.
- `HH:mm:ss.sss` - hours, mins, seconds, milliseconds.
- `Z(Optional)` - the format that denotes the timezone `"+-hh:mm"`. Just appending `Z` at the end means UTC+0.

Month and day `"MM-DD"` are optional. If seconds and milliseconds `"ss.sss"` aren't provided, they are assumed to become `0`.

For instance, we can parse a string with it's timezone into a timestamp:

```js
let timestamp = Date.parse("2000-01-01T00:00-08:00");
alert(timestamp); // 946713600000
```

It can be also be used to create a `new Date` object as an argument:

```js
let date = new Date(Date.parse("2000-01-01T00:00-08:00"));
alert(date); // Adjusts date to the specified timezone
```

## **Summary**

- Date and time is represented by the `Date` object, we can't create "only date" or "only time".
- Months are counted from **`0` - (January) up to `11` - (December)** using `get/setMonth()`.
- Days of the week are counted from **`0` - (Sunday) up to `6` (Monday)** using `getDay()`.
- We can add `Date` objects using addition and out of range objects.
- We can subtract `Date` objects using zero or negative values.
- Date becomes a timestamp when converted into a number, where we can use it to measure time.
  - If we need performance or convenience, use `Date.now()`.
  - If we need more precision, use `performance.now()`.
- `Date` objects get converted into _timestamps_ when used expressions. This means that we need to convert them back to a date object.
