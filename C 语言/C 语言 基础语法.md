# C 语言 基础语法


这样，我们就知道该如何打印我们变量的值了，当然，除了使用`%d`打印有符号整数之外，还有其他的：

| 格式控制符                      | 说明                                                                                                                                                                                      |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| %c                              | 输出一个单一的字符                                                                                                                                                                        |
| %hd、%d、%ld                    | 以十进制、有符号的形式输出 short、int、long 类型的整数                                                                                                                                    |
| %hu、%u、%lu                    | 以十进制、无符号的形式输出 short、int、long 类型的整数                                                                                                                                    |
| %ho、%o、%lo                    | 以八进制、不带前缀、无符号的形式输出 short、int、long 类型的整数                                                                                                                          |
| %#ho、%#o、%#lo                 | 以八进制、带前缀、无符号的形式输出 short、int、long 类型的整数                                                                                                                            |
| %hx、%x、%lx %hX、%X、%lX       | 以十六进制、不带前缀、无符号的形式输出 short、int、long 类型的整数。如果 x 小写，那么输出的十六进制数字也小写；如果 X 大写，那么输出的十六进制数字也大写。                                |
| %#hx、%#x、%#lx %#hX、%#X、%#lX | 以十六进制、带前缀、无符号的形式输出 short、int、long 类型的整数。如果 x 小写，那么输出的十六进制数字和前缀都小写；如果 X 大写，那么输出的十六进制数字和前缀都大写。                      |
| %f、%lf                         | 以十进制的形式输出 float、double 类型的小数                                                                                                                                               |
| %e、%le %E、%lE                 | 以指数的形式输出 float、double 类型的小数。如果 e 小写，那么输出结果中的 e 也小写；如果 E 大写，那么输出结果中的 E 也大写。                                                               |
| %g、%lg %G、%lG                 | 以十进制和指数中较短的形式输出 float、double 类型的小数，并且小数部分的最后不会添加多余的 0。如果 g 小写，那么当以指数形式输出时 e 也小写；如果 G 大写，那么当以指数形式输出时 E 也大写。 |
| %s                              | 输出一个字符串                                                                                                                                                                            |

‍

最后，我们来总结一下前面认识的所有运算符的优先级，从上往下依次降低：

| 运算符              | 解释                                 | 结合方式 |
| ------------------- | ------------------------------------ | -------- |
| ()                  | 同数学中的括号，直接提升到最高优先级 | 由左向右 |
| ! ~ ++ -- + -       | 否定，按位否定，增量，减量，正负号   | 由右向左 |
| * / %               | 乘，除，取模                         | 由左向右 |
| + -                 | 加，减                               | 由左向右 |
| << >>               | 左移，右移                           | 由左向右 |
| < <= >= >           | 小于，小于等于，大于等于，大于       | 由左向右 |
| == !=               | 等于，不等于                         | 由左向右 |
| &                   | 按位与                               | 由左向右 |
| ^                   | 按位异或                             | 由左向右 |
|                     |                                      | 按位或   | 由左向右 |
| &&                  | 逻辑与                               | 由左向右 |
|                     |                                      |          | 逻辑或   | 由左向右 |
| ? :                 | 条件                                 | 由右向左 |
| = += -= *= /= &= ^= | = <<= >>=                            | 各种赋值 | 由右向左 |
| ,                   | 逗号（顺序）                         | 由左向右 |

‍
