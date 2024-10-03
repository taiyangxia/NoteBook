# 二分
当使用 while(l<=r) 作为循环条件时
    1、当target不存在时
    l指向第一个比target大的值；r指向第一个比target小的值
    2、存在target，且有可能重复时。要求查找最左边的target
    这时可以修改 nums[target]==target的执行：不return middle
    而是 r--；这样循环结束时，就可以得到：r指向第一个比target小的数，而$\color{red}{l永远指向r的后一位，所以可以得到：l要么指向左边第一个target；要么出界}$
    