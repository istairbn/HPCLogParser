HPC Server LogParser
============

An application that parses HPC Server 2012 SP1 + Log Files for Analysis.
Ben.Newton@excelian.com
05/12/2014

This program allows you to parse the logs provided by HPC Server 2012 SP1 or greater.
It was designed to integrate with Logscape, so it can be set to run automatically or manually depending on depth and space requirements. 

System Requirements:
 - HPC Server 2012 SP 1 or greater must be run on the machine (needs the hpctrace command)
 - Java 1.6 or greater installed and configured

 Optional Removal:
  - If you are using this in Logscape (as intended) then you can remove the groovy library from the lib folder.

File List:
 - logparser.groovy = The main script
 - parserRunner.bat = A batch file that will execute it quietly. 
 - lib/parser.properties = The file that deals with the set-up. Will probably need configuring for paths
 - lib/groovy-1.8.2.jar = The groovy library 


Before running for the first time, you'll need to configure parser.properties - found in the lib folder. 
This file controls the actions of the script as well as directory paths. If you wish to use more than one properties file, execute the groovy script with an argument

eg logparser.groovy "./someOther.properties"

Manual Mode. The script guides you through, asking which logs you'd like parsed and to what level. 
Set to runMode 1. 

Automatic Mode. The script requires no user interaction, it uses the settings pre-configured in the properties file.
Set to runMode 0.

You can also choose to have all logs of a certain type, or logs of a certain type based on age.

Properties.

runMode [0,1] - This determines whether it is going to run automatically using the defaults in the file [0] or whether you wish to run it manually and select your requirements as you go [1]. If you're running it as a regular script, set it to 0. 1 is designed for on the fly trouble shooting.

sourcePath [directory] - Where the logs are going to be found. Needs to point to the LogFiles directory of HPC. Please note, you must use forward slashes / instead of backwards otherwise you'll get some interesting errors.

destinationPath [directory] - Where the extracted files should be sent to. Same rules as before regarding /.

storage [0,1,2] Determines how you wish to store the files.
	0 = No Storage. The destination directory is wiped of ALL log files (but not folders or their contents) and the latest files are put in. Do NOT make destinationPath a sensistive directory if you are using this option. -- Designed for debugging and pulling data quickly.
	1 = Once per Day. A subfolder is created based on the date and files are extracted there. If you try and run this again during batch, it will refuse. Designed for batching
	2 = Every Run. A subfolder is created for every run.

defaultLevel = [1,2,3,4] Determines the default level of parsing - that which will be used in run mode 0. 
	1 Error and Critical only
	2 Warning, Error, and Critical only
	3 Info, Warning, Error and Critical only
	4 All: Verbose, Info, Warning, Error, and Critical
 
choiceMethod [1,2] - This determines whether you get all logs of a certain type, or a selection based on age.
	1. Type Selection. The simplest. Pick a type of log and they will all be parsed for you. If you have a lot of logs, this could take a while.
	2. Time Selection. Select the type and the age, the script will only parse the ones that meet those criteria. 

days [0,1,2,3...] Any number. If using method 2, this is the age in days it will look for. 

select1 [mgmt] If using method 1, which files it will search for by default. The choices are:

["1":"mgmt","2":"sche","3":"sdm","4":"diag","5":"rept","6":"nmgr","7":"msvrmbrok","8":"mclt","9":"brok","10":"sdgm","11":"sess"]

		1.  HPC Management Service"
		2.  HPC Job Scheduler Service"
		3.  HPC SDM Store Service"
		4.  HPC Diagnostics Service"
		5.  HPC Reporting Service"
		6.  HPC Node Manager Service"	 
		7.  HPC Monitoring Server Service"	 
		8.  HPC Monitoring Client Service"
		9.  HPC Broker Service"
		10. HPC SOA Diag Mon Service"
		11. HPC Session Service"

select2 [.bin] - The pattern to search for if using dates. .bin will get everything.
