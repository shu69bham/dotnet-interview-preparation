# .NET Core Interview Preparation
This repository is setup specifically for important topics that one should know for .NET core interview.

Best series on **.NET core tutorials** - https://www.youtube.com/watch?v=4IgC2Q5-yDE&list=PL6n9fhu94yhVkdrusLaQsfERmL_Jh4XmU&index=1 by **Kudvenkat** for detailed explanation of all .NET core entities and other components to build a web application.

# .NET Core important components
Few Important points about .NET core -
- Open Source
- **Cross-platform** (Can be developed and run on multiple OS)
- Light weight (Heavy unnecessary components were decoupled from the original ASP.NET)
- High performance
- Supports Dependency Injection out of the box
- Modular (with built in support for **middlewares** for forming request/response pipelines)

.NET core is made up of various components that help in smooth running of .net core across all platforms. Following is the list of few such components - 


### `.NET SDK vs .NET Runtime` - 

.NET SDK is a complete package(**superset**) that comprises of all components needed to **develop as well as run .net applications** on a machine. Used in development environment.

.NET Runtime is **only used for running .NET Applications**. It does not include kit for developing apps. Used in production environment.

### `Compilation Process` - 

High level language in **vb or c#** ==> Converted to **MSIL** by the primary complier(either by vbc or csc) ==> MSIL is converted to Machine Code by **JIT** Compiler.

NOTE : vbc = vb compiler, csc = c sharp compiler

- **MSIL(Microsoft InterMediate Language) can be in .dll or .exe** form depending if the high level code had a **main() method** or not.

- **JIT(Just In Time)** complier is a part of .NET runtime(also called as CLR i.e. Common Language Runtime).

- Conversion of high level languages like vb/c# to MSIL is done by taking into consideraion **CLS**(Common Language Specification) and **CTS**(Common Type System) that define how a program or data type is used and converted to the corresponding MSIL.

- Eg. Mono runtime implements it's own JIT to convert MSIL to Linux. There are different Mono Runtimes for different platforms as per the user's need.

Youtube video - https://www.youtube.com/watch?v=6oYcZ-D8Fyw&list=PLWPirh4EWFpGn__IAQDYgdX0VuJ0aCqDK&index=2 by tutorialspoint

### `CLR(Common Language Runtime)` -

Responsible for loading and running .NET application, garabage collection, execution and maintaining parallel threads, etc. Linking of different dlls is also done by CLR as and when needed while program execution.

### `main() method` -

- main() method is present inside Program.cs file. It is the **starting point** of the application.
- .NET core application starts as a console app and by calling the main() method, the configuration of asp.core is done and it kick starts .net core web application.
- **CreateWebHostBuilder(args).Build().Run()** inside main() method is called that **creates a IWebHostBuilder**, builds it and runs the application and starts listening to incoming http requests.
- https://www.youtube.com/watch?v=X60RR34gKy0&list=PL6n9fhu94yhVkdrusLaQsfERmL_Jh4XmU&index=5

### `Startup.cs` -

- Consists of two methods ConfigureServices() and Configure().
- **ConfigureServices()** method **configures the services** that are needed by the applications.
- **Configure()** method **configures the request pipeline**.
- Inside CreateWebHostBuilder() method present in Program.cs file, we can configure to use a different file apart from Startup.cs to act like Startup class.
- **IWebHostBuilder** has an extension method **UserStartup<>()**, that can be used to configure our own class to act as startup.

### `ADO.NET` -

Provides supporting methods for connecting to databases through your .net application. ORM tools like **Nhibernate use ADO.NET internally** to interact with database.

### `In Process Hosting` - 

- By Default in windows we get IIS server.
- In the **.csproj** file, mapping for hosting is done by the tag "AspNetCorHostingModel" and setting it to InProcess
- Setting it to InProcess, **CreateWebHostBuilder() internally calls UseIIS()** method which uses **IIS worker process** for hosting the application.
- **InProcess** hosts the webserver in IIS(**w3wp.exe**) or IISExpress(**iisexpress.exe**).
-  **Visual Studio** **by** **default** **uses** **iisexpress.exe**(light weight version of IIS optimized for developing applications only, not for production).
-  If however we run the application by using **dotnet run** command, then it build and runs using the **Kestrel server(dotnet.exe)**.
-  In Production we use IIS, Nginx alongwith Kestrel server.
-  NOTE : from .NET core 3, the process name is the project name and not dotnet.exe anymore(Eg. ToplogyPlanner.exe)
-  NOTE : **A worker process in the context of IIS is one that can execute web applications** and is responsible for handling the requests **specific to a particular application pool**.
-  NOTE : **An application pool serves as a container** for your applications in IIS

https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/in-process-hosting?view=aspnetcore-5.0

### `Out of Process Hosting` - 

- In the **.csproj** file, mapping for hosting is done by the tag "AspNetCorHostingModel" and setting it to OutOfProcess
- It has **2 webservers(Internal and External web servers).** External = Reverse Proxies or Client facing web servers, Internal = Servers receiving requests from proxy server.
- **IIS/Nginx/Apache are used as reverse proxy servers** alongwith the Kestrel server at the backend.
- Reverse proxy servers adds an additonal layer of configuration and security. These servers integrate better with the existing infastructure unlike Kestrel which is a light weight server. It can also be used for load balancing multiple instances of applications running using Kestrel server.

https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/out-of-process-hosting?view=aspnetcore-5.0

### `Kestrel Server` - 

Kestrel is an open source cross-platform server built to host .NET core applications and provide **better performance than IIS server**. It is lighweight and is not a full fledged server thus it is **used behind more proper servers** like IIS, nginx, apache, etc that act like a proxy to the kestrel server.

Reason why Kestrel is made light weight is because it typically runs behind serves like Nginx/IIS/Apache in production that provide the additonal work of security, load balancing, etc. Thus the job of kestrel is only to process incoming http requests and not be bothered about anything else.

Details - https://stackify.com/what-is-kestrel-web-server/

### `IIS Server` -

Built in server provided by microsoft to host web applications. Supports various protocols like http, ftp, smtp, etc

https://stackify.com/iis-web-server/

### `Kestrel vs IIS` -

https://stackify.com/kestrel-web-server-asp-net-core-kestrel-vs-iis/

### `General flow of a HTTP request` -

- A request arrives from the web to the kernel-mode HTTP.sys driver.
- The driver routes the native request to IIS on the website's configured port, usually **80 (HTTP) or 443 (HTTPS)**.
- After the IIS HTTP Server processes the request the request is sent to the ASP.NET Core middleware pipeline(Configure() method inside Startup.cs configures the pipeline).
- The middleware pipeline handles the request and passes it on as an HttpContext instance to the app's logic.
- The app's response is passed back to IIS.
- IIS sends the response to the client that initiated the request.

# Other useful links -

### `Deploying .NET Core apps on linux` -

https://www.c-sharpcorner.com/article/how-to-deploy-net-core-application-on-linux/
