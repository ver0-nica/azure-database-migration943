# azure-database-migration943

The beginning of this project consists in seting up the correct work enviroment. So, after setting up this repo, the first stage is creating a Production Enviroment: 
- The first step is to create a virtual machine on Azure, and making sure the image of the VM is set up to allow the RDP protocol. In this way we'll be able to connect to the VM by using Windows Remote Desktop.
- Afterthat we have to make sure that SQL Server and SQL Server Management Studio (SSMS) are installed on the VM, and if not we can istall them by using the Microsoft Download Center.
- Now we can download (the dowload was given) and restore the AdventureWorks database on SSMS, and in this way, we'll finally replicate an authentic production database scenario.
