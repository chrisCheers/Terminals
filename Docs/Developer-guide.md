# Developer guide
## Continuous integration
* There are a private Team City and SonarQube servers configured for our project 
* As a project developer ask for developer access
* These builds shouldn't be used as public available development builds (beta or RC)

## How to configure environment
* Install Visual Studio, With Database tools, Install Wix, Resharper.
* Configure the database server to accept the database connection string in test project app.config file.
* Run chocolatey script Build\installprerequisities.ps1 to configure development tools.

# Application life cycle
* To publish release version:
  * the application version should be updated in Setup project and in Common.AsssemblyInfo.cs
  * Use build.ps1 script in build directory and publish both generated files
  * Update related build version in Team City nightly builds
  * Keep in mind, that there is new version detected by published release on CodePlex and compared with the build date of the application =&gt; Pulish date has to older or equals to the build date.
  * Mark release with label. If you want to rollback to previous version load selected version release label
* TFS Tasks have iteration path &quot;Terminals&quot; until there is at least one related checkin. Then change the iteration path to the current development version
* When fixing an issue, mark it as Fixed (not as Closed). Task is closed as a part of release, if there are no comments otherwise it is shifted to next version.

## Cooperation rules
* Miguel de Icaza has a good post on [Open Source Contribution Etiquette](http://tirania.org/blog/archive/2010/Dec-31.html)
 that is worth reading, as the guidance he gives applies well to Terminals (inspired by Nuget project).
* Pickup from task stack by Votings
* Select only task, which you are able to solve in no more than two months
* Don't keep your checkouts long time, use Shelve sets instead
* Always associate check-in change set with task, if your check in is related to it
* In case of formatting make two separate checkins: one which holds only code formatting changes, second with fix/feature changes

## Project structure
* Current development tool is Visual Studio 2015 with .NET C#.
* The main solution project is configured to target .NET framework 4. But there are other external projects, which are still targeting .NET 2.0. Don't change the target framework for them.
* Download both Resources and Source folders from the Main branch to the same location. Terminals solution references libraries and images from Resources directory.
* For Logging the Log4Net is configured. Log files are stored under application Logs subdirectory.
* To build the release setup use the &quot;Distribution release&quot; solution configuration. For general development use standard debug and release.
* Output directory is default directory under the Terminals project.
* Put all localize able resources under the Localization directory in resource file stored there.
* The latest development branch is stored under &quot;Main\Sources&quot;
* To create user branch create your own under &quot;FeatureBranches&quot; directory and branch always whole main subtree
* All external components and other resources like images should be stored under &quot;Resources&quot; directory in its branch

## External components
* SSH protocol from Granados project (actually developed as [Poderosa](http://sourceforge.net/projects/poderosa/))
* Terminal emulator control for ssh and telnet [Terminal control](http://www.codeproject.com/KB/IP/Terminal_Control_Project.aspx)
* Amazon S3 component from Amazon SDK [Amazon SDK for .NET](http://aws.amazon.com/sdkfornet/)
* zLib compression library [zLib](http://www.componentace.com/)
* VNC client library [VncSharp](http://cdot.senecac.on.ca/projects/vncsharp/)
* [Log4Net](http://logging.apache.org/log4net/)
* Packet manipulation library [SharpPcap](http://www.tamirgal.com/blog/page/SharpPcap.aspx)
* ICA Citrix component
* RDP imported as activeX component
* Setup WIX toolkit [WIX Toolkit](http://WixToolSet.org)
* PowerShell comunity extensions
* Chocolatey to download external dependecies in build

## Coding rules
* Use Visual Studio 2015 default settings or similar settings in another editor.
* For developer who are using Resharper, there is a Team shared configuration file for coding rules. Don't change this file, if you want to apply some rules. Discuss it first within the team.
* Indents are 4 spaces. You can use Productivity Power Tools for VS to convert tab characters into spaces.
* Fields should be declared private and above all methods.
* Put curly brackets on a new line and close it in the same indentation.
* Keep classes small up to maximum 500 lines
* Keep methods small up to maximum 35 lines
* Use usings as much as possible and remove not used usings 
* members order: constantsand statics, fields, properties, events, constructors, methods.
* When using an if condition with one statement, put the statement on the next line.

```cs
 if (true)
       DoSomething();
```

* When using an if condition with one statement after if condition and else condition, curly brackets are optional.

```cs
if(true)
       DoSomething();
   else
       DoSomethingElse();
```

* When using an if condition with curly brackets, use curly brackets for all attached conditions

```cs
   if (true)
   {
       x++;
       DoSomething();
   }
   else
   {
       DoSomethingElse();
   }
```

* After an if, while, for each or other conditions that can use curly brackets, leave an empty line.

```cs
if (true)
      DoSomething();

x++;
foreach(String s in stringArray)
{
    Debug.Print(s);
}
   
   DoTheNextThing();
```

* Use String.Format when possible.
* Use String.Empty instead of "", use String.IsNullOrEmpty() instead of (x == null | x = "").
