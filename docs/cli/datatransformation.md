# Zephyr.DataTransformation CommandLine

The Zephyr.DataTransformation library is a collection of classes and methods that provide data transformation capabilities. It supports json, yaml and xml formats. 

The Zephyr.DataTransformation CommandLine class exposes these methods so they can be called from  the command line. You need Zephyr.Cli to invoke the CommandLine method.

Download the latest builds from GitHub:
<a href="https://github.com/SynapseProject/zephyr.DataTransformation.net/releases" target="_blank">https://github.com/SynapseProject/zephyr.DataTransformation.net/releases</a>
<a href="https://github.com/SynapseProject/zephyr.cli.net/releases" target="_blank">https://github.com/SynapseProject/zephyr.cli.net/releases</a>

## CommandLine Help:
```dos
zephyr.datatransformation.dll, Version: 0.1.0.0

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

  Examples:
     zephyr datatransformation json convert file:c:\temp\products.json
         outputFormat:yaml

     zephyr datatransformation json xsltransform file:c:\temp\products.json
         xslt:c:\temp\foo.xslt

     zephyr datatransformation json jsonselect file:c:\temp\products.json
        expression:$..Products[?(@.Price >= 50)].Name

     zephyr datatransformation json regexmatch file:c:\temp\products.json
        pattern:\d+ options:ignorecase,compiled
```
