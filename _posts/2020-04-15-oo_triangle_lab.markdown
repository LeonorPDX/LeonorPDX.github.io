---
layout: post
title:      "OO Triangle Lab"
date:       2020-04-15 19:39:44 -0400
permalink:  oo_triangle_lab
---


As promised, here's my solution to the OO Triangle Lab in Flatiron's Software Engineering program. I'm excited to share this code because as I mentioned in my previous blog post, my solution is different from the "official" solution. First I'll walk through my solution, then I'll contrast it to Flatiron's solution.

The lab is fairly simple: write a `Triangle` class that accepts three arguments to indicate the length of each side of the triangle, and create a `kind` instance method that returns the type of triangle as a symbol:
* `:equilateral`
* `:isosceles`
* `:scalene`

Additionally, the `kind` method should raise a custom error if the triangle is invalid (i.e., the sides entered as arguments cannot make a triangle).

Right... so I totally remember all those things about triangles.

![](https://i.imgur.com/skTi9da.png)

JK, totally don't remember stuff about triangles, but Google makes everything better. So a quick search got me the info I needed:
* The sum of the lengths of any two sides of a triangle always exceeds the length of the third side. This is a principle known as the *triangle inequality.* After Googling for this, I actually saw it was listed as a hint at the end of a lab. But it's good to do your own research, right?
* All sides of an *equilateral triangle* are equal in length.
* Exactly two sides of an *isosceles triangle* are equal length.
* No sides of a *scalene triangle* are equal in length.

Seemed pretty clear that we would be comparing the lengths of the sides of the triangle. First, I thought about validating the triangle and raising an error if the triangle is invalid. I saw we don't need to compare each side against the other two, we really just need to know which side is longest and compare that side to the sum of the remaining two sides. But how to get the longest side? Easy, make an array and sort it. So my initialize immediately put all the arguments into an array and didn't mess around with creating an instance variable for each one, just one `@sides` for the array:

```
class Triangle
  attr_accessor :sides
  
  @sides = []
  
  def initialize (side1, side2, side3)
    @sides = [side1, side2, side3]
    @sides.sort!
  end
end
```

I also sort the array so I would have the sides ordered by shortest to longest. The lab required that the `kind` method raise a custom `TriangleError` if the triangle is invalid, so the first step was to create a custom error that inherited from the `StandardError` class, defined inside the `Triangle` class.

```
  class TriangleError < StandardError
    
  end
```

This error wasn't required to provide any custom message or rescue, so there was no need to write any code inside the `TriangleError` class.

In the `kind` method, I checked to see if the triangle was valid with the following code:

```
if @sides.any?{ |side|  side <= 0 } || ( (@sides[0] + @sides[1]) <= @sides[2] )
      raise TriangleError
```

Here I am checking to see if:
* Any of the sides in the array are equal to or less than zero, *OR*
* The sum of the first and second sides in the array (which I knew to be the shortest, thanks to sorting the array in my initialize method) is equal to or less than the length of the third side. 

If either of the above elements in the block have a truthy value, the `TriangleError` is raised. The rest of the `kind` method uses the unique length of the `@sides` array to determine if a triangle is equilateral, isosceles, or scalene:

```
 def kind
    if @sides.any?{|side| side <= 0} || ((@sides[0] + @sides[1]) <= @sides[2])
      raise TriangleError
    elsif @sides.uniq.length == 1
      :equilateral
    elsif @sides.uniq.length == 2
      :isosceles
    else
      :scalene
    end
 end
```

If the length of unique elements in the array is 1, I know all sides are exactly equal. Similarly if the length of unique elements in the array is exactly 2, then the triangle is isosceles. Any other valid triangle is scalene. So there it is, a tidy and simple way to check for valid triangles and the type of triangle without messy, repetitive code that compares every side against the other two in turn!

Putting it all together, it looks like this:
```
class Triangle
  attr_accessor :sides
  
  @sides = []
  
  def initialize (side1, side2, side3)
    @sides = [side1, side2, side3]
    @sides.sort!
  end
  
  def kind
    if @sides.any?{|side| side <= 0} || ((@sides[0] + @sides[1]) <= @sides[2])
      raise TriangleError
    elsif @sides.uniq.length == 1
      :equilateral
    elsif @sides.uniq.length == 2
      :isosceles
    else
      :scalene
    end
  end
  
  class TriangleError < StandardError
    
  end
end
```

In contrast, Flatiron's official solution looks like this:
```
class Triangle
  attr_reader :a, :b, :c
  def initialize(a, b, c)
    @a = a
    @b = b
    @c = c
  end

  def kind
    validate_triangle
    if a == b && b == c
      :equilateral
    elsif a == b || b == c || a == c
      :isosceles
    else
      :scalene
    end
  end

  def validate_triangle
    real_triangle = [(a + b > c), (a + c > b), (b + c > a)]
    [a, b, c].each do |side|
      real_triangle << false if side <= 0 
    raise TriangleError if real_triangle.include?(false)
    end
  end

  class TriangleError < StandardError
  end

end
```

They've pulled the validation check as its own method, `validate_triangle`, that the `kind` method calls on. It has an array, `real_triangle` with three equations showing that the sum of two sides is greater than the third side; the sides *a, b,* and *c* represent the actual measurments initialized in this instance of the Triangle class. It then iterates over the sides and shovels a `false` into the `real_triangle` array if any of the sides are less than or equal to zero. Finally, it raises the `TriangleError` if the `real_triangle` array includes any false values. This, to me, seems overly complicated and confusing to me, but it does work.

The `kind` method compares the lengths of sides directly and uses `&&` ("and") and `||` ("or") to implement the logic of checking the triangle sides against one another. Like in the `validate_triangle` method, it compares the sides in all possible combinations one by one. In my solution, those comparisons happen "under the hood" using the `.uniq` method on my array of sides.

Looking at the different solutions, it really hits me how in programming there are not necessarily "right" answers and there are many different ways we can solve various problems. I think this highlights the importance of a team-driven approach to coding; different perspectives can lead to different approaches to solutions, which could have a significant impact on the performance of your program down the road.

