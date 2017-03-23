# namespace Friendly.Databases

A fast, generic, keyvalue, multi-dimensional Binary Search Tree written in C#
FcsBTreeN<TKey, TValue>, FcsFastBTree<TKey, TValue>, FcsLockBTreeN<TKey, TValue>, FcsFastLockBTree<TKey, TValue>

 * [FcsBTreeN<TKey, TValue>](FcsBTreeN.cs)
   + Methods: BtnCompares, BtnUpdates, BtnAdd, BtnDeleteAll, BtnFind, BtnFirst, BtnLast, BtnNext, BtnPrev, BtnSearch, BtnSearchPrev, BtnUpdate and BtnUsedKeys.

 * [FcsFastBTree<TKey, TValue>](FcsFastBTreeN.cs)
   + Methods: BtnCompares, BtnUpdates, BtnAdd, BtnDeleteAll, BtnFind, BtnFirst, BtnLast, BtnNext, BtnPrev, BtnSearch, BtnSearchPrev, BtnUpdate and BtnUsedKeys.
   + Methods: BtnFastFind, BtnFastFirst, BtnFastLast, BtnFastNext, BtnFastPrev, BtnFastSearch, BtnFastSearchPrev.

 * [FcsLockBTreeN<TKey, TValue>](FcsLockBTreeN.cs)
   + Methods: BtnCompares, BtnUpdates, BtnAdd, BtnDeleteAll, BtnFind, BtnFirst, BtnLast, BtnNext, BtnPrev, BtnSearch, BtnSearchPrev, BtnUpdate and BtnUsedKeys.

 * [FcsFastLockBTree<TKey, TValue>](FcsFastLockBTreeN.cs)
   + Methods: BtnCompares, BtnUpdates, BtnAdd, BtnDeleteAll, BtnFind, BtnFirst, BtnLast, BtnNext, BtnPrev, BtnSearch, BtnSearchPrev, BtnUpdate and BtnUsedKeys.
   + Methods: BtnFastFind, BtnFastFirst, BtnFastLast, BtnFastNext, BtnFastPrev, BtnFastSearch, BtnFastSearchPrev.


## Functions
Various helper functions used throughout the library.

### Comparator
Some data structures (e.g. TreeMap, TreeSet) require a comparator function to automatically keep their elements sorted upon insertion. This comparator is necessary during the initalization.

Comparator is defined as:

Return values (int):

Comparator signature:

```cs
  protected virtual int BtnCompares(TKey keyX, TKey keyY, object objCmp)
  {
    return keyX.CompareTo(keyY);
  }
```

Writing custom comparators is easy:

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

### Iterator

All ordered containers have stateful iterators. Typically an iterator is obtained by _Iterator()_ function of an ordered container. Once obtained, iterator''s _Next()_ function moves the iterator to the next element and returns true if there was a next element. If there was an element, then element''s can be obtained by iterator''s _Value()_ function. Depending on the ordering type, it''s position can be obtained by iterator''s _Key()_ functions. Some containers even provide reversible iterators, essentially the same, but provide another extra _Prev()_ function that moves the iterator to the previous element and returns true if there was a previous element.Note: it is unsafe to remove elements from container while iterating.

#### IteratorWithKey

The tree passes sequentially from the first or given to or higher than the specified key.

Typical usage:

```cs
  foreach(KeyValuePair<int, uint>? BtnKV in MyBtnKeyValue)
  {
  }
```

Other usages:

```cs
  if (MyBtnKeyValue.BtnFirst(out btnKey, out btnValue) == null)
    return;
  do
  {
  }
  while (MyBtnKeyValue.BtnNext(ref btnKey, out btnValue) != null)
```

```cs
  if (MyBtnKeyValue.BtnFind(btnKey, out btnValue) == null)
    return;
  do
  {
  }
  while (MyBtnKeyValue.BtnNext(ref btnKey, out btnValue) != null)
```

```cs
  if (MyBtnKeyValue.BtnSearch(ref btnKey, out btnValue) == null)
    return;
  do
  {
  }
  while (MyBtnKeyValue.BtnNext(ref btnKey, out btnValue) != null)
```

#### ReverseIteratorWithKey

The tree passes successively from the last or entered or lower than the specified key.

Typical usage of iteration in reverse:

```cs
  if (MyBtnKeyValue.BtnLast(out btnKey, out btnValue) == null)
    return;
  do
  {
  }
  while (MyBtnKeyValue.BtnPrev(ref btnKey, out btnValue) != null)
```

Other usages:

```cs
  if (MyBtnKeyValue.BtnFind(btnKey, out btnValue) == null)
    return;
  do
  {
  }
  while (MyBtnKeyValue.BtnPrev(ref btnKey, out btnValue) != null)
```

```cs
  if (MyBtnKeyValue.BtnSearchPrev(btnKey, out btnValue) == null)
    return;
  do
  {
  }
  while (MyBtnKeyValue.BtnPrev(ref btnKey, out btnValue) != null)
```

### Enumerable

Methods that seek the desired key and returns the key value pair.

**Find**

The method finds the specified key and returns the key value pair.

```cs
  public virtual  bool? BtnFind(TKey key, out TValue value)
  {
  }
  public virtual (TKey key, TValue value)? BtnFind(TKey key)
  {
  }
```

**First**

The method finds the first key and returns the key value pair.

```cs
  public virtual bool? BtnFirst(out TKey key, out TValue value)
  {
  }
  protected (TKey key, TValue value)? _BtnFirst()
  {
  }
```

**Last**

The method finds the last key and returns the key value pair.

```cs
  public virtual bool? BtnLast(out TKey key, out TValue value)
  {
  }
  public virtual (TKey key, TValue value)? BtnLast()
  {
  }
```

**Search**

The method finds the specified key or the next higher and returns the key value pair.

```cs
  public virtual bool? BtnSearch(ref TKey key, out TValue value)
  {
  }
  public virtual (TKey key, TValue value)? BtnSearch(TKey key)
  {
  }
```

**SearchPrev**

The method finds the specified key or the next lower and returns the key value pair.

```cs
  public virtual bool? BtnSearchPrev(ref TKey key, out TValue value)
  {
  }
  public virtual (TKey key, TValue value)? BtnSearchPrev(TKey key)
  {
  }
```

**Example:**

```cs
```
　
## Install
Install via Nuget Package Manager

```
PM> Install-Package FriendlyCSherp.Databases
```

## LICENSE
This project is licensed under the [MIT License](LICENSE).
