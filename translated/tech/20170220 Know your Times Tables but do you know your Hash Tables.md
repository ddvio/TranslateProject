
知道时间表，但是你是否知道“哈希表”
============================================================

探索哈希表的世界并理解潜在的技术是非常有趣的，并且将会受益匪浅。所以，让我们开始探索它吧。

哈希表是许多现代软件、应用程序中一种常见的数据结构。它提供了类似字典的功能，从而你能够执行插入、删除和删除其中的项等操作。这么说吧，比如我想找出“苹果”的定义是什么，并且我知道该定义被存储在了我定义的哈希表中。我将查询我的哈希表来得到定义。哈希表内的记录看起来可能像：`“苹果 ”=>" “一种拥有水果之王之称的绿色水果”`。所以，“苹果”是我的关键字，而“一种拥有水果之王之称的水果”是与之关联的值。

还有一个例子，我们很清楚，下面的是哈希表的内容：


```
1234
```

```
"面包" => "固体""水" => "液体""汤" => "液体""玉米片" => "固体"
```

我想知道*面包*是固体还是液体，所以我将查询哈希表来获取与之相关的值，该哈希表将返回“固体”给我。现在，我们大致了解了哈希表是如何工作的。使用哈希表需要注意的另一个重要概念是每一个关键字都是唯一的。如果到了明天，我拥有一个面包奶昔（它是液体），那么我们需要更新哈希表，把“固体”改为“液体”来反映哈希表的改变。所以，我们需要添加一条记录到字典中：关键字为“面包”，对应的值为“液体”。你能发现下面的表发生了什么变化吗？


```
1234
```

```
"面包" => "液体""水" => "液体""汤" => "液体""玉米片" => "固体"
```

没错，“面包”对应的值被更新为了“液体”。

**关键字是唯一的**，我的面包不能既是液体又是固体。但是，是什么使得该数据结构与其他数据结构相比如此特殊呢？为什么不使用一个[数组][1]来代替呢？它取决于问题的本质。对于某一个特定的问题，使用数组来描述可能会更好，因此，我们需要注意的关键点就是，**我们应该选择最适合问题的数据结构**。例如，如果你需要做的只是存储一个简单的杂货列表，那么使用数组会很适合。考虑下面的两个问题，两个问题的本质完全不同。

1.  我需要一个水果的列表
2.  我需要一个水果的列表以及各种水果的价格（每千克）

正如你在下面所看到的，用数组来存储水果的列表可能是更好的选择。但是，用哈希表来存储每一种水果的价格看起来是更好的选择。


```
123456789
```

```
//示例数组
["apple, "orange", "pear", "grape"]   
//示例哈希表  
{ "apple" : 3.05,     "orange" : 5.5,     "pear" : 8.4,     "grape" : 12.4  }
```

实际上，有许多的机会需要[使用][2]哈希表。

### 时间以及它对你的意义

[对时间复杂度和空间复杂度的一个复习][3].

平均情况下，在哈希表中进行搜索、插入和删除记录的时间复杂度均为 O(1) 。实际上，O(1) 读作“大 O 1”，表示常数时间。这意味着执行每一种操作的运行时间不依赖于数据集中数据的数量。我可以保证，查找、插入和删除项目均只花费常数时间，“当且仅当”哈希表的执行正确。如果执行不正确，可能需要花费很慢的 O(n) 时间，尤其是当所有的数据都映射到了哈希表中的同一位置/点。

### 构建一个好的哈希表

到目前为止，我们已经知道如何使用哈希表了，但是如果我们想**构建**一个哈希表呢？本质上我们需要做的就是把一个字符串（比如 "dog"）映射到一个哈希代码（一个生成数），即映射到一个数组的索引。你可能会问，为什么不直接使用索引呢？为什么没必要呢？通过这种方式我们可以直接查询 "dog" 立即得到 "dog" 所在的位置，`String name = Array["dog"] //name is Lassy`。但是，使用索引查询名称时，可能出现的情况是我们不知道名称所在的索引。比如，`String name = Array[10] // name is now "Bob"`- 现在不是我的狗的名字。这就是把一个字符串映射到一个哈希代码的益处（它和一个数组的索引相对应）。我们可以通过使用模运算符和哈希表的大小来计算出数组的索引：`index = hash_code % table_size`。 

我们需要避免的另一种情况是两个关键字映射到同一个索引，这叫做**哈希碰撞**，如果选取的哈希函数不合适，这很容易发生。实际上，每一个输入比输出多的哈希函数都有可能发生碰撞。通过下面的同一个函数的两个输出来展示一个简单的碰撞：

`int cat_idx = hashCode("cat") % table_size; //cat_idx 现在等于 1`

`int dog_idx = hashCode("dog") % table_size; //dog_idx 也等于 1`

我们可以看到，现在两个数组的索引均是 1 。这样将会出现两个值相互覆盖，因为它们被写到了相同的索引中。如果我们查找 "cat" 的值，将会返回 "Lassy" ，但是这并不是我们想要的。有许多可以[解决哈希碰撞][4]的方法，但是更受欢迎的一种方法叫做**链接**。链接的想法就是对于数组的每一个索引位置都有一个链表，如果碰撞发生，值就被存到链表中。因此，在前面的例子中，我们将会得到我们需要的值，但是我们需要搜索数组中索引为 1 的位置上的链表。伴有链接的哈希实现需要 O(1 + α) 时间，其中 α 是装载因子，它可以表示为 n/k，其中 n 是哈希表中的记录数目，k 是哈希表中可用位置的数目。但是请记住，只有当你给出的关键字非常随机时，这一结论才正确（依赖于 [SUHA][5]）。

这是做了一个很大的假设，因为总是有可能任何不相等的关键字都不散列到同一点。这一问题的一个解决方法是去除哈希表中关键字对随机性的依赖，转而把随机性集中于关键字是如何被散列的，从而减少矛盾发生的可能性。这被称为……

### 通用散列

这个观念很简单，从通用散列家族集合随机选择一个哈希函数来计算哈希代码。用起来话来说，就是选择任何一个随机的哈希函数来散列关键字。通过这种方法，将有很低的可能性散列两个不同的关键字结果不相同。我只是简单的提一下，如果不相信我那么请相信[数学][6]。实现这一方法时需要注意的另一件事是选择了一个不好的通用散列家族。它会把时间和空间复杂度拖到 O(U)，其中 U 是散列家族的大小。而其中的挑战就是找到一个不需要太多时间来计算，也不需要太多空间来存储的哈希家族。

### 上帝哈希函数

追求完美是人的天性。我们是否能够构建一个*完美的哈希函数*，从而能够把关键字映射到整数集中，并且几乎没有碰撞。好消息是我们能够在一定程度上做到，但是我们的数据必须是静态的（这意味着在一定时间内没有插入/删除/更新）。一个实现完美哈希函数的方法就是使用 2-级哈希，它基本上是我们前面讨论过的两种方法的组合。它使用*通用散列*来选择使用哪个哈希函数，然后通过链接组合起来，但是这次不是使用链表数据结构，而是使用另一个哈希表。让我们看一看下面它是怎么实现的： [![2-Level Hashing](http://www.zeroequalsfalse.press/2017/02/20/hashtables/Diagram.png "2-Level Hashing")][8] 

**但是这是如何工作的以及我们如何能够确保无需关心碰撞？**

它的工作方式与[生日悖论][7]相反。它指出，在随机选择的一堆人中，会有一些人生日相同。但是如果一年中的天数远远大于人数（平方以上），那么有极大的可能性所有人的生日都不相同。所以这二者是如何相关的？对于每一个链接哈希表，其大小均为第一级哈希表大小的平方。那就是说，如果有两个元素被散列到同一个点，那么链接哈希表的大小将为 4 。大多数时候，链接哈希表将会非常稀疏/空。

重复下面两个来确保无需担心碰撞：

*   从通用散列家族中选择一个哈希函数来计算
*   如果发生碰撞，那么继续从通用散列家族中选择另一个哈希函数来计算

字面上看就是这样（这是一个 O(n^2) 空间的解）。如果需要考虑空间问题，那么显然需要另一个不同的方法。但是值得庆幸的是，该过程平均只需要进行**两次。**

### 总结

只有具有一个好的哈希函数才能算得上是一个好的哈希表。在同时保证功能实现、时间和空间的提前下构建一个完美的哈希函数是一件很困难的事。我推荐你在解决问题的时候首先考虑哈希表，因为它能够为你提供巨大的性能优势，而且它能够对应用程序的可用性产生显著差异。哈希表和完美的哈希函数常被用于实时编程应用中，并且在各种算法中都得到了广泛应用。你见或者不见，哈希表就在这儿。

--------------------------------------------------------------------------------

via: http://www.zeroequalsfalse.press/2017/02/20/hashtables/

作者：[Marty Jacobs][a]
译者：[ucasFL](https://github.com/ucasFL)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]:http://www.zeroequalsfalse.press/about
[1]:https://en.wikipedia.org/wiki/Array_data_type
[2]:https://en.wikipedia.org/wiki/Hash_table#Uses
[3]:https://www.hackerearth.com/practice/basic-programming/complexity-analysis/time-and-space-complexity/tutorial/
[4]:https://en.wikipedia.org/wiki/Hash_table#Collision_resolution
[5]:https://en.wikipedia.org/wiki/SUHA_(computer_science
[6]:https://en.wikipedia.org/wiki/Universal_hashing#Mathematical_guarantees
[7]:https://en.wikipedia.org/wiki/Birthday_problem
[8]:http://www.zeroequalsfalse.press/2017/02/20/hashtables/Diagram.png