# Overview
In this lab, you populate rentals data in Cloud SQL for the rentals recommendation engine to use.

# Objectives 

In this lab, you will:

- Create Cloud SQL instance

- Create database tables by importing .sql files from Cloud Storage

- Populate the tables by importing .csv files from Cloud Storage

- Allow access to Cloud SQL

- Explore the rentals data using SQL statements from CloudShell

# Qwiklabs setup

GCP Console Ressources: 

```

```


# Introduction

In this lab, you populate rentals data in Cloud SQL for the rentals recommendation engine to use. 
The recommendations engine itself will run on Dataproc using Spark ML.

# Create Cloud SQL instance

1. In the Console, click Navigation menu > SQL (in the Storage section).

2. Click Create instance.

3. Choose MySQL. Click Next if required.

4. For Instance ID, type rentals.

5. Scroll down and specify a root password. Before you forget, note down the root password.

6. Click Create to create the instance. It will take a minute or so for your Cloud SQL instance to be provisioned.

# Create tables

1. While you wait for your instance to be created, read the below mySQL script and answer the questions that follow below

~~~~sql
    CREATE DATABASE IF NOT EXISTS recommendation_spark;

    USE recommendation_spark;

    DROP TABLE IF EXISTS Recommendation;
    DROP TABLE IF EXISTS Rating;
    DROP TABLE IF EXISTS Accommodation;

    CREATE TABLE IF NOT EXISTS Accommodation
    (
      id varchar(255),
      title varchar(255),
      location varchar(255),
      price int,
      rooms int,
      rating float,
      type varchar(255),
      PRIMARY KEY (ID)
    );

    CREATE TABLE  IF NOT EXISTS Rating
    (
      userId varchar(255),
      accoId varchar(255),
      rating int,
      PRIMARY KEY(accoId, userId),
      FOREIGN KEY (accoId)
        REFERENCES Accommodation(id)
    );

    CREATE TABLE  IF NOT EXISTS Recommendation
    (
      userId varchar(255),
      accoId varchar(255),
      prediction float,
      PRIMARY KEY(userId, accoId),
      FOREIGN KEY (accoId)
        REFERENCES Accommodation(id)
    );

    SHOW DATABASES;
~~~~

__QUIZ__

    How many tables will this script create?

    2

    3

    1


__QUIZ__ 

    When a user rates a house (giving it four stars for example), 
    an entry is added to the _______ table.

    Recommendation

    Rating

    Accommodation
    
__QUIZ__

    General information about houses, such as the number of rooms they have 
    and their average rating is stored in the _________ table.

    Recommendation

    Rating

    Accommodation

__QUIZ__

    The job of the recommendation engine is to fill out the ___________ table for 
    each user and house: this is the predicted rating of that house by that user.

    Recommendation

    Rating

    Accommodation

2. In Cloud __SQL__, click rentals to view instance information.

__Connect to the database__

3. Find the Connect to this instance box on the page and click on connect using Cloud Shell
  ```
  Note: You could also connect to your instance from a dedicated Cloud Compute Engine VM 
  but for now we'll have Cloud Shell create a micro-VM for us and operate from there.
  ```
4. Wait for Cloud Shell to load

5. Once Cloud Shell loads, you will see the below command already typed:

   ``` gcloud sql connect rentals --user=root --quiet```
   
6. Hit Enter

7. Wait for your IP Address to be whitelisted
  ```Whitelisting your IP for incoming connection for 5 minutes...⠹```
  
8. When prompted, enter your password and hit Enter (note: you will not see your password typed in or even ****)
    You can now run commands against your database!

9. Run the below command
    ``` SHOW DATABASES; ```
    
    You should see the default system databases:
    
    ```
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | sys                |
    +--------------------+
    ```
    
    Note: You must always end your mySQL commands with a semi-colon ```;```
10. Copy and paste the below SQL statement you analyzed earlier paste it into the command line
      ~~~~sql
      CREATE DATABASE IF NOT EXISTS recommendation_spark;

      USE recommendation_spark;

      DROP TABLE IF EXISTS Recommendation;
      DROP TABLE IF EXISTS Rating;
      DROP TABLE IF EXISTS Accommodation;

      CREATE TABLE IF NOT EXISTS Accommodation
      (
        id varchar(255),
        title varchar(255),
        location varchar(255),
        price int,
        rooms int,
        rating float,
        type varchar(255),
        PRIMARY KEY (ID)
      );

      CREATE TABLE  IF NOT EXISTS Rating
      (
        userId varchar(255),
        accoId varchar(255),
        rating int,
        PRIMARY KEY(accoId, userId),
        FOREIGN KEY (accoId)
          REFERENCES Accommodation(id)
      );

      CREATE TABLE  IF NOT EXISTS Recommendation
      (
        userId varchar(255),
        accoId varchar(255),
        prediction float,
        PRIMARY KEY(userId, accoId),
        FOREIGN KEY (accoId)
          REFERENCES Accommodation(id)
      );

      SHOW DATABASES;
      ~~~~

11. Hit Enter

12. Confirm you see recommendation_spark as a database now:
      ```
      +----------------------+
      | Database             |
      +----------------------+
      | information_schema   |
      | mysql                |
      | performance_schema   |
      | recommendation_spark |
      | sys                  |
      +----------------------+
      ```

13. Run the following command to show our tables
      ~~~~sql
      USE recommendation_spark;

      SHOW TABLES;
      ~~~~

14. Hit Enter

15. Confim you see the three tables:
      ```
      +--------------------------------+
      | Tables_in_recommendation_spark |
      +--------------------------------+
      | Accommodation                  |
      | Rating                         |
      | Recommendation                 |
      +--------------------------------+
      ```

16. Run the following query
  ~~~~sql
  SELECT * FROM Accommodation;
  ~~~~
  
__QUIZ__ 

    How many rows are in the Accommodation table?

    Empty set (0)

    100

    1,000







