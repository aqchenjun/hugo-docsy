---
categories: Pandas
title: Pandas 日期与时间
tags: [ 教程, python]
date: 2023-03-17
lastmod: 2023-03-22 
thumbnail: https://s1.vika.cn/space/2023/03/25/084259a349bd4d7a9a92f570dc1fdf95?attname=soviet-1354211_960_720.jpg
published: "true"
slug: 5zzq2wj
---


## 1 构造与转换  

### 1.1 单个日期的构造 

#### 1.1.1 Timestamp 类  

```python
pd.Timestamp(ts_input=<object object>,  #datetime-like, str, int, float
                freq=None,
                tz=None,
                unit=None,        #当 ts_input 为 float 时，有效值：‘D’, ‘h’, ‘m’, ‘s’, ‘ms’, ‘us’, and ‘ns’
                year=None,
                month=None,
                day=None,
                hour=None,
                minute=None,
                second=None,
                microsecond=None,
                nanosecond=None,
                tzinfo=None,
                *,
                fold=None)
```  

常用属性：day_of_week、day_of_year、is_leap_year、week、year、month、day、hour、minute、second
常用方法：
1. date：
2. isocalendar：Return a 3-tuple containing ISO year, week number, and weekday。
3. isoweekday：Return the day of the week represented by the date.
4. month_name：Return the month name of the Timestamp with specified locale
5. strftime(format)：
6. time：
7. weekday()：Return the day of the week represented by the date.  

#### 1.1.2 to_datetime() 方法  

该方法既可以用于单个变量，也可以用于序列  

```python
pd.to_datetime(arg,        #int, float, str, datetime, list, tuple, 1-d array, Series, DataFrame/dict-like
               dayfirst=False,
               yearfirst=False,
               format=None,
               unit=None,
               infer_datetime_format=False)
```  

### 1.2 日期序列的构造 

#### 1.2.1 DatetimeIndex 

```python
pd.DatetimeIndex(data=None,        #array-like (1-dimensional)
                 freq=NoDefault.no_default,
                 tz=None,
                 dayfirst=False,
                 yearfirst=False,
                 name=None)
```  

#### 1.2.2 to_datetime() 方法 

同上述 [1.1.2 to_datetime() 方法](#1.1.2-to_datetime()%20方法)  

#### 1.2.3 date_range() 方法  

```python
pd.date_range(start=None,      #str or datetime-like
              end=None,
              periods=None,
              freq=None)
```  

### 1.3 日期转字符串 strftime()方法  

```python
Timestamp.strftime(date_format)
DatetimeIndex.strftime(date_format)
Series.dt.strftime(date_format)
```  

## 2 日期偏置  

### 2.1 DateOffset 对象  

用于日期数据 Timestamp 的加减  

```python
pd.offsets.DateOffset(**kwargs)
pd.DateOffset(**kwargs)
```  

参数：
- years=None
- months
- weeks
- days
- hours
- minutes
- seconds
- microseconds
- nanoseconds  

### 2.2 offsets对象  

### 2.3 代替 Timedelta的部分功能  

pd.offsets.Week()
pd.offsets.Day()
pd.offsets.Hour()
pd.offsets.Minute()
pd.offsets.Second()
pd.offsets.Milli()
pd.offsets.Micro()
pd.offsets.Nano()  

### 2.4 Week(首字母 W 大写，切记)  

```python
pd.offsets.Week(n=1,
                weekday=None,  #0-6，0 for Monday
                normalize=False    #日期中的时间是否归零
                )
```  

### 2.5 BusinessDay、BDay、BusinessHour  

```python
pd.offsets.BusinessDay(n=1,
                       normalize=False,    #日期中的时间是否归零
                       weekmask = "Mon Tue Wed Thu Fri",
                       holidays=None)
#pd.offsets.BDay()
pd.offsets.BusinessHour(n=1,
                        normalize=False,    #日期中的时间是否归零
                        weekmask = "Mon Tue Wed Thu Fri",
                        start = "9:00",
                        end = "17:00")
```  

### 2.6 CustomBusinessDay、CDay、CustomBusinessHour  

```python
pd.offsets.CustomBusinessDay(n=1,
                             normalize=False,
                             weekmask = "Mon Tue Wed Thu Fri",
                             holidays=None)
```  

参数 holidays:
排除在工作日外的日期列表。  

### 2.7 关于中国的节假日  

可以使用 chinesecalendar 模块。此处利用 pandas 自己解决。由于中国的节假日涉及到农历和调休，因此可以采用列表的方式，列出所有节假日（包括正常休假的周六和周日），然后将参数 weekmask 设为 None。
以2022年为例：
```python
sundays=pd.date_range('2022-1-1','2022-12-31',freq=pd.offsets.Week(weekday=6))
saturdays=pd.date_range('2022-1-1','2022-12-31',freq=pd.offsets.Week(weekday=5))

#法定假日，根据情况添加
legal_holidays=pd.DatetimeIndex(['2022-1-1','2022-5-1','2022-10-1','2022-10-2','2022-10-3'])    

holidays = sundays.append(saturdays).append(legal_holidays).unique().sort_values()    #合并后去重、排序

bdays = ['2022-10-8','2022-10-9']    #调休成工作日的原假日，要从 holidays 中去除掉。

holidays=holidays.to_series().drop(bdays).index    #holidays 是DatetimeIndex，转成 Series 后才能 drop ，再用 index 转成DatetimeIndex
```  

### 2.8 YearEnd  

```python
pd.offsets.YearEnd(n=1,
                   month=None,        #设定年终所在的月份，不一定是12月。
                   normalize=False)
```  

### 2.9 LastWeekOfMonth  

```python
pd.offsets.LastWeekOfMonth(n=1
                           weekday=0,    #0 for Monday,the last Monday of each month
                           )
```  

## 3 关于日期偏置的几个实例  

### 3.1  求2022年9月第一个周二的日期  

#### 3.1.1 方法一，Timedelta  

```python
t1 = pd.Timestamp('2022-9-1')
week = 0    #表示第一周，week=1 表示第二周
weekday = 1 #星期二，0（星期一），6（星期日）
t1+pd.Timedelta(days=(weekday+7*week-t1.day_of_week if weekday >=t1.day_of_week else t1.day_of_week+weekday+7*week+1))    #如果 weekday 大于 t1.day_of_week，二者相减，得偏移天数，否则，二者相减得偏移天数。
```  

#### 3.1.2 方法二，date_range  

```python
s = pd.Series(pd.date_range('2022-9-1','2022-9-7'))    
s[s.dt.dayofweek==1].iloc[0]
```  

根据第几周的变化，可以修改 date_range 的日期取值范围和 iloc 的索引号。  

#### 3.1.3 方法三，offsets  

```python
pd.Timestamp('2022-9-1')+pd.offsets.WeekOfMonth(week=0,weekday=1)
```  

### 3.2 求2020年9月7日后的第30个工作日是哪一天  

#### 3.2.1 方法一，date_range  

`pd.date_range('2022-9-7',freq='30B',periods=2)[1]`  

#### 3.2.2 方法二，offsets  

`pd.Timestamp('2022-9-7') + pd.offsets.BDay(30)`  

## 4 时间差  

### 4.1 构造  

#### 4.1.1 Timedelta 类的实例化  

```python
pd.Timedelta(value=<object object>,
             unit=None,
             **kwargs)
```  

参数：
- value：Timedelta, timedelta, np.timedelta64, str, or int
- unit：当value为 int 时有效。取值范围：
    1. ‘W’, ‘D’,'H', ‘T’, ‘S’, ‘L’, ‘U’, or ‘N’
    2. ‘days’ or ‘day’
    3. ‘hours’, ‘hour’, ‘hr’, or ‘h’
    4. ‘minutes’, ‘minute’, ‘min’, or ‘m’
    5. ‘seconds’, ‘second’, or ‘sec’
    6. ‘milliseconds’, ‘millisecond’, ‘millis’, or ‘milli’
    7. ‘microseconds’, ‘microsecond’, ‘micros’, or ‘micro’
    8. ‘nanoseconds’, ‘nanosecond’, ‘nanos’, ‘nano’, or ‘ns’.
- *kwargs：weeks, days, hours, minutes, seconds, milliseconds, microseconds, nanoseconds 

#### 4.1.2 to_timedelta 函数，可以构造序列  

```python
pd.to_timedelta(arg,        #str, timedelta, list-like or Series
                unit=None,
                errors='raise')
``` 

参数：
- arg：str, timedelta, list-like or Series
- unit：当 arg 为 int 时有效。取值范围：
    - ‘W’
    - ‘D’ / ‘days’ / ‘day’
    - ‘hours’ / ‘hour’ / ‘hr’ / ‘h’
    - ‘m’ / ‘minute’ / ‘min’ / ‘minutes’ / ‘T’
    - ‘S’ / ‘seconds’ / ‘sec’ / ‘second’
    - ‘ms’ / ‘milliseconds’ / ‘millisecond’ / ‘milli’ / ‘millis’ / ‘L’
    - ‘us’ / ‘microseconds’ / ‘microsecond’ / ‘micro’ / ‘micros’ / ‘U’
    - ‘ns’ / ‘nanoseconds’ / ‘nano’ / ‘nanos’ / ‘nanosecond’ / ‘N’
- **errors：**{‘ignore’, ‘raise’, ‘coerce’}, default ‘raise’
    - If ‘raise’, then invalid parsing will raise an exception.
    - If ‘coerce’, then invalid parsing will be set as NaT.
    - If ‘ignore’, then invalid parsing will return the input.  

```python
pd.to_timedelta('1 days 06:05:01.00003')

#结果
Timedelta('1 days 06:05:01.000030')
pd.to_timedelta('15.5us')

#结果
Timedelta('0 days 00:00:00.000015500')
pd.to_timedelta(['1days 6h 5m', '15.5us', 'nan'])

#结果
TimedeltaIndex(['1 days 06:05:00.000000', '0 days 00:00:00.000015500', NaT],
               dtype='timedelta64[ns]', freq=None)
pd.to_timedelta(np.arange(5), unit='s')

#结果
TimedeltaIndex(['0 days 00:00:00', '0 days 00:00:01', '0 days 00:00:02',
                '0 days 00:00:03', '0 days 00:00:04'],
               dtype='timedelta64[ns]', freq=None)
```  

#### 4.1.3 Timestamp 的相减，可以构造序列  

```python

pd.Timestamp("1999-02-05") - pd.Timestamp("1998-05-24")

#结果

Timedelta('257 days 00:00:00')

```

  

#### 4.1.4 序列构造 TimedeltaIndex 和 timedelta_range  

```python
pd.TimedeltaIndex(data=None,    #array-like (1-dimensional)
                  unit=None,
                  freq=NoDefault.no_default,
                  closed=None,
                  dtype=dtype('<m8[ns]'),
                  copy=False,
                  name=None)
pd.timedelta_range(start=None,
                   end=None,
                   periods=None,
                   freq=None,
                   name=None,
                   closed=None)
``` 

### 4.2 比较大小  

Timedelta 的大小比较是结果的比较。  

```python
ts = pd.to_timedelta(['2 Days 3 hours ',pd.Timedelta(weeks=2)])

ts>'100 hours'

#结果
array([False,  True])
```  

### 4.3 frequency 的转换 

由于 Timedelta 是时间差，涉及到取样频率，频率不同，表现形式也不同，可以转换。TimedeltaIndex也是如此。 

```python
td=pd.to_timedelta(['1 days 2 hours 30 minutes','2 days 1 hours 3 minutes'])

td
#TimedeltaIndex(['1 days 02:30:00', '2 days 01:03:00'], dtype='timedelta64[ns]', freq=None)

#上述结果中，dtype='timedelta64[ns]'，timedelta64[ns]表示数据类型(timedelta64)和频率(ns)

td.astype('timedelta64[D]')    #转换成 days，后面的小时和分钟被省去

td.astype('timedelta64[h]')    #转换成小时，后面的分钟被省去
```  

## 5 计算 week_of_month(所在月的第几周)  

对于单个 Timestamp ，Timestamp.week 返回所在年的周数，Timestamp.isocalendar() 返回一个元组，(year,weekofyear,weekday)。对于日期序列，Series.dt.isocalendar() 返回一个元组，(year,weekofyear,day)。  

计算 week_of_month (所在月的第几周)的思路：首先计算 week_of_year(所在年的周数)，然后计算所在月的第一天的年的周数，二者相减，加上1，即为所在月的周数。  

```python 计算 week_of_month

def fu(x):
    a = pd.Timestamp(year=x.year,month=x.month,day=1)    #所在月的第一天
    result=x.week-a.week+1        .week 返回所在年的周数
    return result

pd.date_range('2022-10-1','2022-10-31').map(fu)

#结果
Int64Index([1, 1, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 3, 3, 3, 4, 4, 4, 4, 4, 4,
            4, 5, 5, 5, 5, 5, 5, 5, 6],
           dtype='int64')
```

## 6 resample  

resample 实际上是基于时间的 groupby。当使用groupby 替代时，要用 Grouper 对象  

```python
DataFrame.resample(rule,               #DateOffset, Timedelta or str
                   axis=0,
                   closed=None,        #取样区间的封闭情况，‘right’ or  ‘left’
                   label=None,
                   convention='start',
                   kind=None,
                   loffset=None,
                   base=None,
                   on=None,           #如果不是针对 index ，该参数为列名  
                   level=None,        #多级索引的级数
                   origin='start_day',
                   offset=None)
```  

## 7 日期范围的参数 freq 取值范围  

### 7.1 date_range 和 timedelta_range 中 freq的可选值  

| Alias(别名) | Description（意义） |
| ----------- |------------------- |
| B | business day frequency |
| C | custom business day frequency |
| D | calendar day frequency |
| W | weekly frequency |
| M | month end frequency |
| SM | semi-month end frequency (15th and end of month) |
| BM | business month end frequency |
| CBM | custom business month end frequency |
| MS | month start frequency |
| SMS | semi-month start frequency (1st and 15th) |
| BMS | business month start frequency |
| CBMS | custom business month start frequency |
| Q | quarter end frequency |
| BQ | business quarter end frequency |
| QS | quarter start frequency |
| BQS | business quarter start frequency |
| A, Y | year end frequency |
| BA, BY | business year end frequency |
| AS, YS | year start frequency |
| BAS, BYS | business year start frequency |
| BH | business hour frequency |
| H | hourly frequency |
| T, min | minutely frequency |
| S | secondly frequency |
| L, ms | milliseconds |
| U, us | microseconds |
| N | nanoseconds |


### 7.2 日期偏置 offsets对象的可选值  

| Date Offset | Frequency String | Description |
| ----------- | ---------------- | ----------- |
| DateOffset | None | Generic offset class, defaults to absolute 24 hours |
| BDay or BusinessDay | 'B' | business day (weekday) |
| CDay or CustomBusinessDay | 'C' | custom business day |
| Week(n,weekday=None)n表示几周,默认为1。weekday表示星期几，取值范围(0-6)。 | 'W'用法：'W-Mon',表示每周一 | one week, optionally anchored on a day of the week |
| WeekOfMonth(week=,weekday=)week表示第几周(0-3)，weekday表示星期几(0-6) | 'WOM'用法：'WOM-2Fri'，表示第二个周五 | the x-th day of the y-th week of each month |
| LastWeekOfMonth(weekday=) | 'LWOM'用法：'LWOM-Fri' | the x-th day of the last week of each month |
| MonthEnd | 'M' | calendar month end |
| MonthBegin | 'MS' | calendar month begin |
| BMonthEnd or BusinessMonthEnd | 'BM' | business month end |
| BMonthBegin or BusinessMonthBegin | 'BMS' | business month begin |
| CBMonthEnd or CustomBusinessMonthEnd | 'CBM' | custom business month end |
| CBMonthBegin or CustomBusinessMonthBegin | 'CBMS' | custom business month begin |
| SemiMonthEnd | 'SM' | 15th (or other day_of_month) and calendar month end |
| SemiMonthBegin | 'SMS' | 15th (or other day_of_month) and calendar month begin |
| QuarterEnd | 'Q' | calendar quarter end |
| QuarterBegin | 'QS' | calendar quarter begin |
| BQuarterEnd | 'BQ | business quarter end |
| BQuarterBegin | 'BQS' | business quarter begin |
| FY5253Quarter | 'REQ' | retail (aka 52-53 week) quarter |
| YearEnd | 'A' | calendar year end |
| YearBegin | 'AS' or 'BYS' | calendar year begin |
| BYearEnd | 'BA' | business year end |
| BYearBegin | 'BAS' | business year begin |
| FY5253 | 'RE' | retail (aka 52-53 week) year |
| Easter | None | Easter holiday |
| BusinessHour | 'BH' | business hour |
| CustomBusinessHour | 'CBH' | custom business hour |
| Day | 'D' | one absolute day |
| Hour | 'H' | one hour |
| Minute | 'T' or 'min' | one minute |
| Second | 'S' | one second |
| Milli | 'L' or 'ms' | one millisecond |
| Micro | 'U' or 'us' | one microsecond |
| Nano | 'N' | one nanosecond |


## 8 总结


| Concept | Scalar Class | 构造方法 | Array Class | 构造方法 | pandas Data Type |
| ------- | ------------ | -------- | ----------- | -------- | ---------------- |
| Date times | Timestamp | pd.Timestamp、to_datetime | DatetimeIndex | to_datetime 、date_range | datetime64[ns] 、 datetime64[ns, tz] |
| Time deltas | Timedelta | pd.Timedelta、to_timedelta、 | TimedeltaIndex | to_timedelta 、timedelta_range、pd.TimedeltaIndex | timedelta64[ns] |
| Date offsets | DateOffset | pd.offsets.DateOffset、pd.DateOffset | None |  | None |
  
|
  
