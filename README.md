# .NET Core Interview Preparation
This repository is setup specifically for important topics that one should know for .NET core interview.

# .NET Core important components
.NET core is made up of various components that help in smooth running of .net core across all platforms. Following is the list of few such components - 


### `Compilation Process` - 

High level language in vb or c# ==> Converted to 'MSIL' by the primary complier(either by vbc or csc) ==> MSIL is converted to Machine Code by 'JIT' Compiler

NOTE : vbc = vb compiler, csc = c sharp compiler

MSIL(Microsoft InterMediate Language) can be in .dll or .exe form depending if the high level code had a main method or not.

JIT(Just In Time) complier is a part of .NET runtime(also called as CLR i.e. Common Language Runtime).

Conversion of high level languages like vb/c# to MSIL is done by taking into consideraion CLS(Common Language Specification) and CTS(Common Type System) that define how a program or data type is used and converted to the corresponding MSIL.

Eg. Mono runtime implements it's own JIT to convert MSIL to Linux. There are different Mono Runtimes for different platforms as per the user's need.

Youtube video - https://www.youtube.com/watch?v=6oYcZ-D8Fyw&list=PLWPirh4EWFpGn__IAQDYgdX0VuJ0aCqDK&index=2

### `CLR(Common Language Runtime)` -

Responsible for loading and running .NET application, garabage collection, execution and maintaining parallel threads, etc

### 'ADO.NET' -

Used for connecting to databases. ORM tools like Nhibernate use ADO.NET internally to interact with database.

### `Kestrel server` - 

Kestrel is an open source cross-platform server built to host .NET core applications and provide better performance than IIS server. It is lighweight and is not a full fledged server thus it is used behind more proper servers like IIS, nginx, apache, etc that act like a proxy to the kestrel server.

Details - https://stackify.com/what-is-kestrel-web-server/

### `Kestrel vs IIS` -

https://stackify.com/kestrel-web-server-asp-net-core-kestrel-vs-iis/

# Other useful links -

### `Deploying .NET Core apps on linux` -
https://www.c-sharpcorner.com/article/how-to-deploy-net-core-application-on-linux/
