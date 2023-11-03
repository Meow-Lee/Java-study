---
description: About Calendar, Date
---

# Date, Time and Formatting

### 1. 날짜와 시간

```java
// Calendar -> Date
Calendar cal = Calendar.getInstance();
...
Date d = new Date(cal.getTimeInMillis());

// Date -> Calendar
Date d = new Date();
...
Calendar cal = Calendar.getInstance();
cal.setTime(d);

```

### 2. LocalDateTime

* LocalTime -> 시간을 표현
* LocalDate -> 날짜를 표현
