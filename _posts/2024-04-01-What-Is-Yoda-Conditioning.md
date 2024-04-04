---
layout: post
title:  "What is yoda conditioning?"
date:   2024-04-01 10:46:43 -0300
categories: code typos conditioning
---

> “Control, control, you must learn control!” – Master Yoda

In "Star Wars: Episode V - The Empire Strikes Back," Master Yoda imparts this wisdom to Luke Skywalker during his training on Dagobah, underscoring the significance of mastering one's emotions and maintaining control, particularly in the utilization of the Force. This counsel, applicable not only to Force wielders, proves highly beneficial for programmers and their code.

Drawing inspiration from the iconic communication style of the fictional character, Yoda conditioning represents a programming approach dedicated to rectifying minor and inconvenient issues in everyday programming practices, such as typos. Let us witness it in action within real code. Can you identify the error in less than five seconds?

{% highlight cpp %}
#include <iostream>
#include <string>

enum PetType{
  DOG,
  CAT,
  BIRD,
  REPTILE
};

struct PetInfo{
  std::string name;
  float hunger;
  float thirst;
  PetType type;
};

bool PetIsHungry(PetInfo info){
  if (info.hunger = 0.5){
    return true;
  }
  return false;
}

int main() {	
  PetInfo petInfo;
  petInfo.name = "Puppy";
  petInfo.hunger = 0;
  petInfo.thirst = 0;
  petInfo.type = PetType::DOG;
	
  std::cout <<"Pet is hungry? : " << std::boolalpha << PetIsHungry(petInfo)<<std::endl;

  return 0;
}
{% endhighlight %}

Did you spot it? I presume you did. Nevertheless, just to ensure, here it is:

{% highlight cpp %}

bool PetIsHungry(PetInfo info){
  if (info.hunger = 0.5){
    return true;
  }
  return false;
}

{% endhighlight %}

As we have all likely experienced at some juncture, a typo within a conditional has been made. In this instance, the omission of the ">" symbol is evident. This oversight results in the program failing to execute a comparison, instead executing a true conditional, where we enter the conditional block with the hunger value now assigned as 0.5. While this is a minor and easily remedied issue, it may go unnoticed during compile time, necessitating manual inspection of the code for correction. While this example may seem straightforward, within larger projects involving teams, such typos can swiftly become problematic. So, how can this be mitigated? Enter Yoda conditional.

This approach posits that, as compile-time constants cannot be assigned values, structuring conditionals to check if constants are identical or adhere to a pattern with variables reduces the likelihood of a program compiling with such errors in the binary. Let's revisit the above example:

{% highlight cpp %}

bool PetIsHungry(PetInfo info){
  if (0.5 = info.hunger){
    return true;
  }
  return false;
}

{% endhighlight %}

Now, the compiler will prohibit compilation, indicating that the assignment of values must adhere to the operand left-to-right notation, i.e., Variable operand value. While this solution works, when should it be employed? This question arises because, as you may have observed, this style lacks readability and may impede collaborative efforts within a team, akin to Luke Skywalker's struggles in mastering the ways of the Force with his mentor.

In my estimation, it is prudent to utilize this notation as a tool for prototyping and testing software during the draft stages of production code. Yoda conditioning should never be deployed in production unless unanimously agreed upon by all team members. Once the software has undergone thorough testing and scrutiny, the final version should be rendered without this style, employing conventional conditioning instead.
