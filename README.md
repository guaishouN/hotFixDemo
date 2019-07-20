
# hotFixDemo
使用反射classLoader的原理实现热修复，这个例子修复的是activity

修复原理：
Android加载类时，BaseClassLoader会从头开始找加载要加载的class，使用反射修改BaseClassLoader里的DexPathList的Elements[]。
可以利用DexClassLoader加载动态下载的.dex文件，而PathClassLoad加载的是安装的app的apk文件。

修复步骤：
1 利用分包设置，把问题Activity打包到classes2.dex

2 把这个classes2.dex放到SDCard中，再拷贝到app私有目录下

3 利用DexClassLoader将加载新的classes2.dex到新Elements[]

4 利用contex.pathClassLoader获取原来的加载已有的Elements[]

5 把新Elements[]插入到已有的Elements[]的前面

6 最后用最新的Elements[]代替contex.pathClassLoader里的DexPathList的Elements[]

7 在加载问题activity.class前先做修复操作就能实现热修复。
