[<- Back to the portfolio](https://s-bishnoi.github.io/shubham-bishnoi/)

# Database Design

## Overview

![] (./images/photo.png)

The intention for “A College Residence Database” topic was to understand the databases that are used at the front desks, as I did while working part time at the front desk. The selected topic is excellent for basic SQL, as there are different data types like text, integer or real which are really efficient when doing simple queries for sum, counting and aggregating by groups. The residence table (with 5 rows) was created first and all the data was created manually and is fictitious. Quad table (with 15 rows) was created after that, keeping residence code as the foreign key. All the data in the quad table was manually created. Student (with 780 rows), Student Employees (with 42 rows) and Employees (with 25 rows) tables were created next. First names, last names, gender and phone numbers were created using generatedata.com. All the other columns for these three tables were manually created to match the properties of the residence. Parking table (with 11 rows) was created last and all the data in that table was created manually.

## Creating the database

`CREATE TABLE "Residence" ("Code" TEXT NOT NULL UNIQUE, "Name" TEXT NOT NULL, "YearOpened" INTEGER, "Style" TEXT NOT NULL, "FrontDesk" TEXT NOT NULL, "StreetNumber" INTEGER, "StreetName" TEXT NOT NULL, "City" TEXT NOT NULL, "State" TEXT NOT NULL, "Country" TEXT NOT NULL, "PostalCode" TEXT NOT NULL,PRIMARY KEY("Code"));`

`CREATE TABLE "Quad" ("ID" INTEGER UNIQUE, "ResidenceCode" TEXT, "Name" TEXT, "Floors" INTEGER, "Rooms" INTEGER, "AreaSquareMetres" REAL, "Bathrooms" INTEGER, "CommonRooms" INTEGER, "Laundry" TEXT, "DoubleRoomUnits" INTEGER, PRIMARY KEY("ID"), FOREIGN KEY("ResidenceCode") REFERENCES "Residence"("Code"));`

`CREATE TABLE "Employees" ("ID" INTEGER UNIQUE, "FirstName" TEXT, "LastName" TEXT, "Gender" TEXT, "PhoneNumber" TEXT, "ResidenceCode" TEXT, "Title" TEXT, FOREIGN KEY("ResidenceCode") REFERENCES "Residence"("Code"), PRIMARY KEY("ID"));`

`CREATE TABLE "StudentEmployees" ("ID" INTEGER UNIQUE, "FirstName" TEXT, "LastName" TEXT, "Gender" TEXT, "QuadID" INTEGER, "SupervisorID" INTEGER, "Title" TEXT, "Phone" TEXT, FOREIGN KEY("SupervisorID") REFERENCES "Employees"("ID"), FOREIGN KEY("QuadID") REFERENCES "Quad"("ID"), PRIMARY KEY("ID"));`

`CREATE TABLE "Students" ("ID" INTEGER, "FirstName" TEXT, "LastName" TEXT, "Gender" TEXT, "QuadID" INTEGER, "UnitNumber" INTEGER, "AcademicYear" TEXT, "EmergencyContact" TEXT, PRIMARY KEY("ID"), FOREIGN KEY("QuadID") REFERENCES "Quad"("ID"));`

`CREATE TABLE "Parking" ("Lot" TEXT, "Charged" TEXT, "ClosestQuad" INTEGER, "Overnight" TEXT, "Accessibility" TEXT, FOREIGN KEY("ClosestQuad") REFERENCES "Quad"("ID"), PRIMARY KEY("Lot"));`