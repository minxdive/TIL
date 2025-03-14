# decltype 의 함수 반환 type

함수에서 `auto` 와 `decltype` 은 자동으로 `type` 을 추론하여 반환하는 방법으로 사용할 수 있다.

다음의 두 함수를 살펴보자.

```c++
template<typename T, typename U>
auto min(T&& a, U&& b)
{
    return a < b ? a : b;
}

template<typename T, typename... Args>
decltype(auto) add(T&& a, Args&&... args)
{
    return (a + ... + args);
}

int main()
{
    int a = 10;
    double b = 5;

    std::cout << "min(a,b) = " << min(a, b) << '\n';  // min(a,b) = 5.5
    std::cout << "add(a,b) = " << add(a, b) << '\n';  // add(a,b) = 15.5

    return 0;
}
```

`auto` 와 `decltype` 모두 자동으로 `type` 을 추론하여 반환하였다.

또 다른 예를 살펴보자.

```c++
template<typename T>
const T& func(const T& ref)
{
    return ref;
}

template<typename T>
auto call_func(T&& ref)
{
    return func(std::forward<T>(ref));
}

template<typename T>
decltype(auto) call_func_2(T&& ref)
{
    return func(std::forward<T>(ref));
}

int main()
{
    int a = 10;
    decltype(func(a)) e1 = a;         // const int&
    decltype(call_func(a)) e2;        // int
    decltype(call_func_2(a)) e3 = a;  // const int&

    return 0;
}
```

`e2` 는 `int` 이고 `e3` 는 `const int&` 이다. 여기서 우리는 `decltype(auto)` 만이 `perfect forwarding` 이 된 것을 알 수있다.

`e2` 가 `int` 이므로 초기화를 하지 않아도 컴파일에 문제가 없었다.

반면 `e3` 는 `const int&` 이므로 초기화 값을 요구한다.

이처럼 `auto` 와 `decltype(auto)` 가 경우에 따라서 반환 `type` 이 다르다는 것을 알았다.
