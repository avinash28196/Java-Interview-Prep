# Java 8 Date-Time API

With Java 8, a new Date-Time API is introduced to cover the following drawbacks of old date-time API.

## Issues with the existing Date/Time API's

**Thread Safety** – The Date and Calendar classes are not thread safe, leaving developers to deal with the headache of hard to debug concurrency issues and to write additional code to handle thread safety. On the contrary the new Date and Time APIs introduced in Java 8 are immutable and thread safe, thus taking that concurrency headache away from developers.

**APIs Design and Ease of Understanding** – The Date and Calendar APIs are poorly designed with inadequate methods to perform day-to-day operations. The new Date/Time APIs is ISO centric and follows consistent domain models for date, time, duration and periods. There are a wide variety of utility methods that support the commonest operations.

**ZonedDate and Time**– Developers had to write additional logic to handle timezone logic with the old APIs, whereas with the new APIs, handling of timezone can be done with Local and ZonedDate/Time APIs.

## Dates

Date class has even become obsolete. The new classes intended to replace Date class are LocalDate, LocalTime and LocalDateTime.

1. The LocalDate class represents a date. There is no representation of a time or time-zone.
2. The LocalTime class represents a time. There is no representation of a date or time-zone.
3. The LocalDateTime class represents a date-time. There is no representation of a time-zone.

# Java 8 Date and time API

## LocalDate
The LocalDateTime class represents a date-time. There is no representation of a time-zone.

```
LocalDate localDate = LocalDate.now();
System.out.println(localDate.toString());                //2013-05-15
System.out.println(localDate.getDayOfWeek().toString()); //WEDNESDAY
System.out.println(localDate.getDayOfMonth());           //15
System.out.println(localDate.getDayOfYear());            //135
System.out.println(localDate.isLeapYear());              //false
System.out.println(localDate.plusDays(12).toString());   //2013-05-27
OffsetDateTime offsetDateTime = OffsetDateTime.now();
ZonedDateTime zonedDateTime = ZonedDateTime.now(ZoneId.of("Europe/Paris"));
```

## LocalTime

The LocalTime class represents a time. There is no representation of a date or time-zone.

```
//LocalTime localTime = LocalTime.now();     //toString() in format 09:57:59.744
LocalTime localTime = LocalTime.of(12, 20);
System.out.println(localTime.toString());    //12:20
System.out.println(localTime.getHour());     //12
System.out.println(localTime.getMinute());   //20
System.out.println(localTime.getSecond());   //0
System.out.println(localTime.MIDNIGHT);      //00:00
System.out.println(localTime.NOON);          //12:00
```

## LocalDateTime
The LocalDateTime class represents a date-time. There is no representation of a time-zone.

```
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println(localDateTime.toString());      //2013-05-15T10:01:14.911
System.out.println(localDateTime.getDayOfMonth()); //15
System.out.println(localDateTime.getHour());       //10
System.out.println(localDateTime.getNano());       //911000000
```

## Time Zone

If you want to use the date functionality with zone information, then Lambda provide you extra 3 classes similar to above one i.e. OffsetDate, OffsetTime and OffsetDateTime. Timezone offset can be represented in “+05:30” or “Europe/Paris” formats. This is done via using another class i.e. ZoneId.

```
OffsetDateTime offsetDateTime = OffsetDateTime.now();
System.out.println(offsetDateTime.toString());              //2013-05-15T10:10:37.257+05:30
 
offsetDateTime = OffsetDateTime.now(ZoneId.of(&quot;+05:30&quot;));
System.out.println(offsetDateTime.toString());              //2013-05-15T10:10:37.258+05:30
 
offsetDateTime = OffsetDateTime.now(ZoneId.of(&quot;-06:30&quot;));
System.out.println(offsetDateTime.toString());              //2013-05-14T22:10:37.258-06:30
 
ZonedDateTime zonedDateTime = ZonedDateTime.now(ZoneId.of(&quot;Europe/Paris&quot;));
System.out.println(zonedDateTime.toString());               //2013-05-15T06:45:45.290+02:00[Europe/Paris]

```


## Timestamp and Duration

For representing the specific timestamp ant any moment, the class needs to be used is Instant. The Instant class represents an instant in time to an accuracy of nanoseconds. Operations on an Instant include comparison to another Instant and adding or subtracting a duration.

```
Instant instant = Instant.now();
Instant instant1 = instant.plus(Duration.ofMillis(5000));
Instant instant2 = instant.minus(Duration.ofMillis(5000));
Instant instant3 = instant.minusSeconds(10);
```

## Duration 

Duration class is a whole new concept brought first time in java language. It represents the time difference between two time stamps.

```
Duration duration = Duration.ofMillis(5000);
System.out.println(duration.toString());     //PT5S
 
duration = Duration.ofSeconds(60);
System.out.println(duration.toString());     //PT1M
 
duration = Duration.ofMinutes(10);
System.out.println(duration.toString());     //PT10M
 
duration = Duration.ofHours(2);
System.out.println(duration.toString());     //PT2H
 
duration = Duration.between(Instant.now(), 
Instant.now().plus(Duration.ofMinutes(10)));
System.out.println(duration.toString());  //PT10M
```

Duration deals with small unit of time such as milliseconds, seconds, minutes and hour. They are more suitable for interacting with application code.

## Period 

To interact with human, you need to get bigger durations which are presented with Period class.


```
Period period = Period.ofDays(6);
System.out.println(period.toString());    //P6D
 
period = Period.ofMonths(6);
System.out.println(period.toString());    //P6M
 
period = Period.between(LocalDate.now().LocalDate.now().plusDays(60));
System.out.println(period.toString());   //P1M29D
```


## Timezone Changes

Timezone related handling is done by 3 major classes. These are ZoneOffset, TimeZone, ZoneRules.

1. The ZoneOffset class represents a fixed offset from UTC in seconds. This is normally represented as a string of the format “±hh:mm”.
2. The TimeZone class represents the identifier for a region where specified time zone rules are defined.
3. The ZoneRules are the actual set of rules that define when the zone-offset changes.


```
//Zone rules
System.out.println(ZoneRules.of(ZoneOffset.of(&quot;+02:00&quot;)).isDaylightSavings(Instant.now()));
System.out.println(ZoneRules.of(ZoneOffset.of(&quot;+02:00&quot;)).isFixedOffset());

```

## Date formatting 

Date formatting is supported via two classes mainly i.e. DateTimeFormatterBuilder and DateTimeFormatter. DateTimeFormatterBuilder works on builder pattern to build custom patterns where as DateTimeFormatter provides necessary input in doing so.


```

DateTimeFormatterBuilder formatterBuilder = new DateTimeFormatterBuilder();
formatterBuilder.append(DateTimeFormatter.ISO_LOCAL_DATE_TIME)
                .appendLiteral(&quot;-&quot;)
                .appendZoneOrOffsetId();
DateTimeFormatter formatter = formatterBuilder.toFormatter();
System.out.println(formatter.format(ZonedDateTime.now()));
```










