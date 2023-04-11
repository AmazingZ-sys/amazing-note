##### Django2.2版本报错解决方案

```python
AttributeError: 'str' object has no attribute 'decode'
// 在虚拟环境venv中找到
// \venv\lib\site-packages\django\db\backends\mysql\operations.py
// 在这个文件中找到报错的行，将decode改为encode
```

