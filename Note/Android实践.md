# Android实践

1. scaleType中只有fitXY会改变图片的比例,其他的均会保持图片比例
2. 判断View是否显示 `View.isShown()`
3. TextView提示用户输入有误 `TextView.setError()`
4. **SparseArray** 替换 **HashMap**
   1. 采用压缩的方式表示洗漱数组的数据，节约内存空间
   2. 存取数据采用的是二分查找法
   3. 主要有`SparseLongArray SparseIntArray SparseBooleanArray SparseArray`
   4. 避免了自动装箱过程
5. http://www.trinea.cn/android/performance/

