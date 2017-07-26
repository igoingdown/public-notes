## 数值类型转到string类型

使用 ==`to_string()`== 函数，有多种重载方式，可以基本可以覆盖所有数值类型。
```
string to_string (int val);
string to_string (long val);
string to_string (long long val);
string to_string (unsigned val);
string to_string (unsigned long val);
string to_string (unsigned long long val);
string to_string (float val);
string to_string (double val);
string to_string (long double val);
```
---

## C风格字符串转到数值类型：

使用`atoi()`，`atof()`, `atol()`, 和`atoll()`等，意思是将ASCII码转换为int,float,long或者long long等类型。
函数原型如下：
```
int atoi (const char * str);
double atof (const char* str);
long int atol (const char * str );
long long int atoll (const char * str);
```
## string类型转到数值类型

```cpp
double stod (const string&  str, size_t* idx = 0);
float stof (const string&  str, size_t* idx = 0);
int stoi (const string&  str, size_t* idx = 0, int base = 10);
long stol (const string&  str, size_t* idx = 0, int base = 10);
long double stold (const string&  str, size_t* idx = 0);
long long stoll (const string&  str, size_t* idx = 0, int base = 10);
unsigned long stoul (const string&  str, size_t* idx = 0, int base = 10);
unsigned long long stoull (const string&  str, size_t* idx = 0, int base = 10);
```
这套函数和`to_string()`函数体系是对应的 ！

string类型可以先用`str.c_str()`将string对象转到C风格字符串再将C风格字符串转到数值类型，但是显然使用专门的将string类对象转到各种数值类型的函数更符号C++程序员的逼格！