---
layout: post
title: "Self Documenting Code"
date: 2013-05-03 00:20
comments: true
categories: 
---
Wikipedia says

"In computer programming, self-documenting (or self-describing) is a common descriptor for source code that follows certain loosely defined conventions for naming and structure. These conventions are intended to enable developers, users, and maintainers of a system to use it effectively without requiring previous knowledge of its specification, design, or behavior"

In other words, can you figure out in 5 seconds what is this code doing?

<!-- more -->

```
class House < ActiveRecord::Base
  has_many :mouse

  def mouse
    if (mouses.count > 0)
      mouse = nil
      mouses.each do |m|
        if m.cheese == 'Cheddar'
          mouse = m
          return mouse
        end
      end

      return mouse
    end

    return nil
  end
end
```

Let's stop for a while and look at the original code. What is method "mouse" suppose to do? Well, it's named "mouse", so it must have something to do with a mouse. That's good, although it's still not clear what kind of mouse it should return. Let's look at the body of the method. If there are mouses, then we make a variable called mouse and iterate over all mouses until we find the one with a cheddar cheese. Nice! So we'll return the mouse if we found it, otherwise we return nil. Oh, and it looks like we also return nil if there is no mouses in a house.

Is this better ?

```
def first_mouse_with_cheddar_cheese
  mouses.find_by_cheese 'Cheddar'
end
```

By just looking at the method name it's clear that it returns the first mouse with a cheddar cheese. The implementation of this method is also straightforward.

How do we get there?

Simple, by writing a test first and taking advantage of Test Driven **Design**.

```
describe House do
  describe "#mouse" do
    it "returns the first mouse with a cheddar cheese" do
      ...
    end
  end
end
```

By writing this simple rspec test before writing the actual code we can figure exactly what our code is suppose to do. Therefore we can come up with a better naming and implementation. In our example we can use ActiveRecord dynamic finders to implement our method in one line of code.

Writing tests first have multiple advantages. One of them is that they drive you towards a better design. You see that I left the implementation of the tests empty, because it does't matter as much as going throughout the mental exercise and writing in plain english what is your code suppose to do.

As a side benefit you also get the evidence that your code works the way you expected and it's self documented, readable, easy to reuse and maintain.
