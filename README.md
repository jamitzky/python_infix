# python\_infix

see also the python recipe from 2005:  
http://code.activestate.com/recipes/384122-infix-operators/

Python has the wonderful "in" operator and it would be nice to have additional infix operator like this. This recipe shows how (almost) arbitrary infix operators can be defined.

```python
# simple multiplication
x=Infix(lambda x,y: x*y)
print 2 |x| 4
# => 8
```

Of course this is a hack that plays with Python's ability of operator overloading. It is some sort of similar to the Ruby syntactic sugar recipe, but I think this might really be useful. One could e.g. define infix operators for set arithmetics like union, intersection and so on which would really enhance the readability of the code. I wonder whether it would be possible to use decorators for the definition of the infixes and one could omit the stars around the infix?

Addendum: Thanks for all the interest and the helpful comments for the recipe. I generated a revised version that took them into account and makes the recipe more useful. Now you can call an operator by using bars || or using \<\< and >>. In principle it is easily possible to define different function calls for different operands. Also added an example from functional programming which might be helpful and shows how the hack greatly enhances readability. Time will tell whether the recipe stays a nice hack or whether people will start using it as a programming style.

Here is a discussion how the hack works: In python there are two ways for overloading an operator. Assume we want to overload the multiplication operator \* for a certain class. i.e. Xy is redefined for all expressions containing X as the first operand where X is of a special class. Then we have to overload (i.e. redefine) the method **mul**(X,y) of the class of X. This is nice and works in many programming languages in some way. But python gives you more: You can also redefine the meaning yX with arbitrary y for all X. Then you have to overload the method **rmul**(X,y). Thus you can define a different meaning of the multiplication operator for left and right multiplication. This is also possible for +,-,/,\*,\<\<,>>,& and | which call the methods **add**, **sub**, **div**, **pow**, **lshift**, **rshift**, **and** and **or**. Now we define a special class "Infix" which exploits this property and by leaving out the blanks we can write it in a form a \*op b.

Be careful that this is a hack of the language and not the definition of a new keyword. (as David S. pointed out in the comments). Of course the precedence and associativity rules for the operators still apply and therefore 23 x 4 means (23) x 4 while 2+3 x 4 means 2 + ( 3 x 4). (thanks to Raymond Hettinger for pointing this out).

This recipe alkso works for currying functions:

```python
# functional programming (not working in jython, use the "curry" recipe! )
def curry(f,x):
    def curried_function(*args, **kw):
        return f(*((x,)+args),**kw)
    return curried_function
curry=Infix(curry)

add5= operator.add |curry| 5
print add5(6)
# => 11
```
