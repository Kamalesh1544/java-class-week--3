# Defining Java Modules — Summary

module-info.java declares module name, dependencies, and exports. requires declares dependencies. exports controls which packages are visible to other modules. opens allows reflection access for frameworks. provides/uses implements service provider interface for dependency injection. Only exported packages are accessible from outside.
