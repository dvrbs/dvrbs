---
layout: post
title:  "How to write colored text to CLI programs"
date:   2024-04-27 16:55:52 -0300 
categories: cpp source code
---

In the past few days, while developing the logging system for my game
engine, I've found myself contemplating ways to enhance its
readability, not only for my testers but also for myself. Struggling
with poor vision, I've long grappled with the idiosyncratic error
handling of C and C++ compilers. This led me to ponder: how can I
improve this?

For many C++ developers, particularly those using g++ 5 or later,
there's a highly beneficial flag called `diagnostics-color`. Once
enabled, this flag colorizes error messages, facilitating easier
interpretation. While it doesn't rectify errors outright, it
significantly aids in error comprehension and, consequently,
resolution.

To activate this flag, it's a simple addition during compilation, as demonstrated below:

```sh
g++ -fdiagnostics-color=always -o my_program my_program.cpp
```

![Example output with the flag enabled](/assets/images/posts/color-text-gcc-example.png)

As straightforward as this fix may seem, it's not entirely optimal for
my needs. While effective for individual developers, the game engine
requires a solution that offers greater flexibility, less reliant on
compiler flags. Thus, I embarked on a journey to explore how color
could be integrated directly into terminal output.

Primarily, it's crucial to consider the capabilities of the terminal
being used. Virtual terminals, akin to emulators rather than physical
devices, vary in their color rendition. Similar to upgrading from an
old TV to a modern one, newer terminals often offer superior color
support. Notably, terminals like ST and early versions of xterm
necessitated patching to enable color text output. Therefore, it's
essential to verify whether your terminal supports color or merely
renders text in black and white.

Next, let's delve into color codes. Each operating system adheres to
its own standards for colorizing text output. In Linux, for instance,
color codes range from 0 (black) to 7 (white) in a linear
fashion. However, it's vital to recognize that enabling a color code
affects all subsequent text, akin to flicking a light switch for
standard output in C++ on Linux. To mitigate this, a reset call to the
standard output must follow the color application.

Below is a practical example demonstrating how to employ color codes
for text output in Linux using macros and incorporating them into the
`cout` method of the `std` namespace:

```c++
#include <iostream>

#define C_BLACK   "\033[1;30m"
#define C_RED     "\033[1;31m"
#define C_GREEN   "\033[1;32m"
#define C_YELLOW  "\033[1;33m"
#define C_BLUE    "\033[1;34m"
#define C_MAGENTA "\033[1;35m"
#define C_CYAN    "\033[1;36m"
#define C_WHITE   "\033[1;37m"
#define C_RESET   "\033[0m"

int main()
{
    std::cout << C_BLACK << "This text is black" << C_RESET << std::endl;
    std::cout << C_GREEN << "This text is green" << C_RESET << std::endl;
    std::cout << C_RED << "This text is red" << C_RESET << std::endl;
    std::cout << C_YELLOW << "This text is yellow" << C_RESET << std::endl;
    std::cout << C_BLUE << "This text is blue" << C_RESET << std::endl;
    std::cout << C_MAGENTA << "This text is magenta" << C_RESET << std::endl;
    std::cout << C_CYAN << "This text is cyan" << C_RESET << std::endl;
    std::cout << C_WHITE << "This text is white" << C_RESET << std::endl;

    return 0;
}
```

Regarding Windows systems, while there may exist methods within the
`windows.h` header for achieving colored output, I cannot definitively
confirm their efficacy. However, I've been advised by others to
explore this avenue further.

```c++
// C++ program to illustrate coloring 
#include <iostream>
#include <windows.h> 
using namespace std; 
  
// Driver Code 
int main() 
{ 
    // 0 for background Color(Black) 
    // A for text color(Green) 
    system("Color 0A"); 
  
    // Print any message 
    cout << "GREEN TEXT BLACK BACKGROUND"; 
    return 0;
} 
```

Given the suitability of color codes for simple text output to the
console, they seamlessly integrate into the logging library, becoming
an indispensable component of the engine. As for improvements, while
I've explored GCC flags and colored outputs, I remain open to further
suggestions. Continuous learning and enhancement are integral to the
coding journey.