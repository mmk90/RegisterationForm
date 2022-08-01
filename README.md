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
* [Usage](#usage)
* [Project Status](#project-status)
* [Room for Improvement](#room-for-improvement)
* [Acknowledgements](#acknowledgements)
* [Contact](#contact)
<!-- * [License](#license) -->


## General Information
> This is a beginner level project to teach you:
- Clean Architecture
- ASP.NET Core Web Api
- .NET Core Library
- Angular


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


## Complete Domain Layer
This is our data diagram:
![image](https://user-images.githubusercontent.com/30793006/182148220-c85e0e71-6a47-4ad0-937d-7be609547f6b.png)
We have 3 tables:
> *Person* is the table that keeps person data.

> *Personality* is the table that keeps the personalities types.(We don't have any form for crud operations on this table and it get feed direct from Sql Server Management Studio)

> *PersonPersonality* is the table that keep the data that each person has how many type of personalities.

we want to create these tables by entity framework code first and also we need a model per table to use in our project so we create a class per table just like this picture:

![image](https://user-images.githubusercontent.com/30793006/182153818-9278e716-1f06-4bee-a85c-194a7000891a.png)

## Project Status
Project is: _in progress_ / _complete_ / _no longer being worked on_. If you are no longer working on it, provide reasons why.


## Room for Improvement
Include areas you believe need improvement / could be improved. Also add TODOs for future development.

Room for improvement:
- Improvement to be done 1
- Improvement to be done 2

To do:
- Feature to be added 1
- Feature to be added 2


## Acknowledgements
Give credit here.
- This project was inspired by...
- This project was based on [this tutorial](https://www.example.com).
- Many thanks to...


## Contact
Created by [@flynerdpl](https://www.flynerd.pl/) - feel free to contact me!


<!-- Optional -->
<!-- ## License -->
<!-- This project is open source and available under the [... License](). -->

<!-- You don't have to include all sections - just the one's relevant to your project -->

