# 中文文件编码在linux系统乱码问题

经常遇到中文文件上传到linux上出现乱码的问题，原因是在不同文件系统中文件名存储的编码不一致
在linux上可以考虑将文件名转换为utf8即可
convmv工具可以解决这个问题

```
convmv -f 源编码 -t 新编码 [选项] 文件名

convmv -f gbk -t utf8 -r MY_DIR
```
