---
title: How to Flatten the dir
categories:
  - Others
---
## Backgrounds

```
/dir1
    /dir2 
       |
        --- file1
       |
        --- file2
```

 Expected:

```
/dir1
  --- file1
	--- file2
```



## Solutions

### Zip and unzip

```shell
tar -cvf all.tar *

# followed by moving all.tar to a new location then
mv all.tar /dest/
cd /dest/

# 4 means the deepth of the directory
tar -xvf all.tar --strip=4
```

### find and cp

```shell
find . -iname '*.*' -exec cp \{\} /dest/ \;
```

