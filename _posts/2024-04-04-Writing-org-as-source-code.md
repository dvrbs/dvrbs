---
layout: post
title:  "Writing org as source code"
date:   2024-04-04 21:00:000 -0300
categories: org emacs source code
---

In my experience as a programmer, I have often adhered to a coding style learned during my college years, where source code was accompanied by comprehensive documentation. While this approach certainly ensured clarity and aided in understanding, it often resulted in bloated projects, with documentation that could become outdated or even incorrect over time. Nevertheless, during my academic journey, this documentation proved invaluable.

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

However, what if we could refine this coding style, enhancing its readability and reducing file sizes? Moreover, what if we could leverage our preferred programming languages and continue using our favorite development stack, while also ensuring accessibility for all team members through cloud-based hosting? This, my friends, is not a mere myth; it's the power of org-mode + org files + Emacs.

# Project structure

For this demonstration, let's embark on a modest command-line calculator project, capable of performing basic arithmetic operations on two numbers. To ensure cross-platform compatibility, I'll utilize CMake and C++, although this methodology is applicable to any programming language or stack. Feel free to experiment with different languages and provide feedback.

Here's the envisioned structure for our project:

{% highlight bash %}
org-math/
├── bin
├── CMakeLists.org
└── src
    ├── main.org
    └── math.org
{% endhighlight %}

As depicted, all file extensions have been replaced with .org, aligning with our intention to utilize Emacs and org-mode. Let's delve into the source section.

# Header files

As is customary in C++ projects, we begin by crafting the header files for our math CLI tool. Within my math.org file, I'll initialize the crucial metadata:

{% highlight cpp %}

#+TITLE: math.h
#+AUTHOR: dvrbs
#+DATE: 2024-04-01

#+OPTIONS: num:nil toc:nil timestamp:nil

#+PROPERTY: header-args :tangle math.h

{% endhighlight %}

This preamble provides a preview of the file's purpose and its evolution over time. Notably, I've fine-tuned the export options to optimize readability across various devices, omitting numbering, table of contents, and timestamps.

Next, let's define the methods for our CLI tool, encapsulating them within C++ code blocks to benefit from org-mode's syntax highlighting capabilities:

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

With each function accompanied by descriptive documentation, our header file is primed for use. Now, let's pivot to the source files.

# Source files

Similar to the header files, we initiate the metadata for our main.cc file within main.org, configuring the tangle output file accordingly:

{% highlight cpp %}

#+TITLE: main.cc
#+AUTHOR: dvrbs
#+DATE: 2024-04-01

#+OPTIONS: num:nil toc:nil timestamp:nil

#+PROPERTY: header-args :tangle main.cc

{% endhighlight %}

Subsequently, we incorporate the necessary include directives within a code block labeled "Include packages":

{% highlight cpp %}
...
* include packages

#+begin_src C++
  #include <iostream>
  #include "math.h"
#+end_src

{% endhighlight %}

Lastly, we encapsulate the main function within its own code block, aptly named "main program":

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

# Build

To facilitate cross-platform compilation, we configure the CMakeLists.org file accordingly:

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

Next we have now to tangle all of the files we have created to their respective code files to be compiled. The easiest way to do this is as follows:

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

To compile a cmake project, in the root of our project, we run the command: 

```bash
$ CMake -S . -B bin.
```

Next, we go to the bin directory and run the make command:

```bash
$ cd bin
$ make
```

Upon successful compilation, execute the org-math project. Voilà! We've created a project using solely org files and org-mode, ensuring easily readable documentation. Moreover, this documentation can be hosted online or within version control repositories such as GitLab or GitHub.

Here's a glimpse of org files rendered as HTML documentation:

![My helpful screenshot](/assets/images/posts/org-math-html.png)

# Conclusion

In conclusion, while this demonstration provides a glimpse into the potential of org files and org-mode for organizing code and documentation, it represents just a small example of their capabilities. There is significant room for improvement to transform org files into a full-fledged programming language suitable for literate programming. With further development, integration, and refinement, org-mode could become an invaluable tool for developers, particularly in languages like C, C++, and Python, offering enhanced readability, maintainability, and collaboration.
