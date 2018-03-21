# Overview
The Zephyr.Crypto library contains classes and methods that lets you perform common cryptographic tasks. It currently supports the following algorithms:

* Base64
* Rsa
* Rijndael

The library has a command line interface class that exposes these functions so they can be accessed from the command line.

# Installation
To use the command line function, you will need to download and install the following builds from GitHub:

1. Zephyr.Crypto Library

    <a href="https://github.com/SynapseProject/zephyr.Crypto/releases" target="_blank">https://github.com/SynapseProject/zephyr.Crypto/releases</a>

2. Zephyr.Cli.Net

    <a href="https://github.com/SynapseProject/zephyr.cli.net/releases" target="_blank">https://github.com/SynapseProject/zephyr.cli.net/releases</a>

# Usage

```
Syntax:
  zephyr crypto {algorithm} {action} {parameters}

  algorithm:   base64|rijndael|rsa

  action:
    base64     encode         Returns base64 encoded value
               decode         Returns base64 decoded value

    rijndael   encrypt        Returns rijndael encrypted value
               decrypt        Returns rijndael decrypted value

    rsa        encrypt        Returns rsa encrypted value
               decrypt        Returns rsa decrypted value
               genkey         Creates rsa keypair for use in encrypt/decrypt
                              actions

  parameters:  List of key:value pair
               Type 'help' in the place of parameters for help on parameters
```
## Base64

### Encode Action Parameter

|Key|Value|Required?|Description
|-|-|:-:|-
|data|Text to encode|Y|-

### Decode Action Parameter

|Key|Value|Required?|Description
|-|-|:-:|-
|data|Text to decode|Y|-

## Rijndael

This algorithm uses 128 bit block size and 256 bit key size.

### Encrypt Action Parameters

|Key|Value|Required|Description
|-|-|:-:|-
|data|Text to encrypt|Y|-
|pass|Pass phrase|Y|-
|salt|Salt value|Y|Min 8 bytes
|iv|Initialization vector|Y|Must be 16 characters long to match the 128 bit block size used in algorithm

### Decrypt Action Parameters

|Key|Value|Required|Description
|-|-|:-:|-
|data|Text to decrypt|Y|
|pass|Pass phrase|Y|
|salt|Salt value|Y|Min 8 bytes
|iv|Initialization vector|Y|Must be 16 characters long to match the 128 bit block size used in algorithm

## RSA

### GenKey Action Parameters

The GenKey action generates a key pair and stores them in the user profile or in key files.

To store the key pair in the user profile, 
* Use the `kcn` parameter key and specify the key container name as the value.
* If the key container name already exists, it will reuse the existing keys instead of creating new ones.
* If you pass an empty key container name, a default key  container is used instead. 

To store the key pair in XML files, 
* Use the `keyFile` parameter key and provide the path to the XML files. 
* This option will create 2 files:
    * `{filePath}.pubPriv` contains public/private keys
    * `{filePath}.pubOnly` holds just the public key

|Key|Value|Required?|Description
|-|-|:-:|-
|kcn|Key container name|N|Use either `kcn` or `keyFile` or both
|keyFile|Path to key files|N|Use either `kcn` or `keyFile` or both

### Encrypt Action Parameters

The Encrypt action encrypts data using a key obtained from user profile key container or key file.

To use a key from the user profile key container,
* Supply the `kcn` parameter key and and its value
* If the key container name cannot be found, one will be created
 
To use a key from key file,
* Supply the `keyFile` parameter key and its value

`kcn` has a higher priority over `keyPath`. So when both parameter keys are supplied, `keyFile` will be ignored.

|Key|Value|Required?|Description
|-|-|:-:|-
|data|Text to encrypt|Y|-
|kcn|Key container name|N|Use either `kcn` or `keyFile`
|keyFile|Path to key file|N|Use either `kcn` or `keyFile`

### Decrypt Action Parameters

The Decrypt action decrypts data using a key obtained from user profile key container or key file.

To use a key from the user profile key container,
* Supply the `kcn` parameter key and and its value

To use a key from key file,
* Supply the `keyFile` parameter key and its value

`kcn` has a higher priority over `keyPath`. So when both parameter keys are supplied, `keyFile` will be ignored.

|Key|Value|Required?|Description
|-|-|:-:|-
|data|Text to decrypt|Y|-
|kcn|Key container name|N|Use either `kcn` or `keyFile`
|keyFile|Path to key file|N|Use either `kcn` or `keyFile`


# Examples

```dos
zephyr crypto base64 encode data:"text to encrypt"

zephyr crypto rijndael encrypt data:"text to encrypt" pass:password salt:saltvalue iv:1234567890123456

zephyr crypto rsa genkey kcn:samplecontainer keyfile:c:\temp\sample

zephyr crypto rsa encrypt data:"text to encrypt" kcn:samplecontainer

zephyr crypto rsa encrypt data:"text to encrypt" keyFile:c:\temp\sample.pubOnly

```

# Get Help
To get help from the CLI, use `help` or `?`

For a high level help,
```
zephyr crypto help|?
```
For action parameter help,
```
zephyr crypto {algorithm} {action} help|?
```  

