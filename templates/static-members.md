# Static members and related shit 

## Initializing static member within template

```c++
#include <list>

struct item {};

template<typename T> struct holder {
    static std::list<item> items;
};

template<typename T> std::list<item> holder<T>::items;
```
