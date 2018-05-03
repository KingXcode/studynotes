math.h 数学函数库
---
一些数学计算的公式的具体实现是放在math.h里,具体有：

>以下的函数 参数都是double类型，实际上函数库中还有与long，float类型相关的函数，但都只是以下函数的函数名变化

1. 三角函数

 `double sin (double x);`  x的正弦值

 `double cos (double x);`  x的余弦值

 `double tan (double x);`  x的正切值

2. 反三角函数

 `double asin (double x);` 结果介于[-PI/2, PI/2]，x值域为[-1,1]

 `double acos (double x);` 结果介于[0, PI],x值域为[-1,1]

 `double atan (double x);` 反正切(主值), 结果介于[-PI/2, PI/2]

 `double atan2 (double y, double x);` 反正切(整圆值), 结果介于[-PI, PI]

3. 双曲三角函数

 `double sinh (double x);`  x的双曲正弦值

 `double cosh (double x);`  x的双曲余弦值

 `double tanh (double x);`  x的双曲正切值

4. 指数与对数

 `double exp (double x);`  幂函数e^x

 `double pow (double x, double y);` x^y，如果x=0且y<=0,或者x<0且y不是整型数，将产生定义域错误

 `double sqrt (double x);` x的平方根，其中x>=0

 `double log (double x);` 以e为底的对数,自然对数，x>0

 `double log10 (double x);` 以10为底的对数，x>0

5. 取整

 `double ceil (double x);` 取上整

 `double floor (double x);` 取下整

6. 绝对值

 `double fabs (double x);`  x的绝对值

7. 标准化浮点数

 `double frexp (double x, int *exp);` 标准化浮点数, x = f * 2^exp, 已知x求f, exp ( x介于[0.5, 1] )并返回f值

 `double ldexp (double x, int exp);` 与frexp相反, 已知x, exp求x*2^exp

8. 取整与取余

 `double modf (double x, double *ip);` 将参数的整数部分通过指针回传, 返回小数部分，整数部分保存在*ip中

 `double fmod (double x, double y);` 返回两参数相除x/y的余数,符号与x相同。如果y为0，则结果与具体的额实现有关
