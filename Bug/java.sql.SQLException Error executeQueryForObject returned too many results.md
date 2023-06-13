# java.sql.SQLException Error executeQueryForObject returned too many results

报错：返回多条数据
![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211231183326530-243807796.png)

![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211231183522375-1878047712.png)
![image](https://img2020.cnblogs.com/blog/2402369/202112/2402369-20211231183606113-1859984213.png)
问题原因：导入数据有两条相同的。反馈，没有定义id，无法指定唯一，导致数据重复！！

解决方法：

<details>
<summary>点击查看代码</summary>

```
# 删除相同数据，插入一条数据
DELETE FROM `employee_dynamic` WHERE `dynamic_code` = '2100000445392';
INSERT INTO `employee_dynamic` VALUE ('2100000445392', 'llC4F36DF8BD4C4CBD1ED13DAE909366EEAC8AA5805B95D9C8', 0,0);

# 用limit设置返回数据只有一条
SELECT authkey  FROM employee_dynamic ed WHERE dynamic_code = '2100000445392' LIMIT 1,1;
```

</details>
