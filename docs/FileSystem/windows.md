# Windows Local Filesystem

The Windows Local Filesystem support is implemented by using the AlphaFS .NET Library, which provides advanced NTFS features over the standard libraries in .NET.  Details of AlphaFS can be found at [http://alphafs.alphaleonis.com](http://alphafs.alphaleonis.com).

## UrlFormat

Below is the expected format for Windows Local Filesystem files in the Zephyr.FileSystem.

|Type|Format|Example
|----|------|-------
|File|[drive]:\\[path]\\[file]|C:\Temp\Dir001\SubDir001\file.txt
|Directory|[drive]:\\[path]\\|C:\Temp\Dir001\SubDir001\

**Note**: All directories must end in a slash.  Click [here](utilities.md#directory-vs-file-url) for more details.

## Client

A client is a generic term for an object any given implementation needs to sucessfully complete its tasks.   This is usually completely containted within the implemenation classes themselves, exposed through their constructors, but the Utilities classes, which parse URL's and return the cooresponding implementation objects will need these clients as well.

No client is needed to connect to the Windows Local Filesystem.

# Windows Network Filesystem

The Windows Network Filesystem support is implemented by using the AlphaFS .NET Library, which provides advanced NTFS features over the standard libraries in .NET.  Details of AlphaFS can be found at [http://alphafs.alphaleonis.com](http://alphafs.alphaleonis.com).

## UrlFormat

Below is the expected format for Windows Network Filesystem files in the Zephyr.FileSystem.

|Type|Format|Example
|----|------|-------
|File|\\\\[server]\\[share]\\[path]\\[file]|\\\\localhost\\c$\\Temp\\Dir001\\SubDir001\\file.txt
|Directory|\\\\[server]\\[share]\\[path]\\|\\\\localhost\\c$\\Temp\\Dir001\\SubDir001\\

**Note**: All directories must end in a slash.  Click [here](utilities.md#directory-vs-file-url) for more details.

## Client

A client is a generic term for an object any given implementation needs to sucessfully complete its tasks.   This is usually completely containted within the implemenation classes themselves, exposed through their constructors, but the Utilities classes, which parse URL's and return the cooresponding implementation objects will need these clients as well.

No client is needed to connect to the Windows Network Filesystem.

