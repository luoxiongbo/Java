 由于C语言本身不存在[哈希](https://so.csdn.net/so/search?q=哈希&spm=1001.2101.3001.7020),因此，我们可以调用开源的第三方头文件，**这只是一个头文件**：uthash.h，使用时只需要在文件首部编写#include<uthash.h>即可。

# uthash的使用

```cpp
#include "uthash.h"
struct my_struct {
    int id;                    /* key */
    char name[10];
    UT_hash_handle hh;         /* makes this structure hashable */
};
/*声明哈希为NULL指针*/
struct my_struct *users = NULL;    /* important! initialize to NULL */
```

这里我们将id作为一个索引值，也就是键值，将name作为value。

**注意**：一定要包含UT_hash_handle hh（在之后的个别函数中可能会用到这个UT_hash_handle）；hh不需要初始化，它可以命名为任何名称，一般都命名为hh。

##  查找元素

```cpp
struct my_struct *find_user(int user_id) {    // 返回哈希结构体类型的变量
    struct my_struct *s;
    HASH_FIND_INT( users, &user_id, s );  /* s: output pointer */
    return s;    // 如果存在就返回正确的地址，反之返回空地址NULL
}
```

在上述代码中，第一个参数users是哈希表，**第二个参数是user_id的地址**（**一定要传递地址**）函数的用法而已不必太在意原理。最后s是输出变量。当可以在哈希表中找到相应键值时，s返回给定键的结构，当找不到时s返回NULL。

## 添加元素

​    HASH_ADD_INT表示添加的键值为int类型
  HASH_ADD_STR表示添加的键值为字符串类型
  HASH_ADD_PTR表示添加的键值为指针类型
  HASH_ADD表示添加的键值可以是任意类型

```cpp
void add_user(int user_id, char *name) {
    struct my_struct *s;
    /*重复性检查，当把两个相同key值的结构体添加到哈希表中时会报错*/
    HASH_FIND_INT(users, &user_id, s);  /* id already in the hash? */
    /*只有在哈希中不存在ID的情况下，我们才创建该项目并将其添加。否则，我们只修改已经存在的结构。*/
    if (s==NULL) {
      s = (struct my_struct *)malloc(sizeof *s);    //现在才实际分配空间
      s->id = user_id;
      HASH_ADD_INT( users, id, s );  /* id: name of key field */
    }
    strcpy(s->name, name);
}
```

HASH_ADD_INT函数中，第一个参数users是哈希表，第二个参数id是键字段的名称。最后一个参数s是指向要添加的结构的指针。

![img](https://img-blog.csdnimg.cn/68d6a2eca25b43d2aa48d4eabe976daa.png)

## 替换元素

实际上 HASH_REPLACE_INT 和 HASH_ADD_INT 相差无几，先查看某一键值是否存在，如果不存在就不予以替换，如果存在就予以替换

```cpp
void replace_user(HashHead *head, HashNode *newNode) {
    HashNode *oldNode = find_user(*head, newNode->id);
    if (oldNode)
        HASH_REPLACE_INT(*head, id, newNode, oldNode);
}
```



## 删除元素

```cpp
void delete_user(struct my_struct *user) {
    HASH_DEL(users, user);  /* user: pointer to deletee */
    free(user);             /* optional; it's up to you! */
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**注意**：

1. 从哈希表中删除结构，必须具有指向它的指针。（如果只有键，请先执行HASH_FIND以获取结构指针）。
2. 这里users是哈希表，user是指向我们要从哈希中删除的结构的指针。
3. 删除结构只是将其从哈希表中删除，实际内存空间没有变，并非free 。何时释放要删除的这个结构的选择完全取决于自己；uthash永远不会释放您的结构，

## 循环删除

```cpp
void delete_all() {
    struct my_struct *current_user, *tmp;
    HASH_ITER(hh, users, current_user, tmp) {
        HASH_DEL(users,current_user); /* delete; users advances to next */
        free(current_user); /* optional- if you want to free */
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 删除哈希表所有元素

删除的是一种 “ **引用** ”

```cpp
HASH_CLEAR(hh,users);
```



## 计算哈希表元素个数

HASH_COUNT可以直接进行调用

```cpp
unsigned int num_users;
num_users = HASH_COUNT(users);
printf("there are %u users\n", num_users);
```



## 排序哈希表

```cpp
HASH_SORT( users, name_sort );
```



HASH_SORT 函数第二个参数是指向比较函数的指针。它必须接受两个指针参数（要比较的项目），并且如果第一个项目分别在第二个项目的前面，等于或后面排序，则必须返回小于零，零或大于零的int。 （这与标准C库中的strcmp或qsort使用的约定相同）。

```cpp
int sort_function(void *a, void *b) {
  /* compare a to b (cast a and b appropriately)
   * return (int) -1 if (a < b)
   * return (int)  0 if (a == b)
   * return (int)  1 if (a > b)
   */
}
```



name_sort和id_sort的两个排序函数示例

```cpp
int name_sort(struct my_struct *a, struct my_struct *b) {
    return strcmp(a->name,b->name);
}
int id_sort(struct my_struct *a, struct my_struct *b) {
    return (a->id - b->id);
}
```



 对排序函数的封装，名字和id两者不同的排序

```cpp
void sort_by_name() {
    HASH_SORT(users, name_sort);
}

void sort_by_id() {
    HASH_SORT(users, id_sort);
}
```