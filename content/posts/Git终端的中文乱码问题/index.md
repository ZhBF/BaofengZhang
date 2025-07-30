

+++

title = 'Git终端的中文乱码问题'

+++

# Git终端的中文乱码问题

对于git终端不显示中文而是显示八进制字符编码的问题

![image-20250730194646933](assets/image-20250730194646933.png)

可以使用如下命令解决。

```bash
git config --global core.quotepath false
```

