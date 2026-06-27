# Migration to Java 9 Modules — The Chill Version

Okay so you have an existing Java project and you want to start using modules. The good news is you do not have to convert everything at once. You can do it gradually which is awesome. Nobody is asking you to rewrite your entire codebase over a weekend. Take it step by step.

First thing you want to do is analyze what you have. Look at all your dependencies. What libraries are you using? What packages do they expose? Are you using any internal APIs like sun or com.sun packages? Those are trouble because they are not meant for public use. You also want to check if you are using frameworks like Spring or Hibernate that rely heavily on reflection. They need special treatment because they need to access your classes in ways that normal modules do not allow.

The easiest way to start is by moving your JAR files to the module path. When you do this Java automatically converts them into automatic modules. You do not have to change anything in your code. Just point the JVM to the module path instead of the classpath. Your old libraries become automatic modules and your new code can start using module declarations. This is like a soft start. You get some benefits without doing much work.

Next you want to create module-info.java files for your own code. Start with the simplest module you have. Maybe it is a utility library with no dependencies. Create a module-info.java, give it a name, export the packages you want to share, and leave it at that. Test it. Make sure everything compiles and runs. Once that works move on to the next module. One at a time. Do not try to convert everything in one go.

The tricky part is usually frameworks. If you are using Spring Boot or Hibernate or Jackson they need reflection access to your classes. Without that they cannot work their magic. You need to open your model packages to those frameworks. Like opens com.example.model to hibernate.core and spring.context. It feels weird giving away that much access but it is necessary for now. The frameworks are working on better module support but until then this is how you make them happy.

Another thing you might run into is split packages. This happens when two different modules try to export the same package name. Like if you have module A and module B both exporting com.example. That is a conflict. You need to reorganize your packages so each module has its own unique namespace. It is a bit of work but it makes your code structure cleaner anyway.

When you are done with the conversion you get some nice benefits. Your startup time improves because the JVM does not have to load everything. Your memory usage goes down because only what is needed gets loaded. Your code is more secure because internal APIs are properly locked down. And your team knows exactly what depends on what because it is all declared in module-info.java. It is like going from a messy junk drawer to an organized toolbox. Everything has a place and you can find what you need.

The key is patience. Do it module by module. Test after every change. Use jdeps to figure out what depends on what. And do not be afraid of using --add-opens as a temporary fix while you sort out the proper modular solution. The goal is to get there eventually not overnight.
