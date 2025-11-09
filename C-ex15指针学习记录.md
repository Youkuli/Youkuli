# C-ex15-pointer

## 指针词库

```
type *ptr
```

`type`类型的指针，名为`ptr`。

```
*ptr
```

`ptr`所指向位置的值。

```
*(ptr + i)
```

（`ptr`所指向位置加上`i`）的值。

> 译者注：以字节为单位的话，应该是`ptr`所指向的位置再加上`sizeof(type) * i`。

```
&thing
```

`thing`的地址。

```
type *ptr = &thing
```

名为`ptr`，`type`类型的指针，值设置为`thing`的地址。

```
ptr++
```

自增`ptr`指向的位置。

我们将会使用这份简单的词库来拆解这本书中所有的指针用例。

## 声明与解引用

```
char *ptr = "Hello";  // 声明指针
```

````
char first_char = *ptr; // 获取'H'，解引用
````

## 字符串数组实际使用示例

### 创建

```
char *names[] = {
    "Alan", "Frank",
    "Mary", "John", "Lisa"
};
```

1. 每个字符串存储在ROM中,分配连续的地址如：

```
地址 0x1000: "Alan\0"
地址 0x1005: "Frank\0" 
地址 0x100B: "Mary\0"
地址 0x1010: "John\0"
地址 0x1015: "Lisa\0"
```

2. 在栈上分配空间存储字符串的地址

```
	  地址	内容			 内容
地址 0x2000: 0x1000  → 指向 "Alan"
地址 0x2008: 0x1005  → 指向 "Frank"  
地址 0x2010: 0x100B  → 指向 "Mary"
地址 0x2018: 0x1010  → 指向 "John"
地址 0x2020: 0x1015  → 指向 "Lisa"
```

3. `name`代表指针首元素的地址`0x2000`
4. 创建指向指针的指针

```
char **cur_name = names;

	 地址		内容			  内容		指令
地址 0x9000: 0x2000  → 指向 "0x1000"  *cur_name
地址 0x2000: 0x1000  → 指向 "Alan"	  
```

当我们执行 `char **cur_name = names;` 时，`names` 代表数组首元素的地址，即 0x2000。所以假设，在地址 0x9000 处存储的值是 0x2000。

因此，我们可以这样总结：

- `cur_name` 的地址是 0x9000，其中存储的值是 0x2000（即 `names` 数组的第一个元素的地址）。
- 通过 `cur_name` 可以访问 `names` 数组的第一个元素，即 `*cur_name` 得到 0x1000，也就是字符串 "Alan" 的地址。

如果我们对 `cur_name` 进行算术运算，例如 `cur_name + 1`，那么它将指向 0x2008（即 `names` 数组的第二个元素），通过 `*(cur_name + 1)` 可以得到 0x1005，即 "Frank" 的地址。

所以，在计算机内部，`cur_name` 是一个指针，它指向一个指针数组，该指针数组的每个元素指向一个字符串。

### 输出

1. 访问整个字符串：输出Alan/Frank

```
printf("names[0] = %s\n", names[0]);//输出Alan
printf("names[1] = %s\n", names[0]);//输出Frank
//OR
printf("names[0] = %s\n",*cur_name);//输出Alan
printf("names[1] = %s\n",*(cur_name+1));//输出Frank
```

2. 访问单个字符：输出A/l/a/n

```
printf("*names[0] = %c\n", *names[0]);//输出A
printf("*names[1] = %c\n", *names[1]);//输出l
printf("*names[2] = %c\n", *names[2]);//输出a
printf("*names[3] = %c\n", *names[3]);//输出n
```

3. 输出字符串的存储地址

```
printf("*names[0] = %p\n", (void*)names[0]);//Alan的存储地址
printf("*names[1] = %p\n", (void*)names[1]);//Frank的存储地址
```

4. 输出names[i]指针的地址

```
printf("*cur_name=%p\n",(void*)cur_name);//names[0]指针地址
printf("*cur_name+1=%p\n",(void*)(cur_name+1));//names[1]指针地址
//or
printf("ptr_add0=%p\n",(void*)&names[0]);//names[0]指针地址
printf("ptr_add1=%p\n",(void*)&names[1]);//names[1]指针地址
```

5. 输出cur_name指针的地址

```
printf("*cur_name=%p\n",(void*)&cur_name);//cur_name指针地址
```

