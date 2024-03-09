# python 数据结构与算法 29-3 用哈希表实现映射

## 实现 map 抽象数据类型

字曲是 python 里最有用的数据集合之一，回想一下，字典是一对键值-数据的组合，键值是用来查找相应的数据，我们把这种思想称为“映射”

映射的抽象数据类型定义如下，这是一个无序的键-值对集合，键值总是唯一的以便建立与数据的对应关系。映射的操作方法如下：

·            Map() 创建一个新的空的映射，返回一个空集合。

·            put(key,val)在映射中增加一对新的键-值对，如果键值已经存在，用新的数据值代替原来的。

·            get(key) 根据给定的键值，返回对应的数值，找不到时返回 None。

·            Del 从映射中删除键-值对，形式 del map[key]

·            len()返回映射中键-值对的数量

·            In 如果 key 值在映射中，key in map 返回 True，否则返回 False

字典的好处之一，是给一个键值可以很快返回相关的数据，为了提供这样快速查询的能力，我们需要引入一个高效的查找功能。可以对列表使用顺序或二分查找，但是最好是用哈希表，因为它能提供接近*O*(1)的性能

在 listing2 中，我们用两个列表创建 HashTable 类来实现映射的抽象数据类型。一个列表，名为 slots 来保存键值元素，另一个平行的列表，名为 data，保存数据。当查询键值的时候，data 列表相应的位置上就保存有数据。我们将键值列表处理成哈希表，注意哈希表的初始大小为是 11，虽然这个大小是随意的，但是选择为质数特别重要，因为这样一来后面处理冲突的效率就比较高。

**Listing 2**

```
classHashTable:
```

```
    def__init__(self):
```

```
        self.size=11
```

```
        self.slots= [None]*self.size
```

```
        self.data= [None]*self.size
```

Hashfunction 函数是简单地用了余数法，冲突解决采用了+1 线性探测再哈希函数，put 函数约定：除非 self.slots 中包括这个键值，否则这个槽位就认为是空的。它计算出的哈希值如果非空，就迭代 rehash 函数，直到找到一个空槽位。如果一个非空的槽位上已经有键值，就用新数据代替原数据。

**Listing 3**

```
defput(self,key,data):
```

```
  hashvalue =self.hashfunction(key,len(self.slots))
```

```
  ifself.slots[hashvalue]==None:
```

```
    self.slots[hashvalue]= key
```

```
    self.data[hashvalue]= data
```

```
  else:
```

```
    ifself.slots[hashvalue]== key:
```

```
      self.data[hashvalue]= data  *#replace*
```

```
    else:
```

```
      nextslot =self.rehash(hashvalue,len(self.slots))
```

```
      whileself.slots[nextslot]!=Noneand \
```

```
                      self.slots[nextslot]!= key:
```

```
        nextslot =self.rehash(nextslot,len(self.slots))
```

```
      ifself.slots[nextslot]==None:
```

```
        self.slots[nextslot]=key
```

```
        self.data[nextslot]=data
```

```
      else:
```

```
        self.data[nextslot]= data *#replace*
```

```
defhashfunction(self,key,size):
```

```
     return key%size
```

```
defrehash(self,oldhash,size):
```

```
    return (oldhash+1)%size
```

Listing4 中，get 函数从计算哈希值开始，如果这个值不是一个起始的槽位，rehash 就去查找另一个可能的位置。注意第 15 行检查有没有返回最早的槽位，如果是，查找将停止，因为那表明已经找过所有的槽位，元素不存在。

HashTable 类的最后一个方法提供了一个附加的字典函数。我们重载了 __getitem__ 和 __setitem__ 方法来实现“[ ]”符号的使用。这也意味着，一旦 HashTable 对象创建，熟悉的索引方法就可用了。其他方法用作练习。

```
defget(self,key):
```

```
  startslot =self.hashfunction(key,len(self.slots))
```

```
  data =None
```

```
  stop =False
```

```
  found =False
```

```
  position = startslot
```

```
  whileself.slots[position]!=Noneand  \
```

```
                       not found andnot stop:
```

```
     ifself.slots[position]== key:
```

```
       found =True
```

```
       data =self.data[position]
```

```
     else:
```

```
       position=self.rehash(position,len(self.slots))
```

```
       if position == startslot:
```

```
           stop =True
```

```
  return data
```

```
def__getitem__(self,key):
```

```
    returnself.get(key)
```

```
def__setitem__(self,key,data):
```

    self.put(key,data)

对下会话显示了 HashTable 类的使用，先创建一个哈希表，存入一些数据。

```
>>> H=HashTable()
```

```
>>> H[54]="cat"
```

```
>>> H[26]="dog"
```

```
>>> H[93]="lion"
```

```
>>> H[17]="tiger"
```

```
>>> H[77]="bird"
```

```
>>> H[31]="cow"
```

```
>>> H[44]="goat"
```

```
>>> H[55]="pig"
```

```
>>> H[20]="chicken"
```

```
>>> H.slots
```

```
[77, 44, 55, 20, 26, 93, 17, None, None, 31, 54]
```

```
>>> H.data
```

```
['bird', 'goat', 'pig', 'chicken', 'dog', 'lion',
```

```
       'tiger', None, None, 'cow', 'cat']
```

然后访问并修改一些元素，注意键值为 20 的数据被替换。

```
>>> H[20]
```

```
'chicken'
```

```
>>> H[17]
```

```
'tiger'
```

```
>>> H[20]='duck'
```

```
>>> H[20]
```

```
'duck'
```

```
>>> H.data
```

```
['bird', 'goat', 'pig', 'duck', 'dog', 'lion',
```

```
       'tiger', None, None, 'cow', 'cat']
```

```
>> print(H[99])
```

```
None
```

完全的哈希表例子代码：

```

```
class HashTable:
    def __init__(self):
        self.size = 11
        self.slots = [None] * self.size
        self.data = [None] * self.size

    def put(self,key,data):
      hashvalue = self.hashfunction(key,len(self.slots))

      if self.slots[hashvalue] == None:
        self.slots[hashvalue] = key
        self.data[hashvalue] = data
      else:
        if self.slots[hashvalue] == key:
          self.data[hashvalue] = data  #replace
        else:
          nextslot = self.rehash(hashvalue,len(self.slots))
          while self.slots[nextslot] != None and \
                          self.slots[nextslot] != key:
            nextslot = self.rehash(nextslot,len(self.slots))

          if self.slots[nextslot] == None:
            self.slots[nextslot]=key
            self.data[nextslot]=data
          else:
            self.data[nextslot] = data #replace

    def hashfunction(self,key,size):
         return key%size

    def rehash(self,oldhash,size):
        return (oldhash+1)%size

    def get(self,key):
      startslot = self.hashfunction(key,len(self.slots))

      data = None
      stop = False
      found = False
      position = startslot
      while self.slots[position] != None and  \
                           not found and not stop:
         if self.slots[position] == key:
           found = True
           data = self.data[position]
         else:
           position=self.rehash(position,len(self.slots))
           if position == startslot:
               stop = True
      return data

    def __getitem__(self,key):
        return self.get(key)

    def __setitem__(self,key,data):
        self.put(key,data)

H=HashTable()
H[54]="cat"
H[26]="dog"
H[93]="lion"
H[17]="tiger"
H[77]="bird"
H[31]="cow"
H[44]="goat"
H[55]="pig"
H[20]="chicken"
print(H.slots)
print(H.data)

print(H[20])

print(H[17])
H[20]='duck'
print(H[20])
print(H[99])

```

```