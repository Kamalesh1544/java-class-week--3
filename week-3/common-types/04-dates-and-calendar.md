# Dates and Calendar in Java

---

## Java Date/Time API

Java provides two ways to work with dates:

1. **Old API** (`Date`, `Calendar`, `SimpleDateFormat`) — legacy, not recommended
2. **New API** (`java.time` package, Java 8+) — modern, thread-safe, recommended

---

## 1. Old API (Legacy)

### Date Class

```java
import java.util.Date;

public class DateDemo {
    public static void main(String[] args) {
        Date now = new Date();
        System.out.println("Current: " + now);
        System.out.println("Time: " + now.getTime());   // milliseconds since 1970
    }
}
```

### Output:
```
Current: Sat Jun 13 11:45:00 IST 2026
Time: 1781331900000
```

### SimpleDateFormat

```java
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateFormatDemo {
    public static void main(String[] args) {
        Date now = new Date();

        SimpleDateFormat sdf1 = new SimpleDateFormat("dd/MM/yyyy");
        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd");
        SimpleDateFormat sdf3 = new SimpleDateFormat("EEE, dd MMM yyyy");

        System.out.println(sdf1.format(now));   // 13/06/2026
        System.out.println(sdf2.format(now));   // 2026-06-13
        System.out.println(sdf3.format(now));   // Sat, 13 Jun 2026
    }
}
```

### Calendar Class

```java
import java.util.Calendar;

public class CalendarDemo {
    public static void main(String[] args) {
        Calendar cal = Calendar.getInstance();

        int day = cal.get(Calendar.DAY_OF_MONTH);
        int month = cal.get(Calendar.MONTH) + 1;   // months start from 0
        int year = cal.get(Calendar.YEAR);

        System.out.println("Date: " + day + "/" + month + "/" + year);
    }
}
```

### Output:
```
Date: 13/6/2026
```

---

## 2. New API (java.time — Java 8+)

### LocalDate — Date without time

```java
import java.time.LocalDate;

public class LocalDateDemo {
    public static void main(String[] args) {
        LocalDate today = LocalDate.now();
        System.out.println("Today: " + today);

        LocalDate birthday = LocalDate.of(2000, 7, 15);
        System.out.println("Birthday: " + birthday);

        System.out.println("Day: " + today.getDayOfMonth());
        System.out.println("Month: " + today.getMonth());
        System.out.println("Year: " + today.getYear());
    }
}
```

### Output:
```
Today: 2026-06-13
Birthday: 2000-07-15
Day: 13
Month: JUNE
Year: 2026
```

### LocalTime — Time without date

```java
import java.time.LocalTime;

public class LocalTimeDemo {
    public static void main(String[] args) {
        LocalTime now = LocalTime.now();
        System.out.println("Now: " + now);

        LocalTime meeting = LocalTime.of(14, 30);   // 2:30 PM
        System.out.println("Meeting: " + meeting);

        System.out.println("Hour: " + now.getHour());
        System.out.println("Minute: " + now.getMinute());
    }
}
```

### Output:
```
Now: 11:47:30.123456
Meeting: 14:30
Hour: 11
Minute: 47
```

### LocalDateTime — Date + Time

```java
import java.time.LocalDateTime;

public class LocalDateTimeDemo {
    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();
        System.out.println("Now: " + now);

        LocalDateTime custom = LocalDateTime.of(2026, 12, 25, 10, 30);
        System.out.println("Christmas: " + custom);
    }
}
```

### Output:
```
Now: 2026-06-13T11:48:00.123
Christmas: 2026-12-25T10:30
```

---

## DateTimeFormatter

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class FormatterDemo {
    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();

        DateTimeFormatter f1 = DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm");
        DateTimeFormatter f2 = DateTimeFormatter.ofPattern("EEE, dd MMM yyyy");
        DateTimeFormatter f3 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

        System.out.println(now.format(f1));   // 13/06/2026 11:49
        System.out.println(now.format(f2));   // Sat, 13 Jun 2026
        System.out.println(now.format(f3));   // 2026-06-13 11:49:00
    }
}
```

---

## Period — Difference Between Dates

```java
import java.time.LocalDate;
import java.time.Period;

public class PeriodDemo {
    public static void main(String[] args) {
        LocalDate start = LocalDate.of(2000, 7, 15);
        LocalDate end = LocalDate.now();

        Period period = Period.between(start, end);
        System.out.println("Age: " + period.getYears() + " years, " +
                           period.getMonths() + " months, " +
                           period.getDays() + " days");
    }
}
```

### Output:
```
Age: 25 years, 10 months, 29 days
```

---

## Duration — Difference Between Times

```java
import java.time.Duration;
import java.time.LocalTime;

public class DurationDemo {
    public static void main(String[] args) {
        LocalTime start = LocalTime.of(9, 0);
        LocalTime end = LocalTime.now();

        Duration duration = Duration.between(start, end);
        System.out.println("Hours since 9 AM: " + duration.toHours());
        System.out.println("Minutes since 9 AM: " + duration.toMinutes());
    }
}
```

---

## ZonedDateTime — Time Zone

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;

public class ZoneDemo {
    public static void main(String[] args) {
        ZonedDateTime now = ZonedDateTime.now();
        System.out.println("Current: " + now);

        ZonedDateTime tokyo = now.withZoneSameInstant(ZoneId.of("Asia/Tokyo"));
        System.out.println("Tokyo: " + tokyo);

        ZonedDateTime london = now.withZoneSameInstant(ZoneId.of("Europe/London"));
        System.out.println("London: " + london);
    }
}
```

---

## Instant — Timestamp

```java
import java.time.Instant;

public class InstantDemo {
    public static void main(String[] args) {
        Instant now = Instant.now();
        System.out.println("Instant: " + now);
        System.out.println("Epoch seconds: " + now.getEpochSecond());
    }
}
```

---

## Quick Comparison

| Class | Use Case |
|---|---|
| `LocalDate` | Date only (birthday, holiday) |
| `LocalTime` | Time only (meeting time) |
| `LocalDateTime` | Date + Time |
| `ZonedDateTime` | Date + Time + Time Zone |
| `Instant` | Timestamp (epoch seconds) |
| `Period` | Difference between dates |
| `Duration` | Difference between times |
| `DateTimeFormatter` | Formatting dates |

---

## Key Points to Remember

- Use `java.time` package (Java 8+) instead of legacy `Date` and `Calendar`.
- `LocalDate`, `LocalTime`, `LocalDateTime` are **immutable** and **thread-safe**.
- Use `DateTimeFormatter` for formatting (not `SimpleDateFormat`).
- Use `Period` for date differences, `Duration` for time differences.
- Always store dates in `LocalDate` or `Instant`, not strings.
