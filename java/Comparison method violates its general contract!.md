此bug为重写对象比对（排序）方法时写的不够严谨，导致违反了一下规则：

- 自反性：x，y 的比较结果和 y，x 的比较结果相反。

- 传递性：x>y,y>z,则 x>z。

- 对称性：x=y,则 x,z 比较结果和 y，z 比较结果相同。