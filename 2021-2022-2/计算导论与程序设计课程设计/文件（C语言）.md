# 文件（C语言）

## 1. 数据的管理与组织



## 2. 文件概述



## 3. 文件的打开和关闭

### fopen()

```C
FILE *fopen(const char *filename, const char *mode);
```

**函数原型：** `stdio.h`

> 打开以`filename`所指向的字符串为文件名的文件，使之与一个流关联。返回一个指向`FILE`类型结构变量的指针（后称文件指针）。若打开失败，返回一个空指针`NULL`。

**文本文件（二进制文件）操作模式：**

|   mode    | description                                                  |
| :-------: | :----------------------------------------------------------- |
| **r(b)**  | **打开一个文本文件，可以读取文件。**                         |
| **w(b)**  | **打开一个文本文件，可以写入文件（但会先删除文件原内容）。如果文件不存在，则创建。** |
| **a(b)**  | **打开一个文本文件，可以写入文件，向已有文件的尾部追加内容，如果该文件不存在则创建之。** |
| **r(b)+** | **打开一个文本文件，可以进行更新（即读和写）**               |
| **w(b)+** | **打开一个文本文件，可以进行更新（即读和写）（但会先删除文件原内容）。如果文件不存在，则创建之。** |
| **a(b)+** | **打开一个文本文件，可以进行更新（即读和写），向已有文件的尾部增加内容）。如果文件不存在，则创建之。** |

**注：**

- 如果不能打开指定的文件，则`fopen()`函数返回一个空指针`NULL` 其值在头文件`stdio.h`中被定义为０）。   

    为增强程序的可靠性，常用下面的方法打开一个文件：   

    ```C
    if ((fp = fopen("clients.dat", "w")) == NULL)
        printf("Can't open the file.");
    else
        ...
    ```

- C程序使用一个`\n`表示行尾，Windows文本文件使用回车换行的组合`\r\n`来表示行尾。C程序读取文本文件时，会将`\r\n`转换为`\n`；而写入文件时，会将`\n`转换为`\r\n`。   

- 对文件操作的库函数，函数原型均在头文件`stdio.h`中。后续函数不再赘述。   



### fclose() 

```C
int fclose(FILE *stream);
```

> 关闭文件指针`stream`所指向的文件，将所有未回写缓冲数据写入文件，断开文件和流的关联，释放文件占用的资源，如缓冲区、FILE类型记录占用的内存等。如果正常关闭了文件，则函数返回值为０；否则，返回`EOF`。





## 4. 位置指针和文件定位

**文件位置：** 文件中有一个“文件位置指针”，指向下一次读写操作所在字节的整数值。每次读写1个（或1组）数据后，系统自动将文件位置指针移动到下一个读写位置上。  

打开文件时位置指针的起始位置与打开文件的方式有关：

- 以`"r"`、`"w"`方式打开文件，位置指针指向文件头。

- 以`"a"`方式打开文件，位置指针指向文件尾



### rewind()

```C
void rewind(FILE *stream);
```

> 把 “文件位置指针” 重新定位到文件的起始位置（即0字节处）。



### fseek()

```C
int fseek(FILE *stream, long offset, int whence);
```

> 将指定文件的位置指针，从`whence`开始，移动指定的字节数`offset`。

- `whence`三种取值（`stdio.h`中的宏）：

  | macro      | value | meaning      |
  | ---------- | ----- | ------------ |
  | `SEEK_SET` | 0     | 表示文件头   |
  | `SEEK_CUR` | 1     | 表示当前位置 |
  | `SEEK_END` | 2     | 表示文件尾   |

- `offset`（类型为`long`）：

  以参照点为起点，向文件尾方向（位移量 > ０）或文件头方向（位移量 < ０）移动的字节数。



### ftell()

```C
long ftell(FILE *stream);
```

> 返回文件位置指针的当前位置（用相对于文件头的位移量表示）。

如果返回值为-1L，则表明调用出错。例如：

```C
	...
	offset = ftell(fp);
	if (offset == -1L)
        printf("ftell() error\n");
```



### 函数使用示例

```C
#include<stdio.h>
int main()
{
    FILE *fp;
    if ((fp = fopen(dict.dic)) == NULL)
        printf("Can't open the file.\n");
    else {
        printf("文件位置指针是：%ld\n", ftell(fp));
        rewind(fp); // rewind the file pointer
        printf("文件的位置是：%ld\n", ftell(fp));
        fseek(fp, 10, SEEK_CUR); // relocate the file pointer
        printf("文件的位置是：%ld\n", ftell(fp));
        fclose(fp); //close the file
    }
    return 0;
}
```






## 5. 文件的读写操作

### (1) 读写文件中的一个字符（文本文件）

#### fputc()

```C
int fputc(int c, FILE *stream);
```

> 将`c`所指定字符写入`stream`所指向的输出流中。与流相关的文件位置指针将向前移动1个字节（即指向下一个写入位置）。如果输出成功，则函数返回值就是输出的字符数据；否则，返回一个符号常量`EOF`（其值在头文件`stdio.h`中，被定义为`-1`）。    

`fputc(c, stdout)`等价于`putchar(c)` 将变量c输出到标准输出设备上（一般为屏幕）。

#### fgetc()

```C
int fgetc(FILE *stream);
```

> 从`stream`所指向的输入流中获取下一个``unsigned char`类型的字符(如果有的话）并转换成`int`类型，同时将文件位置指针向前移动1个字节。函数返回值为读取的字符，如果到文件尾，则设置该流的文件结束指示符，并返回`EOF`；如果出现读错误，则设置该流的错误指示符，并返回`EOF`。   

`ch = fgetc(stdin)`等价于`ch = getchar()`从标准输入流（通常是键盘）中读取一个字符。   

​    

**读文件时如何判断文件是否读到文件尾**

在对ASCII码文件执行读入操作时，如果遇到文件尾，则读操作函数返回一个文件结束标志`EOF`（其值在头文件`stdio.h`中被定义为`-1`）。  
在对二进制文件执行读入操作时，必须使用库函数`feof()`来判断是否遇到文件尾。    

​      

#### feof()

```C
int feof(FILE *stream);
```

**功能：** 在执行读文件操作时，如果遇到文件尾，则函数返回逻辑真`1`；否则，则返回逻辑假`0`。`feof()`函数同时适用于ASCII码文件和二进制文件。



**EXAMPLE 1**

设计函数，将键盘上输入的字符串（以 Ctrl+Z 作为结束字符）逐个字符存储到指定文本文件中。

```C
void writeFileFromKeyboard(char * filename)
{
    char ch;
   	FILE *fPtr = NULL;
   	if((fPtr = fopen(filename, "w")) != NULL){;	//打开文件 
   		while((ch = getchar()) != EOF) 	//从键盘读取字符写入文件 
           	fputc(ch, fPtr);               
       	fclose(fPtr); 	//关闭文件。执行本语句后，数据将写入文件中。
                		//如果缓冲区未满且不关闭文件，则数据不会写入文件中。
   } 
}
```

​        

**EXAMPLE 2**

设计函数：将一个文本文件逐个字符复制到另一 文本文件中。

```C
void copyFile(char * sourceFileName,char * destFileName)
{
    char ch;
    FILE * sourcefPtr,*destfPtr;
    if( (sourcefPtr = fopen(sourceFileName, "r")) == NULL)
        printf("can't open the source file\n");
   	else if( (destfPtr = fopen(destFileName, "w")) == NULL)
       	printf("can't open the dest file\n");
   	else{
       	ch = fgetc(sourcefPtr)；		//从源文件读取一个字符
       	while(!feof(sourcefPtr)){	//逐字符复制
           	fputc(ch, destfPtr); 	//将字符写入目标文件
           	ch = fgetc(sourcefPtr)；
     }
     fclose(sourcefPtr); 
     fclose(destfPtr);
   }
}
```




### (2) 读写文件中的一行字符串（文本文件）

#### fputs()

```C
int fputs(const char *s, FILE *stream);
```

**功能：** 将`s`所指向的字符串写入`stream`所指向的流中(`'\0'`不被写入)。同时将文件位置指针向前移动字符串长度个字节，指向下一写入位置。如果发生写错误，则函数返回`EOF`；否则，返回一个非负值。

`fputs(s, stdout)`等价于`puts(s)`，将`s`所指向的字符串写入到标准输出流。



#### fgets()

```C
char *fgets(char *s, int n, FILE *stream);
```

> 从`stream`指向的流中读取最多 n－1 个字符并放到`s`所指向的数组中，遇到下面情况不再往后读：
>
>- 读到新行符`'\n'`或者文件结束符 **（新行符会被读入到`s`）** ；
>- 虽未遇新行符和文件结束符，但已读入 n-1 个字符。   
>
>最后一个字符读入数组后接着写入结束标志`'\0'` ，并将文件位置指针向前移动 n－1 （字符串长度）个字节。如果遇到文件结束符并且没有字符读入数组，则数组内容不变。
>

`fgets(s, sizeof(s), stdin)`不等价于`gets(s)`，因为前者会读最后的换行符，后者抛弃。

​     

**EXAMPLE 3**

设计函数，将键盘上输入的若干字符串（以 Ctrl+Z 作为结束字符），存储到指定文本文件中。

```C
void writeFileFromKeyboard(char * filename)
{
   	char s[100];
   	FILE * fPtr = NULL;
   
   	if((fPtr = fopen(filename,"w")) != NULL){	//打开文件 
       	fgets(s, sizeof(s), stdin); //从键盘读取一行字符 
      	while (!feof(stdin)){
           	fputs(s, fPtr); //将一行字符写入文件中 
           	fgets(s, sizeof(s), stdin);
      	}                
   
       	fclose(fPtr); 
   } //end of if
}

```



**EXAMPLE 4**

设计函数：将一个文本文件逐行复制到另一文本文件中。

```C
void copyFile(char * sourceFileName, char * destFileName)
{
   	char s[100];
   	FILE *sourcefPtr, *destfPtr;
   	if((sourcefPtr = fopen(sourceFileName, "r")) == NULL)
        printf("can't open the source file\n");
   	else if((destfPtr = fopen(destFileName, "w")) == NULL)
        printf("can't open the dest file\n");
   	else{                
        fgets(s, sizeof(s), sourcefPtr); // 从源文件读取一行字符到数组s
        while(!feof(sourcefPtr)){
             fputs(s, destfPtr); //将s中的字符串写入文件中
             fgets(s, sizeof(s), sourcefPtr);
       	}        
     fclose(sourcefPtr); 
     fclose(destfPtr);
   }
}

```



#### 思考

将以语句一改写成以语句二会发现最后一个字符串会输出两次（如右图所示）！为什么？

Code 1:

```C
fgets(s, sizeof(s), stdin);
while(!feof(stdin)) {
 	fputs(s, stdout);
 	fgets(s, sizeof(s), stdin);
}
```

Code 2:

```C
while(!feof(stdin)) {				
	fgets(s, sizeof(s), stdin);
    fputs(s, stdout);
}
```

Input:

```
This is an example
^Z
```

Output 1:

```
This is an example
```

Output 2:

```
This is an example
This is an example
```

- 只有当读取了文件结束符后，`feof()`才返回`1`。
- `fgets()`如果遇到文件结束符并且没有字符读入数组，则数组内容不变    




### (3) 对文件格式化读写（文本文件）

`fscanf()`和`fprintf()`函数，用于ASCII文件的处理。  
其功能与`scanf()`和`printf()`函数相似，区别在于：`fscanf()`和`fprintf()`函数的操作对象是指定文件，而`scanf()`和`printf()`函数的操作对象是标准输入输出文件。    




#### fscanf()

```C
int fscanf(FILE *stream, const char *format, address);
```

EXAMPLE: `fscanf(fp, "%d, %f", &i, &f);`    

`fscanf(stdin, const char *format, address)`等价于`scanf(const char *format, address)`



#### fprintf()

```C
int fprintf(FILE *stream, const char *format, address);
```

EXAMPLE: `fprintf(fp, "%2d,%6.2f", i, f);`

`fprintf (stdout，const char * format，address)`等价于`printf (const char * format，address)`




### (4) 读写一个数据块（二进制文件）

实际应用中，常常要求一次读写 1 个数据块。为此，ANSIC 标准设置了`fread( )`和`fwrite()`函数，用于读写二进制文件。

#### fread()

```C
int fread(void *buffer, int size, int count, FILE *stream);
```

> 从`stream`所指向的流中读取数据到`buffer`所指向的数组中，`size`表示单个数组元素的大小，最多读取`count`个数组元素，流的文件位置指针根据成功写入的字节数递增。     
>
> 函数返回成功读入的元素个数。如果发生读错误，则返回值可能小于`count`。

EXAMPLE

```C
struct student stu[3], aStu;
// 从文件读取3条记录写入数组stu中
fread(stu, sizeof(struct student), 3, stream);
//一条记录写入结构变量aStu中
fread(&aStu, sizeof(struct student), 1,  stream);
//读取一个字节写入字符变量ch中
fread(&ch, sizeof(char), 1, stream);
```




#### fwrite()

```C
int  fwrite(void *buffer, int size, int count, FILE *stream);
```

> 将`buffer`所指向的数组的内容写入`stream`所指向的流中。`size`表示单个数组元素的大小, 最多写入`count`个数组元素。流的文件位置指针根据成功写入的字节数递增。函数返回成功写入的元素个数，如果遇到写错误，返回值可能小于`count`。       

**`fread()`和`fwrite()`函数，用于二进制文件的处理（因为二进制文件存储记录时每一个记录是等长的）。**      

EXAMPLE

```C
struct student stu[3], aStu;
//把数组stu中所有元素一次性写入文件中
fwrite(stu, sizeof(struct student ), 3, stream);
//把变量aStu中内容写入文件中
fwrite(&aStu, sizeof(struct student), 1, stream);
//读取一个字节写入字符变量ch中
fwrite(&ch, sizeof(char), 1, stream);
```



#### EXAMPLE 1

用文本文件保存不同类型的数据

```C
#include<stdio.h>
int main(void)
{
    char ch='a',ch1;
    short a=10,a1;
    int b=100,b1;
    long c=1000,c1;
    float d=1.0,d1;
    double e=100.10,e1;
    int arr1[5]={1,2,3,4,5},arr[5];
    FILE * fptr1;
    int i;
    //将上述变量写入文本文件types.txt中 
    if( (fptr1=fopen("types.txt","w"))==NULL) 
        printf("can't open file1\n");
    else{
        fprintf(fptr1,"%c\n",ch);
        fprintf(fptr1,"%d\n",a); 
        fprintf(fptr1,"%d\n",b); 
        fprintf(fptr1,"%ld\n",c);   
        fprintf(fptr1,"%f\n",d);
        fprintf(fptr1,"%lf\n",e);
        for(i=0;i<=4;i++)
            fprintf(fptr1,"%3d",arr1[i]);
        fclose(fptr1);
    }
    //从文本文件types.txt中读取不同类型的数据到变量 
    if( (fptr1=fopen("types.txt","r"))==NULL) 
        printf("can't open file1\n");
    else{
        fscanf(fptr1,"%c",&ch1);
        fscanf(fptr1,"%d",&a1);
        fscanf(fptr1,"%d",&b1);
        fscanf(fptr1,"%ld",&c1);
        fscanf(fptr1,"%f",&d1);
        fscanf(fptr1,"%lf",&e1);
        for(i=0;i<=4;i++)
                fscanf(fptr1,"%d",&arr[i]);
        fclose(fptr1);
        //输出变量到显示器
        printf("ch1=%c\n",ch1);
        printf("a1=%d\n",a1);
        printf("b1=%d\n",b1);
        printf("c1=%ld\n",c1);
        printf("d1=%f\n",d1);
        printf("e1=%lf\n",e1);
        for(i=0;i<=4;i++)
            printf("a[%d]=%d\n",i,arr[i]);
    }
}
```

#### EXAMPLE 2

用二进制文件保存不同类型的数据

```C
#include<stdio.h>
int main()
{
    char ch = 'a', ch1;
    short a = 10, a1;
    int b = 100, b1;
    long c = 1000, c1;
    float d = 1.0, d1;
    double e = 100.10, e1;
    int arr1[5] = {1, 2, 3, 4, 5}, arr[5];
    FILE * fptr;
    int i;
    //将上述变量写入二进制文件 types.dat
    if( (fptr = fopen("types.dat","wb")) == NULL) 
        printf("can't open file1\n");
    else{
        fwrite(&ch, sizeof(char), 1, fptr);
        fwrite(&a, sizeof(short), 1, fptr); 
        fwrite(&b, sizeof(int), 1, fptr); 
        fwrite(&c, sizeof(long), 1, fptr);   
        fwrite(&d, sizeof(float), 1, fptr);
        fwrite(&e, sizeof(double), 1, fptr);
        fwrite(arr1, sizeof(int), 5, fptr);
        fclose(fptr);
    }
    //从二进制文件types.dat中读取上述数据进行输出 
    if( (fptr = fopen("types.dat","rb")) == NULL) 
        printf("can't open file1\n");
    else{
        fread(&ch1, sizeof(char), 1, fptr);
        fread(&a1, sizeof(short), 1, fptr); 
        fread(&b1, sizeof(int), 1, fptr); 
        fread(&c1, sizeof(long), 1, fptr);   
        fread(&d1, sizeof(float), 1, fptr);
        fread(&e1, sizeof(double), 1, fptr);
        fread(arr, sizeof(int), 5, fptr);
        fclose(fptr);
        printf("ch1=%c\n", ch1);
        printf("a1=%d\n", a1);
        printf("b1=%d\n", b1);
        printf("c1=%ld\n", c1);
        printf("d1=%f\n", d1);
        printf("e1=%lf\n", e1);
        for(i = 0 ;i <= 4 ; i++)
            printf("a[%d]=%d\n", i, arr[i]);
    }
    return 0;
}
```





## 6. 顺序文件的操作







## 7. 随机文件的操作







