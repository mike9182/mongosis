# Mongosis : Custom SSIS Data Source and Connection Manager for Mongo DB

This is the Custom Adapter for Microsoft's SSIS to connect to MongoDB, licensed under the MIT license.

<div style="margin-left:auto;margin-right:auto;width:300px;"><img src="http://tech.simplybusiness.co.uk/wp-content/uploads/2012/06/Mongosis3Smaller.png" alt="Mongosis logo"></div>

## Dependencies

This data source depends on the C# MongoDB drivers found at:

https://github.com/mongodb/mongo-csharp-driver/downloads

### Notes:

* The latest version of Mongosis (1.7.0) requires the 1.7.0 version of the C# MongoDB drivers.
* From version 1.5.0 of the C# MongoDB drivers and on, the installer does not load the drivers into the Global Assembly Cache (GAC).
    * Run gacutil.exe with '/iF' option to load the C# MongoDB driver DLLs in to the GAC.
    * gacutil.exe can be found here: C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\bin\NETFX 4.0 Tools\gacutil.exe

The unit tests depend on the .NET mocking framework 'JustMock Lite' available at:

http://www.telerik.com/freemocking.aspx

## Deployment

Mongosis has an installer which makes deployment very easy, however if you wish to deploy manually then do the following:

* Copy DLL file to:
	* C:\Program Files (x86)\Microsoft SQL Server\110\DTS\PiplineComponents
	* C:\Program Files (x86)\Microsoft SQL Server\110\DTS\Connections
* Run gacutil.exe with '/iF' option to load DLL in to the Global Assembly Cache, util is found at:
	* C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\bin\NETFX 4.0 Tools\gacutil.exe

## Usage

1. In SQL Server Data Tools, create a 'Business Intelligence > Integration Services' project.
2. Create a MongoDB Connection Manager in the Connection Managers area.
3. Set the database, server, username and password values in the Component Properties
4. Drag a MongoDB Data Source in to the Data Flow area
5. Right click the MongoDB data source and click Edit
6. Under Connection Managers, ensure that MongoDBConnectionManager is selected.
7. In the Component Properties tab, enter the name of the Mongo collection you wish to pull data from in the 'CollectionName' custom property.
8. Under Input and Output properties, verify that all the expected fields are listed under Output - Output Columns
9. Clicking onto each Output Column, ensure that the DataType field is correct
10. Drag your data destination to the Data Flow area
11. Click on the MongoDB Data Source and create a connection to the destination.
12. Select 'Start Debugging' (key F5) to begin the data flow process.

## Upgrading existing SSIS packages to the latest version of Mongosis

1. Uninstall any existing versions of Mongosis
1. Install the latest version of Mongosis
1. Ensure you have the appropriate version of the C# MongoDB drivers installed (1.7.0 is the current compatible version)
1. In a text editor, open up the Package.dtsx file for the SSIS package:
  1. Find the connection manager entry with the name: "MongoDBConnectionManager" and add the following object data after the "SlaveOk" entry:
```
<Ssl
  Type="11"
  Value="0" />
```
  2. Find any property entries that specify a version for "MongoSsisDataSource" and set that to match the version number of Mongosis to which you are upgrading (i.e. 1.7.0.0)
  3. Save the changes
1. Restart Visual Studio in order to complete the upgrade

## Notes:

* Ensure that 'Run64BitRuntime' option in SSIS project Configuration/Debugging properties is set to 'False'.
* If you're running the 32-bit version of Windows there is no 'Program Files(x86)' so deploy to folders under 'Program Files' instead.

## Contact:

The contact for this project is <a href="mailto:mongosis@simplybusiness.co.uk">mongosis@simplybusiness.co.uk</a>
