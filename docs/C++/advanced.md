# Advanced

## any

since C++17

- any_cast
- has_value
- reset (deconstruct)
- emplace (reset + copy construct)
- type
- `type().name()`

```cpp
#include <iostream>
#include <cxxabi.h>
#include <any>
using namespace std;

template <class T>
class MyObj
{
    T data = 1;
public:
    ~MyObj() { cout << "Deleting MyObj" << endl; }
};

any f(const int which_one) {
    if (which_one == 0)
        return 12345;
    else if (which_one == 1)
        return "Hello!";
    else
        return MyObj<double>();
}

auto name_demangle(const char* s) -> char* {
    int status = 0;
    return __cxxabiv1::__cxa_demangle(s, nullptr, nullptr, &status);
}

void print_info(const char* var_name, const any& v) {
    cout << var_name << " has type: "
            "RunTime Type Info: "<< v.type().name() <<
            " ; Demangled type name: " << name_demangle(v.type().name()) << endl;
}

int main()
{
    auto res_num = f(0);
    auto res_str = f(1);
    auto res_obj = f(2);
    if (res_num.has_value())
        print_info("res_num", res_num);
    if (res_str.has_value())
        print_info("res_str", res_str);
    if (res_obj.has_value())
        print_info("res_obj", res_obj);
    try {
        double num = any_cast<int>(res_num);
        cout << "Casting to int succeeds!" << endl;
        string s = any_cast<string>(res_num);
    }
    catch (const bad_any_cast &e) {
        cout << "Casting to string fails: " << e.what() << endl;
    }
    res_obj.reset();
    cout << "A MyObj is deleted!" << endl;
    res_num.emplace<MyObj<int>>(MyObj<int>());
    cout << "res_num now holds a MyObj!" << endl;
    print_info("res_num", res_num);
    return 0;
}
```

