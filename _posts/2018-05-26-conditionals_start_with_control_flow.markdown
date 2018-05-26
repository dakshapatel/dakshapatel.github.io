---
layout: post
title:      "Conditionals start with Control Flow. "
date:       2018-05-26 04:58:13 +0000
permalink:  conditionals_start_with_control_flow
---



Control flow is something that we do everyday without thinking about it. For example, if you are hungry then you should eat or else you should not eat. Pretty simple, right?


In computer language control flow allows you to tell your program what code to execute conditionally. 

There are three basic forms in which you can tell your program to conditionally execute certain code.  

Conditional #1: If, Elsif, Else Statement - ex. If something is true, do one thing, if something is not true, do another. 

In code language, that looks like the following:

```
apples = 8
if (apples > 10)  **<= Conditional**
    puts 'There are greater than 10 apples'  **<= Statement**
elsif (apples < 11 && apples > 9 )   **<= Conditional**
    puts 'There are  10 apples'  **<= Statement**
else **<= Conditional**
    puts 'How many apples are there?'  **<= Statement**
end
```

Conditional #2: Unless Statement - The best way of understanding Unless Statements is to think of them as reversing the return of the conditional statement and Ruby has nicely reduced the effort with the Unless Statement. 

In code language, that looks like the following:

```
x = 1

unless x > 0 **<= Conditional**
  puts "x is less than 0"  **<= Statement**
end
```


Conditional #3: Case Statement - If you have a lot of options you're using in your code, consider using Case/When Statements.

In code language, that looks like the following:

```
 a = 1
 case 
 when a < 5 then puts "#{a} is less than 5"    
 when a == 5 then puts "#{a} equals 5"   
 when a > 5 then puts "#{a} is greater than 5" 
 end
```



Pretty Simple right? Go ahead and teach your mom all about conditionals. 






