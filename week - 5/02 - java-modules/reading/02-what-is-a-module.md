# What is a Module? — The Chill Version

Alright so a module is basically a self contained unit of code. Think of it like an apartment building. Each apartment has its own rooms, its own kitchen, its own bathroom. The person living in apartment 5A cannot just walk into apartment 3B uninvited. There are walls and doors and locks. That is exactly what a module is. It has its own packages, its own classes, and it decides what gets shared and what stays private.

The heart of every module is a file called module-info.java. This file sits at the root of your source code and it is like the directory of your apartment building. It says hey my module is called com.example.myapp, I need java.sql to work, I am sharing my api package with the world, and I am keeping my internal stuff to myself. That is all the module-info.java does. It is a declaration of what your module needs and what it gives.

So there are three types of modules you need to know about. First is a named module. This is the proper module with a module-info.java file. It is like a well organized apartment with a proper address. Everything is labeled and structured. This is what you want to aim for.

Second is an automatic module. This is for old JAR files that do not have a module-info.java. When you put these on the module path Java automatically converts them into modules. It takes the JAR filename and uses that as the module name. So if your file is called mylibrary.jar the module name becomes mylibrary. If it is my-cool-library.jar the name becomes my.cool.library because dashes become dots. It is not perfect but it lets you use old libraries without rewriting them.

Third is an unnamed module. This is the default bucket for anything that does not have a proper module declaration. If you put your code on the classpath instead of the module path it ends up here. You can still access modules that export their packages but unnamed modules cannot be required by named modules. It is like being a guest in the building. You can visit the common areas but nobody knows your name.

The cool thing is you can mix and match. You can have your code as a named module while using some old JARs as automatic modules. Java figures out how to make them work together. It is like having a modern apartment building but allowing some vintage furniture in there. As long as the furniture fits through the door it works.
