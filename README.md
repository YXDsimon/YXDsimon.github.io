# YXDsimon.github.io

[TOC]

## 1.Rvalue Reference and Move semantic
由于vector（大多数std容器）底层的数据都是在放在堆上，也就等于使用new来存储，右值引用这里是直接把存储的元素（例如vector是个数组）的首地址传给新vector，这样就完全没有复制，没有内存的开辟和释放。当然也可以直接用没有const的左值引用，但是
1. 这样你需要把原来容器的指针指向null，这违反了封装性。调用std::move()显式地将参数对象转为右值，即在不调用析构函数的情况下将其释放了，其容器中的元素则没有被释放，而是被转移权限给了新容器。

2. 这样不能传右值来构造函数了，可变左值形参不能接受右值作为参数。

Because most stl vessels(e.g. vector) put their elements on the heap, that is to say they use new to allocate memory. And move semantic(such as move constructor or push_back(&&)) passes the first address of parameter's elements to the target vessel. This completely avoids copy, and allocation/free of the memory. Technically, you can do this with left reference without const. However

1. This violates the RAII principle because after copying elements' address to another vessel, two vessels have accesses to the same memory. If one vessel's detructor is called, both vessel would lose their elements.  If you set the para's element pointer as null, this still violates the RAII.
1. You can't pass rvalue to this function because you don't have const.

### Occassions where std::move() is useful:

1. Push_back(vector), you want to insert an vessel into a vessel whose element type is the type of former vessel. And you don't need this single element after insertion(you access it by indexing the vessel instead).
2. You pass an rvalue to a function and want to initialize a new object using it as parameter. Because after passing into a function, an rvalue is transformed into lvalue, so you have to use move() to transform the parameter into rvalue first to avoid copy.

## LLVM

![image-20221119163234663](/Users/simon/Library/Application Support/typora-user-images/image-20221119163234663.png)

LLVM does the same thing that GCC does, it translates C/C++ code into machine code. However, LLVM is modular. It comprises 3 parts: frontend, middleend, and backend. So we can modify the frontend of it to make it able to compile our own languages, such as Apple's swift.

LLVM has a special virtual language called LLVM IR(LLVM Intermediate Representation). 

Also, you can't use GCC compiled programs to make money, cause all programs compiled by GCC is free. You can only use LLVM.

