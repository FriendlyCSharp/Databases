# FriendlyCSharp.Databases

A fast, generic, keyvalue, multi-dimensional Binary Search Tree written in C#. A library of cross platform C# data structures.

#### [FcsBTreeN&lt;TKey, TValue&gt;](FcsBTreeN.cs)
   + `Methods:` BtnCompares, BtnUpdates, BtnAdd, BtnDeleteAll, BtnFind, BtnFirst, BtnLast, BtnNext, BtnPrev, BtnSearch, BtnSearchPrev, BtnUpdate and BtnUsedKeys.
#### [FcsFastBTreeN&lt;TKey, TValue&gt;](FcsFastBTreeN.cs)
   + `Methods:` BtnCompares, BtnUpdates, BtnAdd, BtnDeleteAll, BtnFind, BtnFirst, BtnLast, BtnNext, BtnPrev, BtnSearch, BtnSearchPrev, BtnUpdate and BtnUsedKeys.
   + `Methods:` BtnFastFind, BtnFastFirst, BtnFastLast, BtnFastNext, BtnFastPrev, BtnFastSearch, BtnFastSearchPrev.
#### [FcsLockBTreeN&lt;TKey, TValue&gt;](FcsLockBTreeN.cs)
   + `Methods:` BtnCompares, BtnUpdates, BtnAdd, BtnDeleteAll, BtnFind, BtnFirst, BtnLast, BtnNext, BtnPrev, BtnSearch, BtnSearchPrev, BtnUpdate and BtnUsedKeys.
#### [FcsFastLockBTreeN&lt;TKey, TValue&gt;](FcsFastLockBTreeN.cs)
   + `Methods:` BtnCompares, BtnUpdates, BtnAdd, BtnDeleteAll, BtnFind, BtnFirst, BtnLast, BtnNext, BtnPrev, BtnSearch, BtnSearchPrev, BtnUpdate and BtnUsedKeys.
   + `Methods:` BtnFastFind, BtnFastFirst, BtnFastLast, BtnFastNext, BtnFastPrev, BtnFastSearch, BtnFastSearchPrev.

&nbsp;
## Performance
|  | sorted by&nbsp;key | duplicate keys | locking records |
| --- | :---: | :---: | :---: |
| [**fastDB&lt;...&gt;**](http://www.inmem.cz/inmem_letak.pdf) | **Yes** | **Yes** | **Yes** |
| **FcsBTreeN&lt;int, uint&gt;** | **Yes** | **Yes** | No |
| **FcsFastBTreeN&lt;int, uint&gt;** | **Yes** | **Yes** | No |	
| **FcsLockBTreeN&lt;int, uint&gt;** | **Yes** | **Yes** | No |
| **FcsFastLockBTreeN&lt;int, uint&gt;** | **Yes** | **Yes** | No |
| SortedSet&lt;KeyValuePair&lt;int, uint&gt;&gt; | **Yes** | No | No |
| HashSet&lt;KeyValuePair&lt;int, uint&gt;&gt; | No | No | No |
| Dictionary&lt;int, uint&gt; | No | No | No |

&nbsp;
## Benchmark 
The benchmark was configured as follows:
* CPU: Intel Xeon E3-1245 @ 3.3 GHz;
* Windows 10, x64, .NET Framework 4.5.1
### Adding in a single thread:
|  | sorted by&nbsp;key | iteration | total&nbsp;(ms) | one time (ns) | speed | RAM&nbsp;(MB) | occupied |
| --- | :---: | ---: | ---: | ---: | :---: | :---: | :---: |
| **FcsFastBTreeN&lt;...&gt;** | **Yes** | 10,000,000 | **6,185** | **619** | **100%** | **128** | **100%** |
| SortedSet&lt;...&gt; | **Yes** | 10,000,000 | ~~&nbsp;19,443&nbsp;~~ | ~~&nbsp;1,944&nbsp;~~ | ~~&nbsp;32%&nbsp;~~ | ~~&nbsp;458&nbsp;~~ | ~~&nbsp;358%&nbsp;~~ |
| HashSet&lt;...&gt; | No | 10,000,000 | 2,017 | 202 | 307% | 229 | 179% |
| Dictionary&lt;...&gt; | No | 10,000,000 | 1,378 | 138 | 449% | 229 | 179% |

### Foreach in a single thread:
|  | sorted by&nbsp;key | iteration | total&nbsp;(ms) | one time&nbsp;(ns) | speed |
| --- | :---: | ---: | ---: | ---: | :---: |
| [**fastDB&lt;...&gt;**](http://www.inmem.cz/inmem_letak.pdf) | **Yes** | 10,000,000 | **101** | **10.08** | **198%** |		
| **FcsFastBTreeN&lt;...&gt;** | **Yes** | 10,000,000 | **200** | **20** | **100%** |		
| SortedSet&lt;...&gt; | **Yes** | 10,000,000 | ~~&nbsp;1,230&nbsp;~~ | ~~&nbsp;123&nbsp;~~ | ~~&nbsp;16%&nbsp;~~ |
| HashSet&lt;...&gt; | No | 10,000,000 | 47.3 | 4,73 | 422%	|
| Dictionary&lt;...&gt; | No | 10,000,000 | 86.5 | 8,65 | 231% |		

&nbsp;
## Functions
>Various methods used throughout the library.

### Comparator
Some data structures require a comparator method to automatically keep their elements sorted upon insertion. 

>Default comparator is initialized as follows:

```cs
  protected virtual int BtnCompares(TKey keyX, TKey keyY, object objCmp)
  {
    return keyX.CompareTo(keyY);
  }
```

>Writing custom class with comparators is easy:

```cs
  public class MyBtnKeyValue : FcsBTreeN<int, uint>
  {
    protected override void BtnUpdates(int keyAdd, uint valueAdd, ref uint valueUpdates, object objUpdates)
    {
      valueUpdates++;
    }
    //////////////////////////
    protected override int BtnCompares(int keyX, int keyY, object objCmp)
    {
      return keyX - keyY;
    }
    //////////////////////////
    public MyBtnKeyValue() : base()
    {
    }
  }
```

### Iterators

All ordered containers have stateful iterators. Typically an iterator is obtained by _Iterator()_ function of an ordered container. Once obtained, iterator''s _Next()_ function moves the iterator to the next element and returns true if there was a next element. If there was an element, then element''s can be obtained by iterator''s _Value()_ function. Depending on the ordering type, it''s position can be obtained by iterator''s _Key()_ functions. Some containers even provide reversible iterators, essentially the same, but provide another extra _Prev()_ function that moves the iterator to the previous element and returns true if there was a previous element.Note: it is unsafe to remove elements from container while iterating.

#### Iterator

Tree gradually passes from the lowest, from the specified keys or higher.

>Typical usage:

```cs
  foreach(KeyValuePair<int, uint>? BtnKV in MyBtnKeyValue)
  {
  }
```

>Other usages:

```cs
  if (MyBtnKeyValue.BtnFirst(out btnKey, out btnValue) != null)
  {
    do
    {
    }
    while (MyBtnKeyValue.BtnNext(ref btnKey, out btnValue) != null)
  }
```

```cs
  if (MyBtnKeyValue.BtnFind(btnKey, out btnValue) != null)
  {
    do
    {
    }
    while (MyBtnKeyValue.BtnNext(ref btnKey, out btnValue) != null)
  }
```

```cs
  if (MyBtnKeyValue.BtnSearch(ref btnKey, out btnValue) != null)
  {
    do
    {
    }
    while (MyBtnKeyValue.BtnNext(ref btnKey, out btnValue) != null)
  }
```

#### Reverse Iterator

The tree passes successively from the last or entered or lower than the specified key.

>Typical usage of iteration in reverse:

```cs
  if (MyBtnKeyValue.BtnLast(out btnKey, out btnValue) != null)
  {
    do
    {
    }
    while (MyBtnKeyValue.BtnPrev(ref btnKey, out btnValue) != null)
  }
```

>Other usages:

```cs
  if (MyBtnKeyValue.BtnFind(btnKey, out btnValue) != null)
  {
    do
    {
    }
    while (MyBtnKeyValue.BtnPrev(ref btnKey, out btnValue) != null)
  }
```

```cs
  if (MyBtnKeyValue.BtnSearchPrev(btnKey, out btnValue) != null)
  {
    do
    {
    }
    while (MyBtnKeyValue.BtnPrev(ref btnKey, out btnValue) != null)
  }
```

### Enumerate

Methods that seek the desired key and returns the key value pair or null.

>**Find**

The method finds the specified key and returns the key value pair or null.

```cs
  public virtual  bool? BtnFind(TKey key, out TValue value)
  {
  }
  public virtual (TKey key, TValue value)? BtnFind(TKey key)
  {
  }
```

>**First**

The method finds the first key and returns the key value pair or null.

```cs
  public virtual bool? BtnFirst(out TKey key, out TValue value)
  {
  }
  protected (TKey key, TValue value)? _BtnFirst()
  {
  }
```

>**Last**

The method finds the last key and returns the key value pair or null.

```cs
  public virtual bool? BtnLast(out TKey key, out TValue value)
  {
  }
  public virtual (TKey key, TValue value)? BtnLast()
  {
  }
```

>**Search**

The method finds the specified key or the next higher and returns the key value pair or null.

```cs
  public virtual bool? BtnSearch(ref TKey key, out TValue value)
  {
  }
  public virtual (TKey key, TValue value)? BtnSearch(TKey key)
  {
  }
```

>**SearchPrev**

The method finds the specified key or the next lower and returns the key value pair or null.

```cs
  public virtual bool? BtnSearchPrev(ref TKey key, out TValue value)
  {
  }
  public virtual (TKey key, TValue value)? BtnSearchPrev(TKey key)
  {
  }
```

>**Example:**

```cs
```
　
## Install
Install via Nuget Package Manager

```
PM> Install-Package FriendlyCSherp.Databases
```

## LICENSE
See the [LICENSE](LICENSE).
