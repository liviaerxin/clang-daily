# Use Clang clangTooling to check sytanx


[Tutorial for building tools using LibTooling and LibASTMatchers — Clang 18.0.0git documentation](https://clang.llvm.org/docs/LibASTMatchersTutorial.html)

```sh
cmake -B build -S .
```

```sh
cmake --build build
```

```sh
$ ./build/loop-convert test.cpp -- 

$ ./build/loop-convert test1.cpp --
/root/Documents/clang-daily/test1.cpp:1:24: error: use of undeclared identifier 'xx'
int main() { return 0; xx}
                       ^
1 error generated.
Error while processing /root/Documents/clang-daily/test1.cpp.
```