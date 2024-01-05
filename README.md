# Azure Database Migration

## Milestone 2
The beginning of this project consists in seting up the correct work enviroment. So, after setting up this repo (in Milestone 1), the first stage is creating a Production Enviroment: 
- The first step is to create a virtual machine on Azure, and making sure the image of the VM is set up to allow the RDP protocol. In this way we'll be able to connect to the VM by using Windows Remote Desktop.
- Afterthat we have to make sure that SQL Server and SQL Server Management Studio (SSMS) are installed on the VM, and if not we can istall them by using the Microsoft Download Center.
- Now we can download (the dowload was given) and restore the AdventureWorks database on SSMS, and in this way, we'll finally replicate an authentic production database scenario.

## Milestone 3
Now we have all the basics to start the Migration Process, and we can begin our work by using Microsoft Azure and creating an Azure SQL Database within an Azure SQL Server. This Server should have SQL login as authentication method, and all the appropriate firewall rules (making sure to include one with the IP address of your virtual machine).
- Then we can move on by using Azure Data Studio, which is an application that can be easily downloaded on our VM. Here we can connect to both our servers: the first is our local containing the AdventureWorks Database, and the second one is the Azure SQL Server we have created on Azure. Something to make sure is that the local server should have windows authentication and the server on Azure SQL authentication under 'authentication methods'.
- Now we can download the Azure Data Studio extention's SQL Schema compare and using it to duplicate all the schemas and tables from the AdventureWorks Database to the Azure SQL Database (we are not coping data but just the setting of the database). If, after a final check, all the schemas are now on the Azure Database, we can move on. (A way to check this is by simply go on the related server on Azure Data Studio, and, under the Azure SQL Database, under Tables, we should find all the tables).
- Then, we can proceed with the migration of the data. So we start by downloading the Azure SQL Migration extention and, by following the wizard procedure we should be able to transferr all the data to our Azure SQL Database. Within this process we will create an Azure Migration Service on Microsoft Azure and we should be able to use it to connect to the cloud. Furthermore, a suggestion to avoid any errors could be making sure to use all the correct passwords and authentication methods.
- The Migration procedure should be finished and succeded, but we should not forget to check and validate that all the data as been transferred correctly. On easy way to do so is by go on the Azure Database on Azure Data Studio and on every table do right click and then 'Select Top 1000'.

## Milestone 4
This next next milestone is about working with backups.
- Firstly, we want to create a backup file of the database on our VM. 
\
In order to do so, we need to open SSMS on the VM (and connect to the VM), and right click on the AdventureWorks database, then 'Tasks', and then 'Back up'. Here a dialog page will open and we could choose the location to store the backup.
- Now we want to move the backup file on Azure. This means that we have to set up a Blob Storage account to store it on Azure. So, we'll begin with creating a storage and a container within the storage. Then, on the storage we can upload the backup file a put it in the correct container.

The next part is about setting up a development enviroment, which is like a sandbox where we can work and manage the database and its data without altering the originals.
What we need to do is creating a new Azure Virtual Machine and installing SQL Server on it. Then we can restore the Adventure Database by using its backup file. Also, as we'll manipulate the data, we want to set up periodical automated backups to avoid and loses:
- On our new VM, that from now on we'll call enviroment VM, we start by connection on our local server on SSMS. Here, in the Object Explorer, we right-click on the SQL Server Agent node and then click Start, then we press yes when we are asked if we want to proceed. Now we should see that have been created many new subsections under the SQL Server Agent node.
- Then we need to ceate new SQL server credentials. In order to do so, we need to open a new query on our server and, then copy and execute the following code (make sure to replace all the needed information):
\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CREATE CREDENTIAL [YourCredentialName]
\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;WITH IDENTITY = '[Your Azure Storage Account Name]',
\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SECRET = 'Access Key';
- Finally, again in the Object Explorer, we expand the managment node and then right clik on Maintenance Plans, and select Maintenance Plan Wizard. By following the wizard we can change the planned backup, which is not scheduled by default, and we can set up it to be, for example, weekly. Then we can decide the type of backup we want to be performed (let's say a Full Backup). Then by navigating the Back Up Database Task dialog, we can choose URL as destination and put the URL of the container on the storage account the has been created in the previous milestone. When we have finished the guided procedure, we should see a new node under the Maintanance Plan subsection. If we right click on this node and press execute, we should be able to execute a backup that will be added to our storage account on Azure.

## Milestone 5
This milestone is a disaster recovery simulation.

- The first thing to do is mimiching a dataloss on the production database. This means opening a query on SSMS and deliberating cancel or modify some data in some table. 
After having done so, we can check on Azure Data Studio that we have actually made on of those changing (we just need to query the affected tables).
- Now we can restore our database. If we go on the Azure SQL Database dashboard, we can locate and select the database that needs to be restored, click on it, and when we are on the SQL Database Homepage, click on Restore. Now a page will open and we will be able to choose to restore the databse from an exact point in time, and to give a name to the new restored database. Then we can establish a connection on Azure Data Studio, and then verifing that the new database contains all the data we had lost. 
Finally, we can delete the old damaged database.