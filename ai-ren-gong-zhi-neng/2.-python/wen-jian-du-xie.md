# 文件读写



```
with open(json_file, 'r', encoding="utf-8") as f:
    json_string = f.read()
```

```
with open(output_file_name, 'a+', encoding="utf-8") as f:
    f.write(cleared_text)
```

with open, 无需close

```
    fin = open(input_file_name, 'r', encoding="utf-8")
    fout = open(output_file_name, 'w+', encoding="utf-8")
    
    for line in fin:
        for word in delete_word_list:
            line = line.replace(word, '')
        fout.write(line)
    
    fin.close()
    fout.close()
```

* r : 读取文件，若文件不存在则会报错
* w: 写入文件，若文件不存在则会先创建再写入，会覆盖原文件
* a : 写入文件，若文件不存在则会先创建再写入，但不会覆盖原文件，而是追加在文件末尾
* rb,wb：分别于r,w类似，但是用于读写二进制文件
* r+ : 可读、可写，文件不存在也会报错，写操作时会覆盖
* w+ : 可读，可写，文件不存在先创建，会覆盖
* a+ ：可读、可写，文件不存在先创建，不会覆盖，追加在末尾
