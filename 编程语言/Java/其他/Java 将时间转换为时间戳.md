# Java 将时间转换为时间戳

```java
    /**
     * 将时间转换为时间戳
     *
     * @param s 2019-03-01 18:00:00
     * @return res
     */
    public static String dateToStamp(String s) {
        String res = null;
        Date date;
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        try {
            date = simpleDateFormat.parse(s);
            long ts = date.getTime();
            res = String.valueOf(ts);
        } catch (ParseException | NullPointerException e) {
            e.printStackTrace();
        }
        return res;
    }


    /**
     * 将时间戳转换为时间
     *
     * @param s 传入时间戳 1551434400000
     * @return 2019-03-01 18:00:00
     */
    public static String stampToDate(String s) {
        String res;
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        long lt = new Long(s);
        Date date = new Date(lt);
        res = simpleDateFormat.format(date);
        return res;
    }

    /**
     *
     * @param s 通过时间戳获取到时间的 1551434400000
     * @return 年月日 2019-3-1
     */
    public static String stampToDate1(String s) {
        String res;
        long lt = new Long(s);
        Date date = new Date(lt);
        res = DateFormat.getDateInstance().format(date);
        return res;
    }
```

‍
