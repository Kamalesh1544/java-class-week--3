# Module Types — The Chill Version

So there are four types of modules in Java and each one has its own personality. Let me break them down for you.

First up is the named module. This is the good stuff. It has a proper module-info.java file with all the declarations. It has a clear name like com.example.myapp. It explicitly says what it needs and what it shares. It sits on the module path and everything is structured and organized. This is what you want your code to be. Named modules are like people with official IDs and addresses. Everyone knows who they are and where to find them.

Next is the automatic module. This is for old school JAR files that were written before modules existed. When you put these JARs on the module path Java looks at the filename and turns it into a module automatically. If your file is called mylibrary.jar the module name becomes mylibrary. If it has dashes like my-cool-library.jar the name becomes my.cool.library because dashes get replaced with dots. The thing about automatic modules is they export everything. Every package in the JAR is available to everyone. You cannot control what gets shared. But the good news is you can require them from your named modules. So old libraries still work with new modular code. Java figured out how to make them play nice together.

Third is the unnamed module. This is where code goes when it does not fit anywhere else. If you put your code on the classpath instead of the module path it ends up in the unnamed module. All legacy code that was never converted to modules lives here. The unnamed module can use any exported package from named modules but named modules cannot require the unnamed module because it has no name. It is like being a mystery guest at a party. You can talk to people but nobody knows your real name so they cannot invite you back.

Finally there is the JDK module called java.base. This is the foundation that everything else is built on. It contains java.lang, java.util, java.io, all the core stuff. Every single module requires java.base automatically. You never have to write requires java.base because it is already there. Think of it as the foundation of a building. You do not see it but everything stands on top of it.

The real power is in mixing these types. You can have your code as a named module while using some old JARs as automatic modules while some really old stuff sits on the classpath as unnamed modules. Java handles all the plumbing between them. You just need to understand which type you are dealing with so you know what to expect.
