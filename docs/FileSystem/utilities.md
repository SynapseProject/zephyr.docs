# Overview

The utilities class provides a set of static, helper methods that, well help when working with ZephyrFiles and ZephyrDirectories.

|Method|Description
|------|-----------
|GetUrlType|Parses a given URL and returns the [UrlType](#urltype) object that best matches that url.
|IsDirectory|Returns true if the url points to a directory.  See [Appendix](#directory-vs-file-url) for more details.
|IsFile|Returns true if the url points to a file.  See [Appendix](#directory-vs-file-url) for more details.
|GetZephyrFile|Returns a ZephyrFile object of the implemenation type specified in the URL.
|GetZephyrDirectory|Returns a ZephyrDirectory object of the implemenation type specified in the URL.
|CreateFile|Creates a ZephyrFile object of the implementation type specified in the URL, then calls its "Create" method.
|CreateDirectory|Creates a ZephyrDirectory object of the implementation type specified in the URL, then calls its "Create" method.
|Delete|Creates a ZephyrFile or ZephyrDirectory object of the implementation type specified in the URL, then calls its "Delete" method.
|Exists|Creates a ZephyrFile or ZephyrDirectory object of the implementation type specified in the URL, then calls its "Exists" method.



# Appendix

## Directory vs File URL

Since, in Windows, there is no way to determine from a URL alone whether an object is a directory or a file with no extension on it.  The decision was made that all directory url's in Zephyr.Filesystem must end in a slash ('/' or '\\').

## UrlType

An enumeration that represents an implementation and object type of a Zephyr-based object.

|Type|Description
|----|-----------
|Unknown|Object is of a type that is not known or supported.
|LocalFile|A Windows-based local filesystem file.
|LocalDirectory|A Windows-based local filesystem directory.
|NetworkFile|A Windows network-based filesystem file.
|NetworkDirectory|A Windows network-based filesystem directory.
|AwsS3File|An Amazon Simple Storage Serivce (S3) file.
|AwsS3Directory|An Amazon Simple Storage Service (S3) directory.