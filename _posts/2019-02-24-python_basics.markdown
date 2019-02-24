---
layout: post
title:      "Python Basics"
date:       2019-02-24 23:11:27 +0000
permalink:  python_basics
---


Part of my continuing education post-graduation from Flatiron School has been learning the Python language. Python is a computer programming language that is widely used and is known for its readability, flexibility, and libraries. Python is also known for its data analysis and visualization capabilities.

We will explore some Python basics in this blog post. These basics will include: variables, collections, loops, functions, and conditionals.

***Variables***

A variable is created when a value is assigned to it. There are no particular commands for declaring a variable as in JavaScript. Variables are also case-sensitive. 

See the examples below:

x = 1
string = “Hello”
z = [0, 1, 2]

Only alpha-numeric characters (0-9, a-z) and underscores ( _ ) can be used to create variables.

***Collections***

There are two main types of collections in Python. These include lists and dictionaries. 

*Lists*

A *list* is a sequence of ordered elements. Lists can consist of any data type, such as strings and integers. 

fruits_list = [“apples”, “oranges”, “bananas”]
numbers_list = [1, 2, 3]

We can add items to a list by using the append method. If we wanted to add peaches to the list of fruits, we would write fruits_list.append(“peaches”).

In order to access a specific item within the list, we would reference the item by its index, or location, within the list. We would write fruits_list[0] to access apples, fruits_list[1] for oranges, and fruits_list[2] for bananas.

Note that computers start counting from 0; for this reason, the first index in the list is 0. 

We can also remove an item from a list by using the remove() and del() methods. For instance, numbers_list.remove(2) would remove the first value of 2 that appears within the list. Whereas del numbers_list[2] removes an item at a specific index, which in this case would be 3.

*Dictionaries*

Another data structure in Python is a *dictionary*, which consists of a key, value pair. Dictionaries can consist of multiple key, value pairs.

student1 = {“name”: “David”}
student2 = {“name”: “Diana”, “age”: 21, “grade”: “junior”, “school”: “Boston University”}

In order to access a specific item within the list, we would reference the item by its key. We would write student2[“name”] to access Diana, student2[“age”] to access 21, student2[“grade”] to access junior, and so forth. 

Similarly, we can add an item to the dictionary by referencing the dictionary variable, followed by the key and assign a value to it. For instance, student1[“hometown”] = “Philadelphia” would add the key “hometown” with the value of “Philadelphia” to the dictionary. As a result, the dictionary student1 would look as follows:

student1 = {“name”: “David”, “hometown”: “Philadelphia”}

***Loops***

A loop is used to iterate over a collection of values and manipulate the data with a block of code. In the example below, the for loop iterates over each sport within the sports list and capitalizes the string. We define a new list called sports_list as a separate container for keeping track of the new values that are being produced with each successive iteration.

sports = [“baseball”, “basketball”, “football”]

for sport in sports:
	sports_list = []
	sports_list.append(sport.capitalize())

The result of sports_list would be as follows:

sports_list =>  [“Baseball”, “Basketball”, “Football”]

A while loop is another example of a loop in Python. The while loop will execute a set of statements repeatedly as long as a given condition is true. See the example below:

count = 0

while (count ˂ 10):
	count += 1
	print(“The count is:”, count)

print(“The loop has ended”) 

The result of the while loop would be as follows:

The count is: 0
The count is: 1
The count is: 2
The count is: 3
The count is: 4
The count is: 5
The count is: 6
The count is: 7
The count is: 8
The count is: 9

The loop has ended.
	
***Functions***

A function is a unit of code that can be used to execute a certain functionality towards any input that is provided. Functions allow us to repeat certain patterns without having to manually type the same code multiple times.

See the example below: 

function introduction(name, profession, country)
    print(f'Hello, my name is {name}. I work as a {profession}. I am from {country}')

Given a name of Sofia, profession of veterinarian, and country of Argentina, the function introduction() would output the following:

Hello, my name is Sofia. I work as a veterinarian. I am from Argentina. 

Note that the function can be used repeatedly with varying inputs and the same structure for the introduction will always be produced. The only difference in the output for the function will be the values for the inputs themselves. 

See another example below:

name = Tomáš
profession = banker
country = Czech Republic

introduction(name, profession, country) => Hello, my name is Tomáš. I work as a banker. I am from the Czech Republic. 

***Conditionals***
