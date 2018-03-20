# Overview

The Zephyr.DataTransformation library is a collection of classes and methods that provide data transformation capabilities. It supports json, yaml and xml formats. 

The library has a class that exposes these methods so they can be called from  the command line. 

# Installation

To use the command line function, you will need to download and install the following builds from GitHub:

1. Zephyr.DataTransformation Library

    <a href="https://github.com/SynapseProject/zephyr.DataTransformation.net/releases" target="_blank">https://github.com/SynapseProject/zephyr.DataTransformation.net/releases</a>
    
2. Zephyr.Cli

    <a href="https://github.com/SynapseProject/zephyr.cli.net/releases" target="_blank">https://github.com/SynapseProject/zephyr.cli.net/releases</a>

# Usage

```dos
Syntax:
  zephyr datatransformation {serializationFormat} {action} {parameters}

  serializationFormat: json|yaml|xml

  action:                parameters

      Convert            Convert file to outputFormat
                         file:{filePath} outputFormat:{json|yaml|xml}

      XslTransform       Transform file using xlst
                         file:{filePath} xslt:{filePath}

      JsonSelect         Query file using SelectToken expression
                         file:{filePath} expression:{string}

      RegexMatch         Query file using Regex return first match
                         file:{filePath} pattern:{string}
                         [options:{RegexOptions}]

      RegexMatches       Query file using Regex return all matches
                         file:{filePath} pattern:{string}
                         [options:{RegexOptions}]
     
```

# Examples

```dos
zephyr datatransformation json convert file:c:\temp\products.json
         outputFormat:yaml

zephyr datatransformation json xsltransform file:c:\temp\products.json
         xslt:c:\temp\foo.xslt

zephyr datatransformation json jsonselect file:c:\temp\products.json
        expression:$..Products[?(@.Price >= 50)].Name

zephyr datatransformation json regexmatch file:c:\temp\products.json
        pattern:\d+ options:ignorecase,compiled
```
# Get Help
To get help from the CLI, use `help` or `?`

For a high level help,
```
zephyr datatransformation help|?
```
For action parameter help,
```
zephyr datatransformation {serializationFormat} {action} help|?
```  