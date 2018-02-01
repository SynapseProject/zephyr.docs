# Overview

Zephyr.Filesystem library is a set of classes and methods designed to provide file system support across multiple implementation types.

The table below shows all currently supported implementations :

|Implementation|URL Format
|--------------|----------
|[Windows Local Filesystem](windows.md)|C:\Temp\MyDirectory\<br>C:\Temp\MyDirectory\MyFile.txt
|[Windows Network Filesystem](windows.md)|//server/share$/MyDirectory/<br>//server/share$/MyDirectory/MyFile.txt
|[Amazon Simple Storage Service (S3)](amazons3.md)|s3://bucket/mydirectory/<br>s3://bucket/mydirectory/myfile.txt

# Base Functionality

Common functionality for all supported implementations exist in the abstract base classes each implementation is required to extend.  Any methods or properties that are not common across implementations are marked "abstract" and classes which inherit are required to implement these according to the underlying filesystem being implemented.

## ZephyrDirectory

This is the base class for all implementations of a "Directory" object.

### Properties

Below is a list of Properties in a ZephyrDirectory object.  All properties marked "abstract" must be implemented by the inheriting class as appropriate for the filesystem being supported.

|Property|Abstract|Description
|--------|--------|-----------
|FullName|Yes|The full name or URL for the directory.
|Name|Yes|Read only property representing the name of the directory.
|Parent|Yes|Read only property providing the full name or URL of the parent directory.
|Root|Yes|Read only property showing the protocol, drive or share of the directory.<br>(Examples : s3://, C:\, \\localhost\c$)
|Exists|Yes|Read only property to tell if a directory exists.
|IsEmpty|No|Read only property to tell if a directory is empty.

### Methods

Below is a list of Methods in a ZephyrDirectory object.  All methods marked "abstract" must be implemented by the inheriting class as appropriate for the filesystem being supported.

|Method|Abstract|Description
|------|--------|-----------
|Create|Yes|Creates a ZephyrDirectory object.
|Delete|Yes|Deletes a ZephyrDirectory object.
|CreateFile|Yes|Creates a ZephyrFile object of the same implementation type as the ZephyrDiretory calling it.
|CreateDirectory|Yes|Creates a ZephyrDirectory object of the same implementation type as the ZephyrDirectory calling it.
|GetDirectories|Yes|Returns a list of ZephyrDirectories which are direct children of the calling ZephyrDirectory.
|GetFiles|Yes|Returns a list of ZephyrFiles which are direct children of the calling ZephyrDirectory.
|PathCombine|Yes|Combines a list of strings into a path based on the implementation type.
|CopyTo|No|Copies the content of one ZephyrDirectory into another ZephyrDirectory.  This is done by using the base "Stream" property of a ZephyrFile, and the "Create" methods of ZephyrFiles and ZephyrDirectories.  This allows for cross-implementation directory copies.
|MoveTo|No|Moves the content of one ZephyrDirectory into another ZephyrDirectory.  This is done by using the base "Stream" property of a ZephyrFile, and the "Create" methods of ZephyrFiles and ZephyrDirectories.  This allows for cross-implementation directory moves.
|Purge|No|Deletes the contents of the ZephyrDirectory, but leaves the main directory intact.  This is done by using the "Delete" methods of both the ZephyrDirectory and ZephyrFile classes.

## ZephyrFile

This is the base class for all implementations of a "File" object.

### Properties

Below is a list of Properties in a ZephyrFile object.  All properties marked "abstract" must be implemented by the inheriting class as appropriate for the filesystem being supported.

|Property|Abstract|Description
|--------|--------|-----------
|FullName|Yes|The full name or URL for the file.
|Name|Yes|Read only property representing the name of the file.
|Exists|Yes|Read only property to determine if a file exists.
|Stream|Yes|The underlying System.IO.Stream object for the file.
|IsOpen|No|Read only property telling if the ZephyrFile is currently open.  This is done by checking the "CanRead" and "CanWrite" properties of the underlying System.IO.Stream object.
|CanRead|No|Read only property telling if a ZephyrFile is available to read from.  This is done by checking the "CanRead" property of the underlying System.IO.Stream object.
|CanWrite|No|Read only property telling if a ZephyrFile is available to write to.  This is done by checking the "CanWrite" property of the underlying System.IO.Stream object.

### Methods

Below is a list of Methods in a ZephyrFile object.  All methods marked "abstract" must be implemented by the inheriting class as appropriate for the filesystem being supported.

|Method|Abstract|Description
|------|--------|-----------
|Create|Yes|Creates a ZephyrFile object.
|Delete|Yes|Deletes a ZephyrFile object.
|CreateFile|Yes|Creates a ZephyrFile object of the same implementation type as the ZephyrFile calling it.
|CreateDirectory|Yes|Creates a ZephyrDirectory object of the same implementation type as the ZephyrFile calling it.
|Open|Yes|Opens the ZephyrFile (and the underlying Stream object) for Read or Write access.
|Close|Yes|Closes the Zephyrfile (and thd underlying Stream object).
|CopyTo|No|Copies the ZephyrFile to another ZephyrFile, or into a ZephyrDirectory with the same File name.  This is done by using the base "Stream" property of a ZephyrFile.  This allows for cross-implementation directory copies.
|MoveTo|No|Moves the ZephyrFile to another ZephyrFile, or into a ZephyrDirectory with the same File name.  This is done by using the base "Stream" property of a ZephyrFile.  This allows for cross-implementation directory moves.
|Reopen|No|Closes, then opens the current ZephyrFile (and the underlying Stream object) for Read or Write access.
|ReadAllLines|No|Reads all the lines from a ZephyrFile and returns them in a string array.
|ReadAllText|No|Reads entire contents of a ZephyrFile and returns it as a string.
|ReadAllBytes|No|Reads the entire contents of a ZephyrFile and returns it as a byte array.
|WriteAllLines|No|Writes a string array, line by line, into the contents of the ZephyrFile.
|WriteAllText|No|Writes a string as the entire contents of a ZephyrFile.
|WriteAllBytes|No|Writes the contents of a byte array into a ZephyrFile.

# Adding New Implementations

New implementations can be added simply by extending the Abstract classes ZephyrFile.cs and ZephyrDirectory.cs and providing the deatils for the abstract properties and methods.  Below is a check-list of the code base objects that need to be updated or modified to support a new implementation type.

* Determine new "root" or protocol identifier (see "Root or Protocol Format" section below)
* Extend ZephyrDirectory and ZephyrFile classes under seperate folder in "Implementations".
* Implement Abstract properties and methods in each class.
* Add new UrlTypes to Enums.cs (MyImpFile, MyImpDirectory).
* Update "GetUrlType" method in Utilities class.
* Update "GetZephyrFile" method in Utilities class.
* Update "GetZephyrDirectory" method in Utilities class.
* Update Clients.cs with new Client (if necessary)

***Root or Protocol Format*** : There is no set format for what is a proper "root" or "protocol" for a file or directory.  As long as the url beings with your protocol, and it doesn't contains a starting substring of another protocol, anything goes (just make sure your users understand it...  don't go crazy just because you can).

|Root|Valid|Description
|----|-----|-----------
|s1\|\||Yes|Pipes are prefectly fine to delimit the root from the rest of the url.
|s2~|Yes|So are tildes.
|s3:|No|This is a sub-string of an already existing root for AWS S3 (s3://)
|s4://|Yes|Doesn't already exist, so why not.
|s5|Yes|Delimeters aren't even required (although it might get messy).

***Client*** : A client is a generic term for an object any given implementation needs to sucessfully complete its tasks.   This is usually completely containted within the implemenation classes themselves, exposed through their constructors, but the Utilities classes, which parse URL's and return the cooresponding implementation objects will need these clients as well.  
