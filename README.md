# Exchange Online Log Pull Console Application

Exchange Online Log Pull Console Application is an application written in C# designed to use the [ISO Log Pull Library](https://github.com/murchisd/ISOLogPullLibrary) to interact with the Office 365 Management API. The project was started as a way to re-establish logging user access for Exchange after a migration from an on-premises Exchange Server to Exchange Online.

There are 3 means to access logs in Exchange Online. 

* The web based security and compliance user interface
* Powershell scripts
* Office 365 Management API

The Office 365 Management API is the only means that will scale, and should be considered to be the primary way to retrieve activity logs for online Office 365 services.

This application provides functionality to  

* Authenticate to the Microsoft Management Activty API 
* Start, Stop, and List subscriptions
* Retrieve available activity log content

## Prerequisites

* [ISOLogPullLibrary.dll](https://github.com/murchisd/ISOLogPullLibrary)
* Office 365 tenant admin account
* X.509 certificate

## Running Application

Clone the repository with
```
git clone https://github.com/murchisd/ExchangeOnlineLogPull.git
```

You will need to have a copy of the [ISOLogPullLibrary.dll](https://github.com/murchisd/ISOLogPullLibrary) on your computer.

Follow the README [here](https://github.com/murchisd/ISOLogPullLibrary/blob/master/README.md) to set up and test the library.

Open the "ExchangeOnlineLogPull.sln" in visual studios

* Right-click "ExchangeOnlineLogPull" in Solution Explorer, select "Add Reference", click "Browse" at the bottom right, select the ISOLogPullLibrary.dll.
* Right-click "Solution" in Solution Explorer, select "Build Solution".

The application should be able to run but you will still need to set up the AppOptions.config file.

If you followed the README steps, to test ISOLogPullLibrary.dll, the AppOptions.config file can be copied from the TestApplication.exe directory.

Otherwise, The easiest way to fill out this file is to run the executable which will create the file with default settings stored in file.

In a command prompt, navigate to your build folder (e.g. ExchangeOnlineLogPull\bin\Debug), then run "ExchangeOnlineLogPull.exe".

Open the AppOptions.config file in a text editor and add the values for any blank fields. The file should appear similar to below:

aadinstance=https://login.microsoftonline.com    
tenant=   
clientid=   
tenantid=   
certthumbprint=   
resourceid=https://manage.office.com   
subscriptiontype=exchange   
tempfolder=C:\Users\<current_user>\AppData\Local\Temp\

## Usage

The "help" argument can be passed on the command line for a detailed description of the application usage and arguments.
```
ExchangeOnlineLogPull.exe -help
```

Basic usage does not require any arguments to passed on the command line. The default config file settings will be used.
```
ExchangeOnlineLogPull.exe
```

Specify settings on command line
```
ExchangeOnlineLogPull.exe starttime=YYYY-MM-DD endtime=YYYY-MM-DD subtype=Exchange output=C:\Users\ExampleUser\Desktop\outputfile
```

## Arguments

StartTime: Optional datetime (UTC) indicating time range of content to return   
		   Must also specify EndTime   
		   
           Acceptable Formats:	YYYY-MM-DD   
					YYYY-MM-DDTHH:MM   
					YYYY-MM-DDTHH:MM:SS   
							   
EndTime:   Optional datetime (UTC) indicating time range of content to return   
           Must also specify StartTime   
		   
           Acceptable Formats:	YYYY-MM-DD   
					YYYY-MM-DDTHH:MM   
					YYYY-MM-DDTHH:MM:SS   
							   
SubType:   Optional argument to specify which SubscriptionType to use   
           This does not change default SubscriptionType in config file   
		   *Case Sensitive - Acceptable parameters below   
		   
           Acceptable Types:
				AzureActiveDirectory
				Exchange   
				SharePoint   
				General

Output:    Optional output argument to specify output file path and name    
           Application will automatically append .csv extension to file name;   
		   Default output path is current directory;   
		   Default file name is current directory;   
		   
           Example: output=C:\Users\ExampleUser\Desktop\outputfile   
		   
Start:     Optional argument to start a subscription   
           If no subtype is specified it will start the default subscription specified in appOptions.config   
		   If no subscription type is specified in config default is Exchange;    
		   
           Example: start subtype=SharePoint   
		   
Stop:      Optional argument to stop a subscription    
           If no subtype is specified it will stop the default subscription specified in appOptions.config

           Example: stop subtype=SharePoint

List:      Optional argument to list all subscriptions

		Example: list subtype=SharePoint

		The application will only execute one action argument.   
		Priority - list, start, stop