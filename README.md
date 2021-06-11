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

Responsible for **loading and running** .NET application, garabage collection, execution and maintaining parallel threads, etc. **Linking of different dlls is also done by CLR as and when needed** while program execution.

Host or Windows (the OS) will be running at Start, the CLR won’t begin execution until Windows starts it. When an application executes, OS inspects the file to see whether it has a special header to indicate that it is a .NET application. If not, Windows continues to run the application.**If an application is for .NET, Windows starts up the CLR and passes the application to the CLR for execution**. The CLR loads the executable assembly, finds the entry point, and begins its execution process.

Article Link - https://medium.com/@mirzafarrukh13/common-language-runtime-dotnet-83e0218edcae

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

NOTE : Process.GetCurrentProcess().ProcessName reports w3wp/iisexpress (in-process) or dotnet (out-of-process).

### `launchsettings.json`-

Configuration file for **storing launch related configuration** present inside properties folder. Can have the following command names -
- IISExpress
- Project

By default visual studio is set to launch IISExpress profile i.e. you app is hosted on IIS Express(If the AppNetCoreHostingModel in .csproj is InProcess).

If however **user choses Project profile** then the app is **hosted in Kestrel server**(If the AppNetCoreHostingModel in .csproj is InProcess).

If however user choses Project profile and the AppNetCoreHostingModel is OutOfProcess then the app is hosted in Kestrel server internally and IIS acts as a reverse proxy server.

**We can set environment variable from here as well**.

https://www.youtube.com/watch?v=u2S4TkkACVc&list=PL6n9fhu94yhVkdrusLaQsfERmL_Jh4XmU&index=8

### `Configuration files`-

ASP.NET core can store configuration at various places as below -
- **appsettings.json**
- appsettings.{Environment}.json
- User Secrets
- Environment variables
- Command Line Arguements

Precedence order shown above is from low to high. **Any key present at higher precendence order will overwrite the once in lower precendence order**.
The above **configurations are injected through the IConfigurationService interface** that stored the mapping of all key-value pairs across all configuration sources.

Eg. Key in appsettings.production.json will overwrite the value in appsettings.json. Similarly key in Environment variable will overwrite the value in appsettings.production.json file.

https://www.youtube.com/watch?v=m_BevGi7zBw&list=PL6n9fhu94yhVkdrusLaQsfERmL_Jh4XmU&index=9

### `Kestrel Server` - 

Kestrel is an open source cross-platform server **built to host .NET core applications only** and provide **better performance than IIS server**. It is lighweight and is not a full fledged server thus it is **used behind more proper servers** like IIS, nginx, apache, etc that act like a proxy to the kestrel server.

Reason why Kestrel is made light weight is because it typically runs behind serves like Nginx/IIS/Apache in production that provide the additonal work of security, load balancing, etc. Thus the job of kestrel is only to process incoming http requests and not be bothered about anything else.

**Default WebHostBuilder in Program.cs is configured to use Kestrel by default as the web server.**

Details - https://stackify.com/what-is-kestrel-web-server/

### `IIS Server` -

Built in server provided by microsoft to host web applications. Supports various protocols like http, ftp, smtp, etc

https://stackify.com/iis-web-server/

### `Kestrel vs IIS` -

https://stackify.com/kestrel-web-server-asp-net-core-kestrel-vs-iis/

### `General flow of a HTTP request` -

- A request arrives from the web to the kernel-mode HTTP.sys driver.
- The driver routes the native request to IIS on the website's configured port, usually **80 (HTTP) or 443 (HTTPS)**.
- Host(Operating System) makes sure that CLR(Common Lanuague Runtime) is loaded whenever first ever request to run a .NET module comes.
- The request is sent to the ASP.NET Core middleware pipeline(Configure() method inside Startup.cs configures the pipeline).
- The middleware pipeline handles the request and passes it on as an HttpContext instance to the app's logic.
- The app's response is passed back to IIS.
- IIS sends the response to the client that initiated the request.

### `Middleware` -

- Middleware is a piece of code in .NET core that can handle an HTTP Request or an HTTP Response (Eg. Authentication Middleware, MVC Middleware)
- Middlewares are **configured in Configure()** method of **Startup.cs** file.
- Logging Middleware automatically logs the incoming HTTP requests and ocassionally responses(when there in an exception).
- Middleware components are **executed in order** as they are added.
- **Add your middleware but calling the Use() method of IApplicationBuilder** inside Startup.cs file or simply create an extension method for it.
- We can use **Run() method of IApplicationBuilder** but that creates a **terminal middleware**. Use() takes two arguement, HttpContext and next middleware, thus we can call next.Invoke() inside Use() to call the next middleware configured in the pipeline.
- Eg. **app.Use(async (context, next) => { //do your code; await next.Invoke(); });** Notice there is a **next parameter in delegate to Invoke the next middleware in the pipeline**.
- For **terminal middleware** we write as **app.Run(async (context) => { //do your code; });** Notice that **there is no next parameter.** in the delegate.
- Serving **static files need to be in a special folder called wwwroot**, and for that you need **Static Middleware**.

Few Important middleware configured in Startup.cs are as follows -
- **app.UseStaticFiles()**
- **app.UseAuthentication()**
- **app.UseAuthorization()**

Notice the **UseMiddlewareName() format for writing extension methods of middlewares**. Similar for like **AddServiceName() is used while writing extension methods for adding services**.

MSDN article - https://docs.microsoft.com/en-us/aspnet/core/fundamentals/middleware/?view=aspnetcore-5.0

### `WebHost.CreateDefaultBuilder(string[] args)` -

- Extension method inside WebHost.cs class
- Creates a default web host builder with some pre-configured default settings like using Kestrel as default web server, Logging, using configurations files etc

### `Environment variables` -

- In asp.net core, environment information is stored in an environment variable named **ASPNETCORE_ENVIRONMENT**. This variable determines on which environment is the app running and accordingly can be used in our application to configure different properties.
- **IHostingEnvironment**(already injected into the configure method) contains an extension method named **IsDevelopment**(), **IsProduction**() and **IsStaging()** to determine the environment type on which the app is running. Also there is a property named EnvironmentName which we can use to print the environment type.
- Environment variables can be either set manually or we can set them in launchsettings.json file.
- **Development, Staging and Production** are the three types of hosting enviroments.
- By **default** environment variable ASPNETCORE_ENVIRONMENT is set to **Production**.

### `Dependency Injection` -

- **ConfigureServices()** method inside Startup.cs is used to register any dependecy.
- ConfigureServices() has **IServiceCollection** interface injected as parameter which stores the list of all dependecies in an application.
- Eg. services.AddMvc() is used to register the services required by the MVC framework.
- **IServiceCollection** contains three extension methods to register dependencies - **AddScoped(), AddSingleton(), AddTransient().**
- We use dependecy injection for Inversion of Control i.e. classes should not bother about creating dependent object's instances.
- It also helps in lose coupling, because in future if we have to configure a different implementation of an interface then we can do that at just once single instance i.e. inside the ConfigureServices() method where the dependency was registered.

# Other useful links -

### `Deploying .NET Core apps on linux` -

https://www.c-sharpcorner.com/article/how-to-deploy-net-core-application-on-linux/
