# What it is

<div style="text-align: justify">
SignCloud is a solution to perform remote RSA transactions. Cryptographic operations are done in Uanataca Trusted Service Center side, where signature keys are stored in a Qualified Electronic Signature Creation Device (QSCD) system.
</div>

# How it works

<div style="text-align: justify">
An http RESTful API is exposed directly from Uanataca Trusted Service Center.
<br></br>
For the execution of signature requests, the hash of the document to be signed must be sent along with the credentials of the SignCloud account that should perform the signature.
<br></br>
In case of authentication, the cyphertext that must be decrypted must be sent along with the credentials of the SignCloud account that is required to perform the authentication.
<br></br>
Additionally, client applications like Signature Creation Application (SCA) can be interfaced with the remote RSA objects using and extended middleware library that implements the standard cryptographic interfaces for each operating system (PKCS#11, MS CSP, TokenD).
</div>
<br></br>

<img style="width: 80%; display: block; margin-left: auto; margin-right: auto" src="https://github.com/UANATACA/SIGNCLOUD-REPO/raw/main/img/signcloud-hiw.png"/> 


# Endpoint URLs

In order to use SignCloud API, every url must be composed as follows:

	https://{signcloudhost}/api/{resource}

The {signcloudhost} changes according to the environment:

- **signcloud.access.bit4id.org:13035/json** if the environment is **test**
- **cryptoapi.uanataca.com** if the environment is **production**

and {resource} is the name of the resource of our interest.<br>

**Example:**

  https://cryptoapi.uanataca.com/api/sign_rsa

# Requests
<div style="text-align: justify">
Every request made to SignCloud must have as content type header "application/json", so the request body must be a JSON object.
</div>

**Example**

A simple HTTP POST request made with curl.

	1 | curl -H "Content-Type: application/json" -d @params.json -X POST https://{signcloudhost}/api/sign_rsa

# Responses

The response of each api call is a JSON object with just 2 keys:

<html>
<table>
  <tr>
    <td>result</td><td>In case of success it is setted to True or contains some other values. Otherwise it is None</td>
  </tr>
  <tr>
    <td>error</td><td>	In case of success its value is null. Otherwise it is a JSON object containing the error occured. This object has three keys: "msg" the error message, "code" the HTTP status code and "details" which can contain some extra info about the error.</td>
  </tr>
</table>
<html>  

**Examples**

Here are listed some example responses returned by SignCloud

	1 | {
	2 |     "result": "fp+6Ull5WbTRqbMtW5R80DVhX+Qcoev0OqiDcIwy3sJYtmBP2RtC6NgukFIALpbUR
	3 |     Dd3izbmjqx8GVF8/BO/vAaqIE5H+xzA553OCvBijXr4KV1W/VY70yEfS1u25xogjZjRj4tg9qEKu
    4 |     mvdA62qT5S0bgxpyL5ESBlrWY9oVaAiamIwfsw3VJQC6Lft/JJuE067oqd4DkvOy6RE1DMzk/imq
    5 |     ffCTfG+LG1fRV/JL3l7gMPO+mAcsTVeUOSC1hpZEDgahdQJmv3ec7wJazc+7eArulCmPQHsAJS4O
    6 |     2AkXOb08/3XH5fH5b4B8vbLiWZG0SPT0adpfRI0euCXirDMBw==",
    7 |     "error": null
    8 | }

</br>

	1 | {
	2 |     "result": None,
	3 |     "error": {
	4 |         "msg": "Token locked",
	5 |         "code": 403,
	6 |         "details": ""
	7 |     }
	8 | }

</br>

    1 | {
    2 |     "result": null,
    3 |     "error": {
    4 |         "msg": "Missing parameter",
    5 |         "code": 412,
    6 |         "details": "'puk'"
    7 |     }
    8 | }

 </br>

    1 | {
    2 |     "result": None,
    3 |     "error": {
    4 |         "msg": "Pin invalid",
    5 |         "code": 403,
    6 |         "details": ""
    7 |     }
    8 | }

## HTTP Status Codes

Below a list of HTTP codes, and their description, that can be returned by SignCloud

<html>
<table>
  <tr>
    <th>Code</th><th>Description</th>
  </tr>
  <tr>
    <td>401</td><td>Invalid Credentials</td>
  </tr>
  <tr>
    <td>403</td><td>Token Not Found</td>
  </tr>
  <tr>
    <td>403</td><td>Pin Invalid</td>
  </tr>
  <tr>
    <td>403</td><td>Token Locked</td>
  </tr>
  <tr>
    <td>407</td><td>Authentication Refused</td>
  </tr>
  <tr>
    <td>412</td><td>Missing Parameter</td>
  </tr>
  <tr>
    <td>500</td><td>Internal Server Error</td>
  </tr>  
</table> 
</br> 
</html>

# Uanataca PKCS#11

Uanataca provides a set of libraries available for Linux, macOS or Windows delivering a PKCS#11 interface focused on loading SignCloud and Cryptowallet tokens to be saved.

## Configuration

Files to be configured prior to initialization shown below:

**Windows:**
```
up11/bin/up11.exe                                         Pkcs11 control executable
up11/bin/cmd.exe                                          Command-line up11 controller utility

up11/pkcs11/win32/bit4p11.dll.conf                        Configuration file of 32 bit pkcs11
up11/pkcs11/win32/bit4p11.dll                             32 bit .dll file

up11/pkcs11/win64/bit4p11.dll.conf                        Configuration file of 64 bit pkcs11
up11/pkcs11/win64/bit4p11.dll                             64 bit .dll file

up11/etc/up11/settings.conf                               Configuration file to set data provider
```
**Linux:**
```
up11/bin/up11.exe                                         Pkcs11 control executable
up11/bin/cmd.exe                                          Command-line up11 controller utility

up11/pkcs11/linux32/libbit4p11.so.conf                    Configuration file of 32 bit pkcs11
up11/pkcs11/linux32/libbit4p11.so                         32 bit .so file

up11/pkcs11/linux64/libbit4p11.so.conf                    Configuration file of 64 bit pkcs11
up11/pkcs11/linux64/libbit4p11.so                         64 bit .so file

up11/etc/up11/settings.conf                               Configuration file to set data provider
```
**macOS:**
```
up11/bin/up11.exe                                         Pkcs11 control executable

up11/pkcs11/libbit4p11.dylib.conf                         Configuration file of dylib
up11/pkcs11/libbit4p11.dylib                              .dylib file  

up11/etc/up11/settings.conf                               Configuration file to set data provider
```

<br>
<br>

<div style="text-align: justify">
You have to extract and mount the up11 depending on your OS and bitness in the directory where you wish them to be.
<br>
For a proper use of this libraries it is needed to run up11.exe in advance. This process will be executed on background and will be assuring the correct performance of up11.
<br>
As an optional measure, it is possible to launch automatically the control process introducing the absolute path of up11.exe inside the configuration file of the respective bitness .conf file setting a value to the parameter serverprocess.
</div>

    hsm_url=uds:/tmp/kchain-uanataca_p11_$USER
    hsm_timeout=1000
    serverprocess=/opt/up11/bin/up11.exe (Sample path)
    serverprocesswait=1000


<br>

To set the data provider and/or environment, settings.conf urls has to be modified.<br>
**Production:**
* SignCloud: https://signcloud.uanataca.com/smartup
* Cryptowallet: https://cryptowallet.uanataca.com/smartup
  
**Test:**
* SignCloud: https://signcloud.access.bit4id.org:13035/smartup
* Cryptowallet: https://cryptowallet.demo.bit4id.org:13035/smartup
  
Sample file in production environment:

     1 |{
     2 |    "default" : {
     3 |        "identity" : {},
     4 |        "up11": {
     5 |            "config" : {
     6 |                "network.options": {
     8 |                    "!ssl_verify": true,
     9 |                    "!ssl_cainfo": "$UP11DATA/cainfo"
    10 |                },
    11 |                "up11.providers": {
    12 |                    "22D5D604-922E-4076-9283-F506D92BEC20": {
    13 |                        "!url": "https://signcloud.uanataca.com/smartup",
    14 |                        "!url_cw": "https://cryptowallet.uanataca.com/smartup"
    15 |                    }
    16 |                }
    17 |            }
    18 |        }
    19 |    }
    20 |}


Once the configuration has been setup and the control process is up, up11 will be always working with and showing two slots
* 0 (Remote slot)
* 1 (Admin slot)

**Remote slot**
: </br>Remote slot is not going to have any data until the login into a remote token is well-made. Once a login is made, the remote slot is going to be fullfilled with one token and his appropiate objects relative to a specific remote identity. It depends on admin slot to contain data.

**Admin slot**
: </br>Admin slot is only used to perform control actions. It works as interface for the login or logout operations of remote tokens through standar PKCS#11 calls. 
</br>

## Importing up11

In case that you want to use Uanataca PKCS#11 libs to complement your own software, you must login first to obtain and operate with the desired token.
### Logging in

1. Load the PKCS#11 module file depending on your desired bitness (32 or 64 bits) and OS. 
2. Switch to Admin Slot
3. Call the function C_CreateObject with the next template:

    ````
      CKA_CLASS     =   CKO_DATA
      CKA_TOKEN     =   True
      CKA_PRIVATE   =   False               
      CKA_VALUE     =   USERNAME PASSWORD   // Login credentials
      CKA_ID        =   login               // Here you specify if you wanna log in or log out

      Replace USERNAME and PASSWORD with credentials to be used
    ````   
4. Once the previous steps are completed, the **Remote Slot** will be containing a token with his appropiate objects relative to a specific remote identity.


<br>


### Logging out

1. Load the PKCS#11 module file depending on your desired bitness (32 or 64 bits) and OS. 
2. Switch to **Admin Slot**
3. Call the function C_CreateObject with the next template:
</br>

    ````
      CKA_CLASS     =   CKO_DATA
      CKA_TOKEN     =   True
      CKA_PRIVATE   =   False               
      CKA_VALUE     =   ""                  // Login credentials
      CKA_ID        =   disconnect          // Here you specify if you wanna log in or log out
    ````   
4. Once the previous steps are completed, the **Remote Slot** will be empty.

## Cmd utility

This utility can be used to perform a quick test on pkcs11.
It is recommended to be in your up11 bin directory when executing the command-lines

### Logging in

    cmd.exe util create-objects --slot-number 1 --cka-id login --data "YOUR USERNAME YOUR PASSWORD"

This command-line intends to login into **Admin slot** (1) to save the token and his objects into **Remote slot** (0).

![example4](https://github.com/WilterToen/Img/blob/0e11b09b1b1439ab6acd77c2f079e19c9748e58b/Admin-slot.png?raw=true)


<br>

    cmd.exe util list-objects --slot-number 0

This command-line shows all the data previously loaded into slot 1
>Notes:
Even if no data is loaded when executing the first order, the utility returns "Succesfully created object"

Example of a cryptowallet with 1 certificate inside

![example9](https://github.com/WilterToen/Img/blob/main/Remote-Slot.png?raw=true)

![](https://github.com/WilterToen/Img/blob/main/Remote-slot2.png?raw=true)

</br>

### Logging out

    cmd.exe util create-objects --slot-number 1 --cka-id disconnect --data ""

This command-line removes the previous login so it gets to clear all the data that exists in **Remote slot**, will be returning *Succesfully created object* when executed.
![example7](https://github.com/WilterToen/Img/blob/main/Logout.png?raw=true)


</br>

    cmd.exe util list-objects --slot-number 0

This command-line will be used to check if there's any object into remote slot, if the slot is empty the order will return *"Exception: no token in Remote Slot"*

![example8](https://github.com/WilterToen/Img/blob/main/Logout2.png?raw=true)

</br>

# Postman collection

A postman collection is available as a support for a quick start.<br>

<a href="https://cdn.bit4id.com/es/uanataca/public/signcloud/Uanataca_SignCloud_Postman.zip"> SignCloud Postman collection download</a>

<div id="APIReference" style="padding-top: 60px;"><h1>API Reference<h1></div>