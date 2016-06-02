# Lesson 1 - Maps

You will create and use two different type of map data structures

## 1.1 Maps

A map is a data structure that provides a key-value lookup table. They are
also sometimes known as Dictonaries (like in C#) In C++ the standard map
implementation is called map<>, and it works well in most scenarios.

```cpp
#include <iostream>
#include <map>
#include <vector>
using namespace std;

int main(int argc, char** argv) {

    map<int, int> iMap; // Provides an int-to-int lookup table
    iMap[5] = 4;
    cout << "iMap[5] is " << iMap[5] << endl;

    map<string, int> sMap; // Provides a string-to-int lookup table
    sMap["hello"] = 1;
    sMap["world"] = 2;
    cout << "sMap[\"hello\"] is " << sMap["hello"] << " and " <<
            "sMap[\"world\"] is " << sMap["world"] << endl;

    map<string, vector<int>> svMap; // A more complex structure,
    // where strings can point to list of numbers

    svMap["myList"] = { 1, 2, 3, 4 };
    svMap["yourList"] = { -1, -2, -3, -4 };

    cout << "svMap[\"myList\"] contains:" << endl;
    for (int i : svMap["myList"]) {
        cout << i << " ";
    }
    cout << endl;

    cout << "svMap[\"yourList\"] contains:" << endl;
    for (int i : svMap["yourList"]) {
        cout << i << " ";
    }
    cout << endl;

    return 0;
}
```

## 1.2 Hash Maps

A hash map is a specialized form of maps, optimized for speed. They use
hashed values of the keys to provide faster lookups. This works very
well in most cases, but sometimes has things called collisions. A
collision happens when two keys generate the same hash, making one or
both of them inaccessible. Collisions are very rare however and for most
use cases can be ignored.

In C++, the standard hash map implementation is called unordered_map<>. The
reason it is called this is because, unlike the standard map implementation,
hash maps cannot guarantee any kind of order. So the order you put things
in, is not necessarily the order you will pull them out.

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main(int argc, char** argv) {

    // For almost all use cases, you can use an unordered_map just like
    // a regular map. Try it with the use cases provided for map.
    unordered_map<string, int> usMap;
    usMap["the_answer"] = 42;

    return 0;
}
```

## 1.3 Looping through Maps

There are a couple ways to loop through maps, but unlike vectors and arrays
you cant use indexes. This is because you can't assume the keys are in any
order that you can determine.

Consider the following code for a vector:

```cpp
vector<int> myVec = { 0, 1, 2, 3 };
for (int i = 0; i < myVec.size(); ++i) {
    cout << myVec[i] << endl;
}
```

This works because we know the indexes into vec range from 0 to the size of
the vector, minus one. Now consider the following code for a map:

```cpp
// Warning: This code is a bad example, do not use
map<int, int> myMap = { {0, 42}, {1, 43}, {2, 44}, {3, 45} };
for (int i = 0; i < myMap.size(); ++i) {
    cout << myMap[i] << endl;
}
```

This works because the keys in map go 0, 1, 2, 3 which is not something we
can guarantee on every map. The solution to this is to use iterators.
Iterators are kind of like pointers to positions in the map. They also work
on all C++ containers like vector<>, list<>, etc.

Here is how you use them:

```cpp
map<int, int> myMap = { {1, 43}, {0, 42}, {3, 45}, {2, 44} };
// This is the "standard" way, and also the most wordy
for (map<int, int>::iterator it = myMap.begin(); it != myMap.end(); ++it) {
    cout << it->second << endl;
}

// You can make this a little more legible by using the auto keyword
for (auto it = myMap.begin(); it != myMap.end(); ++it) {
    cout << it->second << endl;
}

// And even more so by using C++11 style foreach loops
// You'll notice that this one used a . while the other two use ->, this is because
// auto tries to make your life easier
for (auto it : myMap) {
    cout << it.second << endl;
}
```

The last one is what I generally recommend to use.

Now these work great if you want to iterate forward over a collection, but
what about iterating backwards? You might be tempted to rewrite it as such:

```cpp
// Warning: This code is a bad example, do not use
for (auto it = myMap.end(); it != myMap.begin(); ++it) {
    cout << it->second << endl;
}
```

However this wont work, because .end() is a special case and does not actually
point to any element in the collection. The way to do this properly is like this:

```cpp
for (auto it = myMap.rbegin(); it != myMap.rend(); ++it) {
    cout << it->second << endl;
}
```

This makes use of another pair of begin/end functions prefixed with an "r" for
reverse. The full collection of begin/end functions is as follows

| Begin      | End      | Usage                       |
|------------|----------|-----------------------------|
| .begin()   | .end()   | Normal, Forward Iterating   |
| .rbegin()  | .rend()  | Normal, Reverse Iterating   |
| .cbegin()  | .cend()  | Constant, Forward Iterating |
| .crbegin() | .crend() | Constant, Reverse Iterating |

The ones listed as constant are just what you would think, they provide read-only
access to the collection. In most cases you don't need to care about that, but
in some you do.

## 1.3 Working with Maps

So far we've created and iterated over maps, but what about updating them? To add
an element to an existing map, you have a few options:

```cpp
map<int, int> myMap;

// Simplest, most clear solution. This will update an existing element if it
// exists, or create a new one if it does not
myMap[42] = -1;

// Most commonly found solution. Constructs a new pair and inserts it into
// the map
myMap.insert(pair<int, int>(42, -1));

// New way that allows for creating a pair and adding it at once. This is my
// favorite solution, as it doesn't work if the element already exists, which
// stops you from accidentally overwriting something in a map
myMap.emplace(42, -1);
```

To get an element from the map, you can do it one of two ways:

```cpp
cout << myMap[42] << endl;

// This is more useful when you have a pointer to a map
cout << myMap.at(42) << endl;
```

In order to check if an element exists, we need to get the iterator to it and
see if it is a valid one. This has the added bonus of getting us the iterator
so we can use it without having to do another lookup.

```cpp
auto it = myMap.find(42);

if (it != myMap.end()) {
    // The iterator is valid, and we can use it to get the value
    cout << it->second << endl;
}
```

Iterators for maps contain two pieces of information; the key and the value.
They follow the same style of the pair<> class like so:

```cpp
for (auto it : myMap) {
    cout << "Value at " << it.first << " is " << it.second << endl;
}
```

In order to remove an element from a map, you need to have the iterator pointing
to it.

```cpp
auto it = myMap.find(42);
myMap.erase(it);
```

This however, cannot be done from within a loop as it changes the collection as
you're iterating over it.

```cpp
// Warning: This code is a bad example, do not use
for (auto it : myMap) {
    myMap.erase(it); // will cause problems
}
```

If you need to remove all of the elements from within a collection, you can just
use the clear method like so:

```cpp
myMap.clear();
```

Finally, to check the size of a map or if it is empty, you can use the following:

```cpp
int size = myMap.size();

if (myMap.empty()) {
    // the map is empty
}
```
