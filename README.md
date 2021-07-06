# csvToGpx

## Introduction

The **csvToGpx** program converts CSV files containing custom GPS waypoints to Garmin GPX files which are compatible with the Garmin BaseCamp program and Garmin GPS devices.
The **csvToGPX** is very flexible regarding the format of the input CSV files. You can specify the format of the CSV records (field order and number of fields).

As an option, the **csvToGpx** program can write the generated custom waypoints GPX files and any custom waypoint symbol image files you may have to your Garmin GPS device.
Refer to the [Custom Waypoint Symbols](#custom-waypoint-symbols) section below in this document for more details on using custom waypoint symbols.

If you are using Garmin BaseCamp you will need to manually import the generated Garmin GPX files inside Garmin BaseCamp as the **csvToGpx** program cannot automatically import the generated custom waypoints into Garmin BaseCamp.
However the **csvToGpx** program can automatically import any custom waypoint symbol image files you may have. Refer to the [-garmin-basecamp](#-garmin-basecamp) command argument in the Syntax section below in this document.

The **csvToGpx** program has been tested on Linux, macOS, and Windows. Garmin BaseCamp does not run on Linux therefore the ability to automatically import custom waypoint symbols images files is not applicable when running on Linux.

## CSV file format

The default format of an input CSV file is defined by the exactly named comma separated fields below in this order:

    "latitude,longitude,name,symbol,description,street,city,state,zipcode,country,phonenumber,proximity"

If your input CSV file does not match that format you can specify the format. Refer to the [-csv-fields](#-csv-fields-comma-separated-list-of-csv-fields) command argument in the Syntax section below in this document. 

## Syntax

```bash
csvToGpx -csv-file <input CSV file>
        [-csv-fields <comma separated list of CSV fields>]
        [-default-symbol <name|000-055>]
        [-custom-symbols-directory <directory>]   
        [-default-country <country>] 
        [-default-proximity <0>]	
        [-garmin-basecamp] 
        [-garmin-device <Garmin Device mount point>]
        [-author] <author of the waypoints file>]
        [-email] <email address>]
        [-suppress-warnings]
        [-help]
```
The only required argument is [-csv-file](#-csv-file-input-csv-file)

### Arguments

#### -csv-file "\<input CSV file\>"

Specifies the input custom waypoints CSV file to be converted to a GPX formatted output file. 
*The input CSV file is not modified.*

You can specify One or more CSV files separated by a comma.
You can also specify a wildcard file pattern which needs to be enclosed in double quotes (e.g "*.csv").

The generated GPX file(s) are created in the same directory as the CSV file(s).
A generated GPX file will have the same filename as the input CSV file but with the file extension of "gpx".
If a GPX file already exists it will be overwritten and replaced.

The **csvToGpx** program can automatically copy the GPX files to your Garmin GPS device if it is supported 
and connected to your personal computer. Refer to the [-garmin-device](#-garmin-device) command argument below in this document.

#### -csv-fields "\<comma separated list of CSV fields\>"

Specifies the format of the CSV file records. If not specified then the default is:

```csv
"latitude,longitude,name,symbol,description,street,city,state,zipcode,country,phonenumber,proximity"
```

If your input CSV file does not match the default format you must provide a comma separated list of fields (exact names) which match the fields and their order in the CSV file records. Minimally you must specify **latitude**, **longitude** and **name**.
        
If any fields in your CSV file have comma's then enclose the entire field in double quotes. 

#### -garmin-device

Specifies the path for the Garmin GPS device or it's Micro SD memory card.
The **csvToGpx** program will copy the custom waypoints GPX files and any custom waypoint symbol image files to your Garmin GPS Device or it's Micro SD memory card.
The Garmin GPS Device or it's Micro SD Memory card must be connected to your system. If running on Linux or macOS you can display the mount point by running a `df -h` command
from a Linux or macOS command prompt. If running on Windows the Garmin GPS Device or it's Micro SD memory card shows up as a drive letter which is visible with Windows Explorer.

##### Example for Linux:
```
    /mnt/garmin       
```   
##### Example for macOS:
```
    /Volumes/GARMIN   
```     
##### Example for Windows:
```
    F:\              
```

#### -garmin-basecamp

Specifies to copy any custom waypoint symbol image files to the appropriate Garmin BaseCamp **CustomSymbols** directory on macOS and Windows.
You must also specify the [-custom-symbols-directory](#-custom-symbols-directory-directory) command argument.

#### -default-symbol "\<name|000-055\>"

Specifies a waypoint symbol name or symbol number (for a custom symbol) to be configured for any CSV file waypoint that does not specify a symbol.
Optional. If not specified the value for [-default-symbol](#-default-symbol-name000-055) is "Waypoint" which displays as a dot.

If you want to use a Garmin Symbol you specify the symbol name (e.g. "Campground", "Circle with X", "Gas Station", "Park", "Restaurant").
Refer to the following online document [Garmin GPS Unit Waypoint Icons Table](https://freegeographytools.com/2008/garmin-gps-unit-waypoint-icons-table) for a table of Garmin Symbol names you can use.  *Note: Not all devices have all Garmin Symbol names built it.*

If you want to use your own Custom symbol you specify the symbol number (000-055) and you must also provide the symbol image file. 

#### -custom-symbols-directory "\<directory\>"

Specifies a directory where your custom waypoint symbol image files reside.
If you specify this command argument along with the [-garmin-basecamp](#-garmin-basecamp) command argument then the custom waypoint symbol image files will be copied to the appropriate Garmin BaseCamp **CustomSymbols** directory when running on macOS or Windows. If the [-garmin-device](#-garmin-device) command argument is specified then the custom waypoint symbol image files will also be copied to your Garmin GPS device. Refer to the [Custom Waypoint Symbols](#custom-waypoint-symbols) section below in this document for more details on using custom waypoint symbols.

#### -default-country "\<country name\>"

Specifies an optional default country name to be used for any CSV file custom waypoints that do not specify a waypoint country.

#### -default-proximity "\<proximity radius alarm value\>"

Specifies an optional default proximity radius alarm value in meters to be used for any CSV file custom waypoints that do not specify a waypoint proximity.

#### -author

Specifies the Author for the Waypoints File(s). The Author will be set in the GPX file's metadata.

#### -email

Specifies a contact Email address for the Waypoints File(s). The Email address will be set in the GPX file's metadata.

#### -suppress-warnings

The **csvToGpx** program makes an attempt to perform validation on the City, State, Zip Code, Country and Phone Number.
The validation checking is not 100% foolproof. Warnings messages may be generated for invalid values in those fields which might actually be valid values.
The messages are warnings and the data for those fields will still be used to create the custom Waypoints.
You should inspect the CSV Waypoint records identified in the warning messages to see if your data is valid.
If desired you can ignore the warnings and suppress the warning messages by specifying the -suppress-warnings command argument.

#### -help

Displays the command help.

## Custom Waypoint Symbols

### Garmin Waypoint Symbols

Garmin provides over 150+ Waypoint Symbol Icons you can use for your Waypoints. All you need to do is to locate the Garmin Waypoint Symbol Icon you would like to use and specify it's name in the **symbol** field in your CSV Waypoint field. 
For example: **Campground**

When you create a Waypoint inside Garmin BaseCamp you can select the Garmin Waypoint Symbol Icon. If you leave the mouse pointer over a Garmin Waypoint Symbol Icon for a few seconds the name will be displayed as "hover help". In this case the *tent* Garmin Waypoint Symbol Icon's name is **Campground**.

<img style="border-style:solid;border-width: 2px;" src="./images/Garmin_Symbol.png"/>

Waypoint defined in a CSV File using the **Campground** Waypoint Symbol Icon (4th comma separated field). 
```csv
40.378513,-105.852245,"Timber Creek Campground",Campground,"Rocky Mountains Campground","Trail Ridge Road","Grand Lake","Colorado","80447","USA","970-586-1206"
```
Refer to the following online document [Garmin GPS Unit Waypoint Icons Table](https://freegeographytools.com/2008/garmin-gps-unit-waypoint-icons-table) for a table of Garmin Symbol names you can use. A lot easier than locating a Garmin Symbol icon inside Garmin BaseCamp. *Note: Not all Garmin GPS devices have all Garmin Symbol names built it.*

### Using Custom Waypoint Symbols

If you cannot find an appropriate Garmin Waypoint Symbol Icon for your custom waypoints you can provide and use a custom waypoint symbol icon. There are some websites that have custom waypoint symbol icons that you can download for free.
[POI Factory](http://www.poi-factory.com) is one site. Note: I have used some custom waypoint symbol icons from sites such as [POI Factory](http://www.poi-factory.com) and I had to modify the icon images in order for
them to show up in Garmin BaseCamp and on my Garmin GPS device. 
The symbol image icon file must be a bitmap image and must meet specific criteria in order for the custom symbol image to appear with your custom waypoints in Garmin BaseCamp and on your Garmin GPS Device.
Refer to the Garmin online document [Custom Waypoint Symbol Creation](https://support.garmin.com/en-US/?faq=VTS8XTdjCW5Tx3HyfJ3eQ6) for details on custom waypoint symbols and how to create custom symbol icon image files.

Custom symbol icon image files are stored in Garmin BaseCamp and on Garmin GPS devices with different filenames.
In Garmin BaseCamp the custom symbol icon image files need to have filenames with 3 digit numbers and must be bitmap image files with a file extension of ".bmp".
For example: "**000.bmp**", "**001.bmp**", "**002.bmp**", "**003.bmp**", etc. etc.  
   
On your Garmin GPS device those same custom symbol icon image filenames must be named "**Custom 0.bmp**", "**Custom 1.bmp**", "**Custom 2.bmp**", "**Custom 3.bmp**", etc. etc.

The **csvToGpx** program will copy your custom waypoint symbols to Garmin BaseCamp and to your Garmin GPS device for you automatically as an option.
You do not have to provide 2 copies of every custom symbol image file ("**000.bmp**" and "**Custom 0.bmp**"). You just need to provide the filename named "**000.bmp**" 

To make the **csvToGpx** program copy your custom waypoints to Garmin BaseCamp, specify the [-garmin-basecamp](#-garmin-basecamp) command argument and also specify the [-custom-symbols-directory](#-custom-symbols-directory-directory) command argument with the directory where your custom waypoint symbols are stored. The **csvToGpx** program will copy the files ("**000.bmp**", "**001.bmp**", "**002.bmp**", "**003.bmp**", etc. etc.) to the appropriate Garmin BaseCamp directories on macOS and Windows so they will be visible and available for use in Garmin BaseCamp.

To make the **csvToGpx** program copy your custom waypoints to your Garmin GPS device then specify the [-custom-symbols-directory](#-custom-symbols-directory-directory) command argument and the [-garmin-device](#-garmin-device) argument with the path to your Garmin GPS device.  The **csvToGpx** program will copy the files ("000.bmp", "001.bmp", "002.bmp", "003.bmp", etc. etc.) to the appropriate directory on your Garmin GPS Device (or Micro SD Card) as ("**Custom 0.bmp**", "**Custom 1.bmp**", "**Custom 2.bmp**", etc. etc.) so they will be visible and available for use on your Garmin GPS device. 
        
### Adding a description to Custom Waypoints using Custom Symbols

You can add a "description" after the 3 digit number in a custom symbol icon image filename. 
The **csvToGpx** program will automatically include that "description" in the "Comment" section (1st line) of any custom waypoint that uses that custom symbol.
That "description" will show up in Garmin BaseCamp and your Garmin GPS device in the "Comment" section when you view details on a custom waypoint using that custom symbol.
The **csvToGpx** program will not include that "description" in the custom symbol icon image filename when copying the custom symbol icon image file to Garmin BaseCamp or your Garmin GPS Device.
The "description" needs to be prefix with a dash/minus sign (-) and must immediately follow the 3 digit number in the custom symbol icon image filename.
I myself use that "description" to give a custom waypoint symbol some additional details. 
For instance I have a custom waypoint symbol image for a National Park. It is named "**031-National Park.bmp**".  The text "**National Park**" will be added to the Comment section of any waypoint using the symbol **031**. 

The **csvToGpx** program also adds some additional information in the comment section of a custom waypoints. Refer to the [Waypoint Comment](#waypoint-comment) section below in this document for more details..        

## Waypoint Comment

The **csvToGpx** program generates a detailed "Comment" section for your custom waypoints which can be viewed in Garmin BaseCamp and from your Garmin GPS device. 

The "Comment" will contain the following information (if available) on separate lines for a custom waypoint:

* The custom "description" retrieved from a custom symbol image filename if the waypoint is using a custom symbol and the custom symbol image file contained a "description". Refer to the [Adding a description to Custom Waypoints using Custom Symbols](#adding-a-description-to-custom-waypoints-using-custom-symbols) section above in this document.

* The description value from the CSV file for the custom waypoint if provided.

* The address from the CSV file for the custom waypoint if provided. 
  
    * Street address value from the CSV file for the custom waypoint if provided.
    * City, State, ZipCode and Country from their values from the CSV file for the custom waypoint if provided.
    * Phone number from the CSV file for the custom waypoint if provided.

    The address is always available in Garmin BaseCamp and does not have to placed in the Waypoint Comment field.
    However on some Garmin GPS devices (hand held) the address is not displayed. 
    By placing the address in the Waypoint Comment field it is viewable from the Garmin GPS device when viewing information on a custom waypoint.     

#### Example of a custom waypoint created by the csvToGpxX program and displayed in Garmin BaseCamp showing the Comment field.

The waypoint is using the custom waypoint symbol image "**031-National Park.bmp**".

![031-National Park.bmp](./images/031-National_Park.png "National Park Custom Symbol# 031")


The CSV custom waypoint record looked like this below.  I specified the custom waypoint symbol number **031** in the 4th comma separated field.

```csv
35.251530,-75.528327,"Cape Hatteras Visitor Center",031,"Visitor Center","46375 Lighthouse Rd","Buxton","North Carolina","27920","USA","252-473-2111"
```

The GPX generated by the **csvToGpx** program and loaded into Garmin BaseCamp and written to the Garmin GPS looked like this below. Note: the **`&#10;`** values in the Comment **`<cmt>`** section which are "line breaks" added by the **csvToGpx** program that will separate the fields on different lines when displayed in Garmin BaseCamp and Garmin Devices.

**Garmin GPX**
```xml
<wpt lat="35.251530" lon="-75.528327">
	<time>2021-07-06T16:39:02Z</time>
	<name>Cape Hatteras Visitor Center</name>
	<cmt>National Park&#10;Visitor Center&#10;46375 Lighthouse Rd&#10;Buxton, North Carolina 27920 USA &#10;252-473-2111</cmt>
	<desc>Visitor Center</desc>
	<sym>Custom 31</sym>
	<type>user</type>
	<extensions>
		<gpxx:WaypointExtension>
			<gpxx:DisplayMode>SymbolAndName</gpxx:DisplayMode>
			<gpxx:Address>
				<gpxx:StreetAddress>46375 Lighthouse Rd</gpxx:StreetAddress>
				<gpxx:City>Buxton</gpxx:City>
				<gpxx:State>North Carolina</gpxx:State>
				<gpxx:Country>USA</gpxx:Country>
				<gpxx:PostalCode>27920</gpxx:PostalCode>
			</gpxx:Address>
			<gpxx:PhoneNumber>252-473-2111</gpxx:PhoneNumber>
		</gpxx:WaypointExtension>
		<wptx1:WaypointExtension>		
			<wptx1:Proximity>10</wptx1:Proximity>
			<wptx1:DisplayMode>SymbolAndName</wptx1:DisplayMode>
			<wptx1:Address>
				<wptx1:StreetAddress>46375 Lighthouse Rd</wptx1:StreetAddress>
				<wptx1:City>Buxton</wptx1:City>
				<wptx1:State>North Carolina</wptx1:State>
				<wptx1:Country>USA</wptx1:Country>
				<wptx1:PostalCode>27920</wptx1:PostalCode>
			</wptx1:Address>
			<wptx1:PhoneNumber>252-473-2111</wptx1:PhoneNumber>
		</wptx1:WaypointExtension>
	</extensions>
</wpt>
```

**Custom Waypoint displayed in Garmin BaseCamp**

<img style="border-style:solid;border-width: 2px;" src="./images/Garmin_BaseCamp_Custom_Waypoint.png"/>

**Custom Waypoint displayed on a Garmin Montana 700 GPS device**

<img style="border-style:solid;border-width: 2px;" src="./images/Garmin_Device_Custom_Waypoint.png" width="400" height="800"/>

## Examples

### Example 1

A Garmin GPX file will be created from the csv-file named **NC_Lighthouses.csv** which contains 6 custom waypoints.
All 6 custom waypoints are using a custom waypoint symbol icon named **025-Lighthouse.bmp**. 

<img style="border-style:solid;border-width: 2px;" src="./images/Example1_Custom_Waypoint_Symbol.png"/>

Here is the input CSV file below. It follows the default csv-fields format. It is using the custom waypoint symbol icon image **25**. 
No need to prefix the symbol in the CSV file a leading zeros to make it 3 digits.

```csv
33.873514,-78.000395,"Lighthouse - Old Baldy",25,"Bald Head Island","101 Light House Wynd","Bald Head Island","NC","28461","USA","910-457-7481"
35.108947,-75.985997,"Lighthouse - Ocracoke",Custom 25,"Ocracoke Island","Lighthouse Road","Ocracoke","North Carolina","27960","USA","252-473-2111"
35.250602,-75.528830,"Lighthouse - Cape Hatteras",Custom 25,"National Seashore","46375 Lighthouse Road","Buxton","North Carolina","27920","USA","252-995-4474"
35.818557,-75.563273,"Lighthouse - Bodie Island",Custom 25,"Pea Island","8210 Bodie Island Lighthouse Road","Nags Head","North Carolina","27959","USA","252-441-5711"
35.908130,-75.668190,"Lighthouse - Roanoke Marshes",Custom 25,"Downtown Manteo","104 Fernando Street","Manteo","North Carolina","27954","USA","252-475-1750"
36.376646,-75.830629,"Lighthouse - Currituck Beach",Custom 25,"Currituck Beach","1160-1210 Corolla Village Rd","Corolla","North Carolina","27927","USA","252-453-4939"
```

Running the **csvToGpx** program to convert the CSV **NC_Lighthouses.csv** file to the GPX file **NC_Lighthouses.gpx**. The **-garmin-basecamp** and **-custom-symbols-directory** command arguments were specified which triggered the **csvToGpx** program to copy any custom waypoint symbol icon images to the Garmin BaseCamp **CustomSymbols** directory.

<img style="border-style:solid;border-width: 2px;" src="./images/Example1_command.png"/>

#### Manually importing the generated Garmin GPX file **NC_Lighthouses.gpx** into Gamin BaseCamp.

<img style="border-style:solid;border-width: 2px;" src="./images/Example1_Import1.png"/>

#### Locating and selecting the Garmin GPX file **NC_Lighthouses.gpx**.

<img style="border-style:solid;border-width: 2px;" src="./images/Example1_Import2.png"/>

#### The Garmin GPX file **NC_Lighthouses.gpx** has been imported into Garmin BaseCamp with the 6 Custom Waypoints.

The filename **NC_Lighthouses** (minus the file extension .gpx) appears in the **My_Collection** along with the 6 custom waypoints.

<img style="border-style:solid;border-width: 2px;" src="./images/Example1_Import3.png"/>



