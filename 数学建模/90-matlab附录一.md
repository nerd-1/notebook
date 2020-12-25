## Vectors

向量

```matlab
a = [1 2 3 4 5 6 7 8 9]
t = 0 : 2 : 20     % t = 0	2	4	6	8	10	12	14	16	18	20
% 给向量内的每一个元素加2
b = a + 2    % b = [3, 4, 5, 6, 7, 8, 9, 10 ,11]
% 两个向量相加
c = a + b    % c = [4 6 8 10 12 14 16 18 20]
```

## Function

matlab收录了常用函数，例如sin，cos，tan，exp，sqrt等，还有pi，i或者j等。使用如下：

```matlab
sin(pi/4)
```

如果想要查明某个函数的具体用法，可以这样查明：

```matlab
help [function_name]
```

matlab也允许你通过函数命令写自己的函数。

## Plotting

绘图在matlab中也十分简单。使用代码如下：

```matlab
t = 0:0.25:7;
y = sin(t);
plot(t, y)
```

## Polynomials

在matlab中多项式被表示为一个向量。比如下列多项式：s<sup>4</sup>+3s<sup>3</sup>-15s<sup>2</sup>-2s+9：

```matlab
x = [1, 3, -15, -2, 9]
```

如果是s<sup>4</sup>+1，则表示为：

```matlab
x = [1, 0, 0, 0, 1]
```

计算多项式，如果多项式右边的值为2，则：

```matlab
z = polyval([1, 0, 0, 0, 1], 2)   % 输出17
```

如果直接求根，可以用roots函数，表示s<sup>4</sup>+3s<sup>3</sup>-15s<sup>2</sup>-2s-9=0的根：

```matlab
roots([1, 3, -15, -2, 9])
```

表示两个多项式相乘：

```matlab
x = [1, 2];
y = [1, 4, 8];
z = conv(x, y);   % z = [1, 6, 16, 16]
```

多项式相除：

```matlab
[xx, R] = deconv(z, y)   % z 除以 x，如果为假分式那么，xx是多项式，R是剩余的真分式
```

多项式相加，可以使用`+法`运算符，也可以直接是用polyadd函数：

```matlab
z = polyadd(x, y);
```

## Matrices

矩阵用`；`分隔：

```matlab
B = [1 2 3 4;5 6 7 8;9 10 11 12]
```

转置矩阵：

```matlab
C = B'
```

矩阵的乘法：

```matlab
D = B * C  % 顺序很重要
```

两个矩阵相应元素相乘：

```matlab
E = [1, 2; 3, 4];
F = [2, 3; 4, 5];
G = E .* F;     % G = [2, 6; 12, 20]
```

入过E是个方阵，也可以使用`E^3`来运算：

```matlab
E^3; % [37, 54; 81 118]
```

还可以使用`.^`运算：

```matlab
E .^ 3  [1 8; 27 64]
```

求逆矩阵：

“设A是数域上的一个n阶方阵，若在相同数域上存在另一个n阶矩阵B，使得： AB=BA=E。 则我们称B是A的逆矩阵，而A则被称为可逆矩阵。注：E为单位矩阵。”

```matlab
X = inv(E);    % X = [-2 1; 1.5 -0.5];
```

求矩阵本征值：

```
H = eig(E);
```

特征多项式系数的向量：

```matlab
p = poly(E);
```

记住矩阵的特征值与其特征多项式的根相同：

```matlab
roots(p)
```