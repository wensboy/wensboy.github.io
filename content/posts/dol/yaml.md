### pre

yaml是一种简单的资料声明格式, 通过utf-8编码格式的.yaml或.yml后缀的文件定义. 采用yaml替代json主要是考虑简单编写自定义配置方便, 当然完全可以看心情来采用不同格式, 同时也需要在一定程度上考虑语言的适配性.

### syntax

**tips: 因为基本上很简单, 这里直接引导到yaml官方参考, 留出一点空间作为易错点提供给后面记录.**

- [yaml.org](https://yaml.org)

### mark

```yaml
# 1) 序列容器
tuple: [one,two,...] # 单一元素
object_tuple:
  - person: # 对象元素
    - name:
    - age:
  - person:
    - name:
    - age:
  - ...
```