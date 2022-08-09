
# RegisterationForm 
> Clean Architecture + ASP.NET Core 6 Web API + .NET Core 6 Class Library + Angular 13

## Table of Contents
* [General Info](#general-information)
* [Technologies Used](#technologies-used)
* [Features](#features)
* [What do you learn in this project?](#what-do-you-learn-in-this-project)
* [Screenshots](#screenshots)
* [What does this application do?](#What-does-this-application-do)
* [Create Projects](#create-projects)
* [Develop Domain Layer](#Develop-Domain-Layer)
* [Connect To Database](#Connect-To-Database)
* [Implement UnitOfWork](#Implement-UnitOfWork)
* [Implement BL Layer](#Implement-BL-Layer)
* [Implement Api Layer](#Implement-Api-Layer)
* [Initializing UI Layer](#Initializing-UI-Layer)
<!-- * [License](#license) -->


## General Information
> This is a beginner level project to teach you:
- Clean Architecture
- ASP.NET Core Web Api
- .NET Core Library
- Angular
- Automapper


## Technologies Used
- .NET Core - version 6
- Angular - version 13
- EF Core - version 6



## Features
- CRUD Operation


## What do you learn in this project?
- How to design a multi layers project with clean architecture
- Build web api application with ASP.NET Core 6
- Build the web application UI base on Angular 13 
- Connect the UI to Backend With Angular + TypeScript 
- How to use RadiButton, Checkbox, DropDown, Input and feed them by api in Angular


## Screenshots
![image](https://user-images.githubusercontent.com/30793006/181934783-d6d901a7-102c-4907-9377-7558753a6c51.png)

## What does this application do?
this is a simple singel page application that contains a table and a form. the form has multi type of html inputs and allow you register the person data into database or edit previous data. by the table you can see data or delete them.

## Create Projects
At the first step we Create a blank solution in visual studio the add the projects as follow:
> a class library with the name *Domain* for our models, dto, ...

> a class library with the name *Infrastructure* for interfaces such as unitofwork and other common general operations that are gonna use in other layers

> a class library with the name *DAL* to communicate with database

> a class library with the name *IoC* to handle our dependency injections in one layer

> a class library with the name *BL* to implement our business roles 

> a web api .net core to implement our apis

![image](https://user-images.githubusercontent.com/30793006/182139351-cc2cc3dc-ec16-4483-bbf5-b91d503200ca.png)


## Develop Domain Layer
This is our data diagram:
![image](https://user-images.githubusercontent.com/30793006/182148220-c85e0e71-6a47-4ad0-937d-7be609547f6b.png)
We have 3 tables:
> *Person* is the table that keeps person data.

> *Personality* is the table that keeps the personalities types.(We don't have any form for crud operations on this table and it get feed direct from Sql Server Management Studio)

> *PersonPersonality* is the table that keep the data that each person has how many type of personalities.

we want to create these tables by entity framework code first and also we need a model per table to use in our project so we create a class per table just like this picture:

![image](https://user-images.githubusercontent.com/30793006/182153818-9278e716-1f06-4bee-a85c-194a7000891a.png)

as we don't have any base table for *gender* and *status* in *Person* table so we create two enums for these fileds.

## Connect to Database
Now we want to connect to database. the *DAL* Layer should be responsible for this one.
before start we should install necessory packages. install these packages on DAL Project:
> Microsoft.EntityFrameworkCore

> Microsoft.EntityFrameworkCore.Design

> Microsoft.EntityFrameworkCore.SqlServer

> Microsoft.EntityFrameworkCore.Tools

> as you know the database context get the connectin string from the end layer. in this project our context get the database connection string from *Api* layer. so we need to define database connection string in the file *appsettings.json* in *Api* layer. like this:
  `"ConnectionStrings": {
    "RegisterationFormDbContext": "Server=.;Database=RegisterationForm;Trusted_Connection=True;MultipleActiveResultSets=true"
  }`

> now we should connect our context to databse. for this one we should do it in the layer that injects the services(here, IoC layer). for this you need install the package *Microsoft.Extensions.DependencyInjection.Abstractions* then we create a class with name DbConfig and use the ef core service to connect our database context to database. like this:

![image](https://user-images.githubusercontent.com/30793006/182185009-37b606e0-adfe-43fe-8a95-f8696e17bc46.png)


> at the next step you should inject this config in the end layer with the path: *Api/Proram.cs* like this:

`builder.Services.AddDatabaseConfig(configuration);`

> now we need create database and tables by ef. before use migration you should install *Microsoft.EntityFrameworkCore.Design* on Api layer because it's needed.

> now you can go to *Package Manager Console* and write the first add migration code like this: *Add-Migrations InitializeDb*

> Congratulations your project is connected to database.

## Implement UnitOfWork
We use UnitOfWork pattern to work with data such as (get, write, update or delete) so since the UnitOfWork has 2 parts, the first part is *Interface* and the second part is *Implementation* we put the *Interface* in Infrastructure layer and implement them in the DAL layer.

> In the layer *Infrastructure* create a folder with name *IUnitOfWork* then create 2 interfaces one with name IGenericRepository and the other with name IUnitOfWork

> In IGenericRepository you should define all crud methods that you want use in project such as (getById, getAll, Insert, ...) and this class is generic type of *T* that *T* is the name of our table. it means that the *IGenericRepository* can be polymorphism of any model and your methods can operate for any table of database.

> In IUnitOfWork you should declare instance of IGenericRepository per table of database just like this:

![image](https://user-images.githubusercontent.com/30793006/182314749-6c76c23e-f58c-4553-8e15-053a2819da0a.png)

> Ok now it's time to implementation. go to *DAL* layer and create a folder with name *UnitOfWork* the create 2 classes with name GenericRepository and UnitOfWork the implement your interfaces that you made in *Infrastructure* layer.

>>don't forget that IUnitOfWork interface and UnitOfWork class both should inherit from *IDisposable* so when the UnitOfWork finished  with database it can free unmanaged resources.

> at the last ste you should inject the UnitOfWork in *IoC* layer and then in *Api* layer. so Create a class with name UnitOfWorkConfig in *IoC* layer and inject the unitofwork like this:

![image](https://user-images.githubusercontent.com/30793006/182325601-b35054f1-cec3-4f18-818b-d86c83130bed.png)

then inject this class in *Api* layer like this:

![image](https://user-images.githubusercontent.com/30793006/182325716-d78f1127-c798-4c93-9755-fb489a3a23df.png)


> You are done with *UnitOfWork*.

## Implement BL Layer
Now it's time to implement *BL* layer according to our business roles and our senarios for *CRUD* operations.
*Note:* it's not good to return database model to api, it's better to convert your database model to dto (data transfer object).
it's better to have one or more dtos per senario.

> the first step is installing *Automapper*. why? to convert database model to dto. since the working with data is *DAL* layer duty so we put it in *DAL* layer then install these packages in *DAL* layer: `AutoMapper` and `AutoMapper.Extensions.Microsoft.DependencyInjection` by the nuget package manager.

> after installing *Automaooer* You need to set somthings. 
- first create a folder with name *AutomapperProfile* in *DAL* layer then create a class with name *MappingProfile*. in this class you must declare what model map to what dto or reverse.
- second you should inject *Automapper* and your *MappingProfile* class in *IoC* layer as usual. so create a class with name *AutomapperConfig* in *IoC* layer and inject *Automapper* like this:

![image](https://user-images.githubusercontent.com/30793006/182592286-e77ffa56-9145-4b7f-bab7-5466e965b401.png)
and finally inject this mthod in *Api* layer as we did in previous steps.

> Now it's time to declare our senarios and then implement the suitable methods in *BL* layer. 

- we have a grid in our application that shows every data of person, so we need a mehod that return all persons data and for each person also show the all personalities for that person. so in the *PersonBusiness* class create a method with name *GetAllPersonInfo* then in this method first get all data from *Person* table then map this list to dto then for each record in this list get the titles of this person personalities from *PersonalityBusiness*.

- we have a form that must show all personalities from database. so we need a method that get all personalities from database. so we have a methd with name GetPersonalitiesTitle in *PersonalityBusiness* that do this.

- the user should register person info by the form. so we need a method that gets the data and save it in database. so we create a method with name *InsertPerson* in *PersonBusiness* to register data into database and we should have an other method to insert this person personalities in *PersonPersonality* table so we create an other method with name *InsertRange* in *PersonPeronalityBusiness*  and call it after we inserted person data.

- the user should be able to update person data. so we need 2 methods here one for get the target person data and put it in the form for edit and the other one for get the new data and update previous one. 

    **first** create a method with name *GetPersonById* in *PersonBusiness* to get person data.
    
    **second** create a method with name *UpdatePerson* in *PersonBusiness* to update the person data.
    
    **third** we need a method with name *DeleteRange* to delete all previous personalities for this person in *PersonPersonalityBusiness*.
    
    **fourth** we need a method with name *InsertRange* to insert new selected personalities for this person in *PersonPersonalityBusiness*.
    
    **fifth** call the *DeleteRange* and *InsertRange* in method *UpdatePerson*.

- the user should be able to delete the person data. so we want a method to delete the person from database.

before deleting person you should delete his/her personalities from *PersonPersonality* table, so we call *DeleteRange* method from *PersonPersonalityBusiness* before deleting person, then to delete the person create a method with name *DeletePerson* in *PersonBusiness*.

after all these steps don't forget inject your Businesses class in *IoC* layer so create a class with name *BusinessConfig* in *IoC* layer and inject your Businesses here and finally inject it in *Api* layer.


## Implement Api Layer
The api layer is communication bridge to out and tis the layer that serve data for other applications that communicate with your project.
base on our senarios we need 2 controllers one with name *PersonController* and the other with name *PersonalityController*.

we have 5 methods in *PersonController* and 1 method in *PersonalityController* that it's clear what they do.

> we used 4 type of http request in these 2 controllers:

  - `[HttpGet]` : use it when the method is going to deliver data in the output.
  - `[HttpPost]` : use it when the method is going to get some data from input and save it in database.
  - `[HttpPut]` : use it when the method is going to get some data and update an already saved data.
  - `[HttpDelete]` : use it when the method is going to delete some data from database.


## Initializing UI Layer
We are completely done about the backend so now it's turn to implement the UI, let's go.

I choosed bootstrap 5 for design UI and Angular 13 to connect the UI to Backend. 

So follow these steps:

- to run angular app you need to install nodejs so go to https://nodejs.org/en/download/ and download then install it.
- after installing nodejs you should angular cli to be able to complie angular code. you can install it with this command `npm install -g @angular/cli`.
- create a folder with name *RegisterationForm.UI*.
- run powershell with *administrator* mode and route to the *RegisterationForm.UI* path then execute the command `ng new UI`.
- after executing the command it starts to download necessory files and asks some questions as follow:
  - would you like to add angular routing? *press y*
  - which stylesheet format would you like to use? *select css*
- go to the path `RegisterationForm.UI/UI/src` and the file *index.html* then put the bootstrap stylesheets link in the header:

  `<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">`
  
  `<link rel="preload" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.14.0/css/all.min.css" data-rocket-async="style" as="style" onload="this.onload=null;this.rel='stylesheet'" integrity="sha512-1PKOgIY59xJ8Co8+NE6FZ+LOAZKjy+KY8iq0G4B3CyeY6wYHN3yt9PW0XpSriVlkMXe40PTKnXrLnZ9+fkDaog==" crossorigin="anonymous" />`
  
  and put the js file in the body:
  
  `<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>`
  
  now we are done with initializing the UI. the next step is implementing this layer.





