# name lookup 의 종속(dependent)

```c++
template<typename T>
    struct base {
        void func() 
        {
            std::cout << "base<T>::func\n";
        };
    };
    template<typename T>
    struct derived : base<T> {
        void call_func()
        {
            std::cout << "derived<T>::call_func\n";
            // 1
            func();        // error

            // 2
            base<T>::func();  // ok

            // 3
            this->func();     // ok
        }
    };
```

```c++
template<typename T>
struct base {
    using key_type = int;
};
template<typename T>
struct derived : base<T> {
    key_type k;  // error

    base<T>::key_type k = 0;  // error, but C++20, ok

    typename base<T>::key_type k = 0;  // ok
};
```

```c++
template<typename T>
struct base {
    template<typename U>
    void func()
    {
        std::cout << "base<T>::func<U>\n";
    }
};
template<typename T>
struct derived : base<T> {
    void call_func()
    {
        base<T>::func<int>();  // error
        
        typename base<T>::func<int>();  // ok
    }
};
```
