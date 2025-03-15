[More C++ Idioms/Friendship and the Attorney-Client](https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms/Friendship_and_the_Attorney-Client)

```c++
class bar;
class attorney;

class client {
private:
    void a(int i) {}
    void b(float f) {}
    void c(double d) {}
    friend class attorney;
};
class attorney {
private:
    static void call_a(client& c, int i) { c.a(i); }
    static void call_b(client& c, float f) { c.b(f); }
    friend class bar;
};
class bar {
public:
    void run()
    {
        client c;
        attorney::call_a(c, 10);
        attorney::call_b(c, 5.5f);
    }
};
```

```c++
template<typename T>
struct connection {
    connection(const std::string& host, const int port)
        : connection_string{host + ":" + std::to_string(port)} {}
private:
    std::string connection_string;
    friend T;
};
struct executor {
    void run()
    {
        connection<executor> c{"localhost", 1234};
        
        std::cout << c.connection_string << '\n';
    }
};
```
