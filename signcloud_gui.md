# What it is

<div style="text-align: justify">
SignCloud is the solution for the enrolment, custody and usage of PKI remote credentials. SignCloud allows to digitally sign any document from any platform, desktop and mobile, exploiting a Secure Element on the Cloud, a Remote Virtual Token, thus releasing the End User from the burden of operating a smart card, USB token or any other sort of cryptographic device.
</div>

# How it works

This image summarizes how SignCloud works:

![img](https://raw.githubusercontent.com/UANATACA/SIGNCLOUD-REPO/main/img/signcloud-hiw.png?token=ATF574RBIQRMTBZ3ZXJHXF3ALXHH2)
<br></br>

<div style="text-align: justify">
SignCloud integrates a server side Digital Signature Engine, an Authentication Server, a certified Hardware Security Module (HSM) and an encrypted DB. During the enrollment phase the End User key-pair is generated on-board of the HSM in a secure environment.<br></br>
</div>

<div style="text-align: justify">
The private keys are stored and protected by means of the Key Wrapping, a certified native mechanism offered by the HSM. The creation and enrollment of a Remote Virtual Token is performed by means of the Bit4id smartCMS or by means of API.
<br></br>
</div>

<div style="text-align: justify">
The credentials binding a Virtual Token to an End User identity are created during this process. End Users have the sole control of their Virtual Token, in fact key usage is allowed only by Two Factor Authentication; after identification with username/password each signing operation is protected by an OTP request.<br></br>
</div>

<div style="text-align: justify">
Every third-party Signature Creation Application (SCA) (e.g. client application, web applet, etc.) can be interfaced with the Virtual Token thanks to an extended middleware library that implements the standard cryptographic interfaces for each operating system (PKCS#11, MS CSP, TokenD).<br></br>
</div>

# Endpoint URLs

In order to use SignCloud API, every url must be composed as follows:

	https://{signcloudhost}/api/{resource}

The {signcloudhost} changes according to the environment:

- **cryptoapi.access.bit4id.org:13035** if the environment is **test**
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