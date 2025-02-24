# Getting Started

This document is aimed to create and run an ASP.NET Zero based project in just 5 minutes. It's assumed that you already [purchased](https://aspnetzero.com/Pricing) and created your ASP.NET Zero account.

## Create Your Project

[Login to aspnetzero.com](https://aspnetzero.com/Account/Login) with your username and password. Go to the [download](https://aspnetzero.com/Download) page. You will see a form as shown below:

![Download Angular Project](images/download-angular-3.png)

Select **ASP.NET Core & Angular** as Project Type and fill other required fields. Your project will be ready in one minute. When you open the downloaded zip file, you will see four folders:

![Client Server Folders](images/angular-solution-folders.png)

-   **angular** folder contains the Angular UI application which is configured to work with the [angular-cli](https://cli.angular.io/).
-   **aspnet-core** folder contains the server side ASP.NET Core solution and configured to work with [Visual Studio](https://www.visualstudio.com/vs/community/).
-   **ui-tests-playwright** folder contains end-to-end tests for the UI using Playwright, a Node.js library for browser automation. This folder is configured to work with [Visual Studio Code](https://code.visualstudio.com/).

### Merging Client and Server Solutions

Client and Server solutions are designed to work separately by default. If you want to work on a single Visual Studio solution, you can select "Single Solution" checkbox while downloading your project.

If you have selected this option, please follow [Getting Started](Getting-Started-Angular-Merged) document for ASP.NET Core & Angular merged solution.

## Pre Requirements

- [Visual Studio 2017 (v15.9.0+)](https://www.visualstudio.com) (for backend ASP.NET Core application)
- [Node.js 16.x+ with NPM 8.x+](https://nodejs.org/en/download/)
- [Yarn v1.x](https://classic.yarnpkg.com/lang/en/)

## ASP.NET Core Application

When you open the server side solution (**\*.Web.sln**) using **Visual Studio 2017+**, you will see the solution structure as below:

![ASP.NET Core solution structure](images/aspnet-core-host-solution-4.png)

> If you want to work on only MAUI project, open **\*.Maui.sln** solution. If you want to work on both of MAUI and Web projects, open **\*.All.sln** solution.

Right click the **.Web.Host** project and select "**Set as Startup project**". Then **build** the solution. It may take a longer time during the first build since all **nuget** packages will be restored.

### Database

#### Connection String

Open **appsettings.json** in **.Web.Host** project and change the **Default** connection string if you want:

```json
"ConnectionStrings": {
    "Default": "Server=localhost; Database=PhoneBookDemoDb; Trusted_Connection=True; TrustServerCertificate=True;"
}
```

#### Migrations

We have two options to create and migrate database to the latest version.

##### ASP.NET Zero Migrator Application

ASP.NET Zero solution includes a **.Migrator** (like Acme.PhoneBookDemo.Migrator) project in the server side solution. You can run this tool for database migrations on development and production  (see [migrator docs](Migrator-Console-Application) for more information).

##### Entity Framework Migration Command

You can also use Entity Framework Core's built-in tools for migrations. Open **Package Manager Console** in Visual Studio, set *.**EntityFrameworkCore** as the **Default Project** and run the **Update-Database** command as shown below: 

![dotnet ef database update](images/update-database-ef-core.png)

This command will create your database. Initial data will be inserted when you run the ***.Web.Host** project. You can open SQL Server Management Studio to check if database is created:

![ASP.NET Zero Database Tables](images/created-database-tables-4.png)

You can use EF console commands for development and Migrator.exe for production. But notice that; Migrator.exe supports running migrations in multiple databases at once, which can be useful in development/production for multi tenant applications.

### Configure Multi-Tenancy

ASP.NET Zero supports multi-tenant and single-tenant applications. Multi-tenancy is **enabled by default**. If you don't have an idea about multi-tenancy, you can read it on [wikipedia.org/wiki/Multitenancy](https://en.wikipedia.org/wiki/Multitenancy). If you don't want to create a multi-tenant application, you can **disable** it by setting **PhoneBookDemoConsts.MultiTenancyEnabled** to false in the ***.Core.Shared** project (name of the PhoneBookDemoConsts will be like *YourProjectName*Consts for your project).

### Run API Host

Once you've done the configuration, you can run the application. Server side application only contains APIs. When you start the application you will see a login page like below:

![Swagger UI Login Page](images/host-login-page.png)

If you log in to host application, then you will see a page like below:

![Swagger UI Home Page](images/host-home-page.png)

You can navigate to **Swagger UI**, **Hangfire Dashboard** or **GraphQL Playground** from this page. Note that, by default only Swagger UI and GraphQL Playground is enabled, you can enable Hangfire by following [Hangfire documentation](Infrastructure-Background-Jobs).

For example when you navigate **Swagger UI**, you will see following page:

![Swagger UI](images/swagger-ui-ng2-1.png)

## Angular Application

### Restore Packages

Navigate to the **angular** folder, open a command line and run the following command to restore the packages:

```bash
yarn
```

Then, run the following command to create dynamic bundles *(This is only required for the first time when you download the project or when you update dynamic bundles)*:

```bash
npm run create-dynamic-bundles
```

We use [yarn v1.x](https://classic.yarnpkg.com/lang/en/) because NPM has some problems; It is slow and can not consistently resolve dependencies. Yarn solves those problems and it is compatible to NPM as well.

### Running The Application

Run the following command in the command line:

```bash
npm start
```

Once the application compiled, you can browse <http://localhost:4200> in your browser. ASP.NET Zero also has also **HMR** (Hot Module Replacement)  enabled. You can use the following command (instead of NPM start) to enable HMR on development time:

```bash
npm run hmr
```

### Login

All ready! Now, you can login to the application:

![Angular Login Page](images/login-screen-3.png)

If multi-tenancy is enabled, you will see the current tenant and a change link. If so, click to **Change** and enter **default** as tenant name. If you leave it empty, you login as the host admin user. Then enter **admin** as username and **123qwe** as password (remember to change it to a more secure password on production!).

### Application UI

When you login to the application as a Tenant admin, you will see the sample dashboard screen:

<img src="images/dashboardV3.png" alt="Dashboard" class="img-thumbnail" width="1235" height="965" />

## ASP.NET Zero Power Tools

ASP.NET Zero Power Tools lets you to create a new page from the database to the UI layer by just typing your entity properties. It creates the entity, related permissions, application services, DTOs, client-side code, a menu element and so on... It also creates & applies database migrations.

Finally you will have a CRUD page with insert, update, delete, list and excel export functions. To minimize the effort of creating a new page, install the Power Tools from the following link:

[https://marketplace.visualstudio.com/items?itemName=Volosoft.AspNetZeroPowerTools](https://marketplace.visualstudio.com/items?itemName=Volosoft.AspNetZeroPowerTools)

## Next

* [Solution Overview](Overview-Angular.md)
