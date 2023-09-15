
# 1、删除指定文件夹下的所有文件

```bash
#!/bin/bash
dir=$HOME/tmp
if [[ -d ${dir} ]];then
  for file in `ls ${dir}`
  do
    rm -rf "$dir/${file}"
  done
fi
```

