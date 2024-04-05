---
layout: post
title:  "Writing org as source code"
date:   2024-04-04 21:00:000 -0300
categories: org emacs source code
---

For most of my projects and work as a programmer I was that guy that would write the source code following the same structure as I was tought in college, 
documentation accompanied by the source code itself. Sure, it made the project much bigger than it needed to be, and many times what was written was outdated or outwrite wrong.
But for most of our college years, that documentation saved us.

{% highlight cpp %}
class Model
{
  /*Read data from OBJ file*/
  void ReadModel(const std::string file_name);
  /*Read MTl file from OBJ file*/
  void ReadMaterial(const std::string file_name);
  /*Send data read from OBJ file to Graphics Card*/
  void SendModelData(void);
  void LoadTexture(const std::string file_name);
}
{% endhighlight %}

<p align=center>
<sup><sup>Example source code from OpenGL project from college</sup></sup>
</p>

Now, what if we could improve this coding style, not only making it better looking, but also better to read and smaller files overall?
And what if we could use our language of choice and keep using our favorite stack? And as icing on top keep all our projects available on the cloud so all our team members can see?
That my friend is no myth, is **org-mode** + **org files** + **emacs**.

# Project structure

For this example project, we will be making a small cli calculator for addition, subtraction, division and multiplication between only two numbres.
As the plan is to make it cross platform, I'll be using CMake and C++, but all languages for any programming stack apply to this method, up to what I know. So feel free to use and test what
you like and let me know how it was.

For this project, the structure will be as follows:

{% highlight bash %}
org-math/
├── bin
├── CMakeLists.org
└── src
    ├── main.org
    └── math.org
{% endhighlight %}

As we can see we replaced all of our file extensions from all of our files and used only org for this, from which we will take advantage of it to use emacs and org-mode. 
Let us begin with the source part, shall we?

# Header files

As most C++ projects found in nature, in here we will start by working on the header files of the project to make our math cli tool.
For this, in my math.org file I will start by including the important metadata for this file.

{% highlight cpp %}

#+TITLE: math.h
#+AUTHOR: dvrbs
#+DATE: 2024-04-01

#+OPTIONS: num:nil toc:nil timestamp:nil

#+PROPERTY: header-args :tangle math.h

{% endhighlight %}

In here, we can already have an idea of what this file is and what it will become in the future.
For a better idea, let us look at the options section.
In it, we can see we made some alteration to the export options of the file, where, to take advantage of the tools for exporting our files to the web, and make it readable,
we removed the number listing of the topics of the files, the table of contents at the begining of the files and the timestamp information. With this, we ensure our source code
is readable in most devices and allows for a more minimalistic look to it.

Next comes the methods we will use for our CLI tool.
To take advantage of org-modes syntax highlighting capabilities, let us add all of our methods to code blocks indicating that it is C++ code. It should look something like this.

{% highlight cpp %}
...

#+begin_src C++
  int Add(int a, int b) {
    return a + b;
  }
#+end_src

#+begin_src C++
  int Sub(int a, int b) {
    return a - b;
  }
#+end_src

#+begin_src C++
  int Mul(int a, int b) {
    return a * b;
  }
#+end_src

#+begin_src C++
  int Div(int a, int b) {
    return a / b;
  }
#+end_src

{% endhighlight %}

As we can see, all of the Functions are nice and ready to be called, but it is missing something. The documentation.
Before org, our documentation lived inside of our source code, cluthering our project and sometimes, not even doing nothing usefull, 
like commenting old code or some TODO topic that will never be done.

For us to add the documentation to the functions, my style is to have the title of the function accompanied by its parameters and than a description of what it does.
In the end we will have something like this.

{% highlight cpp %}
...

* Add
** int a, int b
Takes two integers and returns their sum

#+begin_src C++
  int Add(int a, int b) {
    return a + b;
  }
#+end_src

* Subtract
** int a, int b
Takes two integers and returns their subtraction

#+begin_src C++
  int Sub(int a, int b) {
    return a - b;
  }
#+end_src

* Multiplication
** int a, int b
Takes two integers and returns their multiplication

#+begin_src C++
  int Mul(int a, int b) {
    return a * b;
  }
#+end_src

* Division
** int a, int b
Takes two integers and returns their division

#+begin_src C++
  int Div(int a, int b) {
    return a / b;
  }
#+end_src
{% endhighlight %}

Now that we have the header file ready, let us take a look at the source cpp file.

# Source files

Similar to the header files before, first we add the metadata again but now we have to change the tangle output file to main.cc. On main.org we will have something like this.

{% highlight cpp %}

#+TITLE: main.cc
#+AUTHOR: dvrbs
#+DATE: 2024-04-01

#+OPTIONS: num:nil toc:nil timestamp:nil

#+PROPERTY: header-args :tangle main.cc

{% endhighlight %}

Next, we have to add the include files to main. For this, let us add another cpp code block, include the files and describe this part of the code as "Include package"

{% highlight cpp %}
...
* include packages

#+begin_src C++
  #include <iostream>
  #include "math.h"
#+end_src

{% endhighlight %}

And for a simple demonstration of the tool, we will add the final code block for the main function of the program. Once again we will describe the block but now as "main program"

{% highlight cpp %}
...
* main program

#+begin_src C++
  int main() {
    int a = 10;
    int b = 20;

    std::cout << Add(a,b) << std::endl;
    std::cout << Sub(a,b) << std::endl;
    std::cout << Mul(a,b) << std::endl;
    std::cout << Div(a,b) << std::endl;
  }
#+end_src

{% endhighlight %}

Will both source and header files ready, it is time for us to compile the program and test it in a console.

# Build

As we are making a cross platform program, we will open the CMakeList.org file and paste this to it.

{% highlight cmake %}
#+TITLE: CMakeLists.txt 
#+AUTHOR: dvrbs
#+DATE: 2024-04-01
#+OPTIONS: num:nil toc:nil timestamp:nil
#+PROPERTY: header-args :tangle CMakeLists.txt

* Basic project configuration

#+begin_src cmake
  cmake_minimum_required(VERSION 3.29)
  project(org-math)
  set(CMAKE_CXX_STANDARD 17)
#+end_src

* Source files and library configuration

#+begin_src cmake
  set(SOURCE_FILES src/main.cc)
#+end_src

* Project source configuration

#+begin_src cmake
  add_executable(simple_example ${SOURCE_FILES})
#+end_src
{% endhighlight %}

Next we have now to tangle all of the files we have created to their respective code files to be compiled.
The easiest way to do this is as follows:

- Open the file
- At the beginning of the file, type the command C-c C-c
- type the command M-x org-babel-tangle

If all files were created, now we should have something like this:

```txt
org-math/
├── bin
├── CMakeLists.org
├── CMakeLists.txt
└── src
    ├── main.cc
    ├── main.org
    ├── math.h
    └── math.org
```

If we now check the newly generated files, we will see that they are all normal and simple code files like any other, but now are clean and have no documentation on them.
To test if this work, let us compile it and run on our console. 

To compile a cmake project, firt in the root of our project in the console, we run the command: 

```bash
$ CMake -S . -B bin.
```

Next, we go to the bin directory and run the make command:

```bash
$ cd bin
$ make
```

Now run the org-math project we created and we are finished. We have made a project with nothing but org files, org mode and got easy to read documentation that can be read
as org files that will be fine, but we could also have it online on the cloud or on git repository services like gitlab and github.
Below we have an example of org files rendered as html documentation.

![My helpful screenshot](/assets/images/posts/org-math-html.png)
