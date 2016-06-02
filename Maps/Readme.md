# Lesson 1 - Maps

You will create and use two different type of map data structures

## Maps

A map is a data structure that provides a key-value lookup table. They are
also sometimes known as Dictonaries (like in C#) In C++ the standard map 
implementation is called map works for most things.

    #include <iostream>
    #include <map>
    using namespace std;
    
    int main(int argc, char** argv) {

        map<int, int> iMap; // Provides an int-to-int lookup table
        iMap[5] = 4;
        cout << "iMap[5] is " << iMap[5] << endl;

        map<string, int> sMap; // Provides a string-to-int lookup table
        sMap["hello"] = 1;
        sMap["world"] = 2;
        cout << "sMap[\"hello\"] is " << sMap["hello"] <<
                "sMap[\"world\"] is " << sMap["world"] << endl;

        map<string, vector<int>> svMap; // A more complex structure,
        // where strings can point to list of numbers

        svMap["myList"] = { 1, 2, 3, 4 };
        svMap["yourList"] = { -1, -2, -3, -4 };

        cout << "svMap[\"myList\"] contains:" << endl
        for (int i : svMap["myList"]) {
            cout << i << " ";
        }
        cout << endl;

        cout << "svMap[\"yourList\"] contains:" << endl
        for (int i : svMap["yourList"]) {
            cout << i << " ";
        }
        cout << endl;

        return 0;
    }

# Hash Maps

A hash map is a specialized form of maps, optimized for speed. They use 
hashed values of the keys to provide faster lookups. This works very 
well in most cases, but sometimes has things called collisions. A 
collision happens when two keys generate the same hash, making one or 
both of them inaccessible. Collisions are very rare however and for most
use cases can be ignored. 

In C++, the standard hash map implementation is called unordered_map. The
reason it is called this is because, unlike the standard map implementation,
hash maps cannot guarantee any kind of order. So the order you put things
in, is not necessarily the order you will pull them out.

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
