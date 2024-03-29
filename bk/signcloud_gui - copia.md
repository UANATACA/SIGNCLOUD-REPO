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


# Postman collection

A postman collection is available as a support for a quick start.<br>

<a href="https://cdn.bit4id.com/es/uanataca/public/signcloud/Uanataca_SignCloud_Postman.zip"> SignCloud Postman collection download</a>

<div id="APIReference" style="padding-top: 60px;"><h1>API Reference<h1></div>