# Videcom

> This document was created based on lessons learned from working with the Videcom API. There is a good bit of "camp fire knowledge" here and knowledge developed through inference and deduction.

> Much like the Pirates Code, it is correct to think of this document as more of a set of "guidelines" than actual rules.
![Guidelines](./guidelines.gif)


## Connecting

### Endpoints

Each airline has it's own unique endpoint defined by the pattern below:

```
# TEST
https://customertest.videcom.com/${airlineName}/vars/public/webservices/VRSXMLWebservice3.asmx

# PROD
https://customer.videcom.com/${airlineName}/vars/public/webservices/VRSXMLWebservice3.asmx
```

In this URL, the `${airlineName}` is the name of the airline as assigned by Videcom.

#### Commands via XML - `?op=RunVRSCommand`

This endpoint allows you to submit commands as though you were typing them out at the Videcom-supplied command line interface. Each command is wrapped in an XML payload.

```xml
<soap12:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap12="http://www.w3.org/2003/05/soap-envelope">
  <soap12:Body>
    <msg xmlns="http://videcom.com/">
      <Token>${token}</Token>
      <Command>${command}</Command>
    </msg>
  </soap12:Body>
</soap12:Envelope>
```

The `${token}` value is provided by Videcom and is associated with the `${airlineName}` and source IP that your application is using.

The `${command}` value is the same as the command that you might otherwise type into the Videcom command line interface.

#### Commands & XML via SOAP - `?WSDL`

This endpoint provides a WSDL that can be used by SOAP clients to construct both standard Videcom commands and submissions to their XML API.

It's recommended that you use the `VrsXmlWebService3.VrsXmlWebService3Soap12` branch of the SOAP API. This branch has two methods that are particularly useful:

1. `RunVRSCommand` - Equivalent to the `?op=RunVRSCommand` endpoint.
2. `RunVRSCommandXml` - Allows you to submit XML payloads. Mainly for flight scheduling. See `VRS XML Flight Schedule.doc` for more detailed information about some of the XML payload options.

### Authentication

There are two layers of authentication:

1. Source IP validation.
2. Payload token validation.

#### Source IP Validation

Videcom requires that you register the source IP address for each application that is going to interact with their API. Each IP must be associated with the endpoint that it will interact with. You will have to provide the sine code when you request adding an IP address.

Both the `RunVRSCommand` and `WSDL` endpoints perform this validation.

#### Payload Token Validation

If you are interacting with the `?op=RunVRSCommand` endpoint or the `RunVRSCommand` SOAP method, you will need to provide a Videcom-supplied token to authenticate your request.

The token may be the same for multiple endpoints/airlines.

### Error Codes

- `101` - Not HTPPS
- `102` - No token
- `103` - Invalid token
- `104` - Invalid agent sine
- `105` - No IP configured for agent
- `106` - Invalid IP
- `107` - ApiIpAddress missing from agent table


## Videcom Commands

A Videcom command is defined as a string of characters that are interpreted by the Videcom systems to perform a specific action. While a single string performs a specific action, you can concatenate multiple strings together using a carat (`^`).

When interacting with the Videcom command API, it's correct to think of submitting each request as being the same as pressing the `Enter` key at the command line.

Videcom provides some documentation for it's command-line interface - http://manuals.videcom.com/interfaces/commands/all-commands/list-of-all-commands/
To access this documentation, you'll need to log in with the password of `videcom`.

### Multiple Command Concatenation

When you need to execute multiple commands in a single PNR context, you should concatenate the individual commands with a carat `^`. Look at the sample booking command below for an example of what this looks like.

If a command is concatenated, the order of the commands will affect the success of the command as a whole. For example, if you try to add contact information for a passenger before you have defined the passenger, the command will fail.
