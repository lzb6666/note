## 基本定义及大小
类型|大小|作用
---|---|---
CHAR	|0-255字节	|定长字符串
VARCHAR	|0-65535 字节	|变长字符串
TINYBLOB	|0-255字节	|不超过 255 个字符的二进制字符串|
TINYTEXT	|0-255字节|	短文本字符串
BLOB	|0-65 535字节	|二进制形式的长文本数据
TEXT	|0-65 535字节	|长文本数据
MEDIUMBLOB	|0-16 777 215字节	|二进制形式的中等长度文本数据
MEDIUMTEXT	|0-16 777 215字节	|中等长度文本数据
LONGBLOB	|0-4 294 967 295字节	|二进制形式的极大文本数据
LONGTEXT	|0-4 294 967 295字节	|极大文本数据
## char
char(X)中的X表示最大字符数，不区分中英文；但最大大小为255个字节，因此中文
储存数量肯定少于英文（具体存储字符数看编码）；  

char对不足所设置的X的字符数都会补空格，因此若插入的字符串尾部是空格，就会
丢失；
## varchar
varchar(X)表示最多储存X个字符，不区分中英文；尾部不用补空格，因为varchar
会用初始的1到2个字节记录长度，这里这长度以字节为单位。  
这里的65535是最大行长度（max row size），包含1个字节的null标志，1到2个字
节的长度记录，所以最大可用储存大小为65533（当该列定义为not null时）或
65532
```
ERROR 1074: Column length too big for column 'max_varchar' (max = 21845); use BLOB or TEXT instead
SQL Statement:
ALTER TABLE `local_test`.`char_test` 
ADD COLUMN `max_varchar` VARCHAR(30000) NULL AFTER `varchar_col`
```
## text
text常用来储存大文本数据，如果硬要储存二进制数据，需要经过base64之类的编
码；  
会有额外的空间储存长度，当text过长时，会在数据处理保留指针，指向实际数据
## blob
blob准确的说并不能算作字符类型，其本身是一个二进制大对象，能够储存二进制
数据，因此实际上可以储存任何数据；