# MVC 4 Music Store

## For web site administration tool (For StoreManagerController class [Authorize(Roles = "Administrator")] part need this section) 
go to this location and open command prompt here : ```cd C:\Program Files (x86)\IIS Express```

### Note: Your project IIS hosting or IIS Express hosting port here,find project vs project properties mine is:50345
```iisexpress.exe /path:C:\Windows\Microsoft.NET\Framework\v4.0.30319\ASP.NETWebAdminFiles /vpath:"/webdev" /port:50345 /clr:4.0 /ntlm```

```http://localhost:50345/webdev/default.aspx?applicationPhysicalPath=E:\ProjectPractise_GrowRich\MVC4SampleApp\MvcMusicStore&applicationUrl=/AppAdmin```

## ASP.NET Website Administration Tool (WSAT) common problems 
#### Problem: Role Manager is Not Enabled
#### Solution: Open your web.config file and ensure the role manager is enabled.

Add or modify the following section:
```
<system.web>
  <authentication mode="Forms" />
  <roleManager enabled="true" />
</system.web>
```

#### Problem: ASP.NET Membership Database is Missing
#### Solution: Run the ASP.NET database setup tool to create the required membership tables:

Open Command Prompt and navigate to:
```cd C:\Windows\Microsoft.NET\Framework\v4.0.30319```
Run:
```aspnet_regsql.exe```
Follow the wizard to configure the ASP.NET membership database.

#### Problem: The Database is Not Properly Configured in WSAT
#### Solution: In WSAT, click the "Choose a new data store" button and ensure it points to the correct database.

#### Problem: I have followed all of your advice. but it says the same problem , additionally it says "The following message may help in diagnosing the problem: Access to the path 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\appadmin\f11ba3f2\4b28c3cb\hash' is denied."
#### Solution: 
1. Grant Full Permissions to the Temporary ASP.NET Files Folder
Navigate to the Folder
Open File Explorer and go to:
```C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files```
Right-click on the "Temporary ASP.NET Files" folder → Select Properties.
Go to the Security tab → Click Edit → Click Add.
Add the required users:
Enter Everyone and click OK.
Add IIS_IUSRS (for IIS applications).
Add NETWORK SERVICE (if your app runs under Network Service).
Add Users and Authenticated Users.
Give "Full Control" to these users and click OK.

2️. Clear the Temporary ASP.NET Files
Close Visual Studio or any running ASP.NET applications.
Manually delete all files from:
```C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files```
and
```C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Temporary ASP.NET Files```
Restart your computer or restart IIS by running:
```iisreset```
in Command Prompt (Run as Administrator).

3️. Run Visual Studio as Administrator
Right-click on Visual Studio and select Run as administrator.
Then try launching the ASP.NET Website Administration Tool (WSAT) again.

4️. Ensure Your Application Pool Has Correct Identity
Open IIS Manager (inetmgr).
Go to Application Pools.
Find your App Pool and check its Identity (e.g., ApplicationPoolIdentity, NetworkService).
Ensure that identity has read/write permissions to ```C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files.```

## For code first migration 

1. If you use this line in Global.asax.cs file in application_start method: 
```Csharp
System.Data.Entity.Database.SetInitializer(new DropCreateDatabaseIfModelChanges<MvcMusicStore.Models.MusicStoreEntities>());
or System.Data.Entity.Database.SetInitializer(new MvcMusicStore.Models.SampleData());
```
then you don't need to execute the command like: ```Enable-Migrations, Add-Migration InitialCreate, Update-Database.```

2. But if you don't want that and comment out that line of code, then first delete your existing database then you have to exceute in Package Manager Console:```Enable-Migrations, Add-Migration InitialCreate, Update-Database.```
3. But
 * If you have data in your database and Initial_Migration file in Migration folder then you don't need to delete it and then you have to execute in Package Manager Console:```Enable-Migrations,  Update-Database```.  <mark>you can use -Force if you want to create database forcefully.</mark>
* if in your Configuration class, constructor has this line, then you don't need to excute : ```Enable-Migrations, Just Add-Migration InitialCreate, Update-Database.```
```CSharp
public Configuration()
    {
        AutomaticMigrationsEnabled = true;  // Enables automatic migrations
        AutomaticMigrationDataLossAllowed = false; // Prevents accidental data loss
    }
```
* If you have a Migration folder with an Initial_Create file, then just ```Update-Database``` will work. So, for this project code, only ```Update-Database``` will work.
4. After verification if your database has _MigrationHistory table, then for the new changes you can use ```Add-Migration NewChanges(any name) and Update-Database.``` use ```Update-Database -Script``` to see the script before applying it to the database. or ```Update-Database -Force``` to recreate the database.
5. Finally you fill out the seed method in Configuration class. Then you can get your filled data in the database as well.

[Going Top](#mvc-4-music-store) 


