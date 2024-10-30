# Date time format



{% code fullWidth="true" %}
```
// Some code
Year
	y - minimum digits. Example value: 20.
	yy - 2 digits + zero padded. Example value: 20.
	yyy - 3 digits + zero padded. Example value: 023.
	yyyy - 4 digits or more + zero padded. Example value: 2023.
	The upper case versions of these format strings are for formatting the week-numbering year.

Month
	M - 1 digit month. Example value: 9.
	MM - 2 digits + zero padded. Example value: 09.
	MMM - abbreviation. Example value: Sep.
	MMMM - full month name. Example value: September.
	MMMMM - narrow month name. Example value: S.

Weekday
	E, EE and EEE - abbreviation. Example value: Tue.
	EEEE - full name. Example value: Tuesday.
	EEEEE - narrow name. Example value: T.
	EEEEEE - short name. Example value: Tu.
	replace Es with cs to format standalone weekdays.


Week of Year
	w - minimum digits. Example values: 1… 53.
	ww- 2 digits + zero padded. Example values: 01… 53.
	Week of Month
	W - 1 digit. Example value: 1… 5.

Day of Month
	d - minimum digits. Example value: 1.
	dd - 2 digits + zero padded. Example value: 01.

Hour 1-12
	h - minimum digits. Example values: 1, 12.
	hh - 2 digits + zero padded. Example values: 01, 12.

Hour 0-23
	H - minimum digits. Example values: 1, 23.
	HH - 2 digits + zero padded. Example values: 01, 23.

Minute
	m - minimum digits. Example values: 1, 59.
	mm - 2 digits + zero padded. Example values: 01, 59.

Second
	s minimum digits. Example values: 0… 59.
	ss - 2 digits + zero padded. Example values: 00… 59.




{{ currentDate | date : "fullDate" }}
{{ currentDate | date : "EEEE YYYY-MM-dd hh:mm:ss.SSS OOOO" }}
```
{% endcode %}
