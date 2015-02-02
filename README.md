# SameName Developer API

SameName is an API to reserve usernames for corporate trademarks. SameName reduces startup workload for username disputes and exposes your business to top brands. We've create a simple work-flow to make it simple for developers to integrate the SameName API. 

Here's how it works:

Businesses provide SameName with a list of their trademarked usernames (such as "pepsi", or "adobe"). We then automatically reserve their usernames across all developers registered with SameName.

  - During registration, developers use an API check if a given username is reserved.
  - If it's reserved, developers prompt users to enter their SameName authorization code.
  - Businesses access their authorization code via their SameName dashboard.
  - Developers make a final API call to verify the provided authorization code is correct.
  
## Support
For support, please e-mail support@getsamename.com.

## Register an Account

To use the SameName API, you'll need to register as a SameName Developer:
https://getsamename.com/#/developer

Once you've registered, make note of your API credentials, which can be found from your dashboard. It will be referenced in this documentation as `api_key` and `api_secret`.

### Developer API Credentials

Param         | Description                             | Privacy
------------- | --------------------------------------- | --------------
api_key       | Public API Key used for all calls.      | Public; OK for client side calls.
api_secret    | Private API Secret.                     | Private; **DO NOT** expose to the public.

## API

----
### Username Status Check
#### `/username/status`

Checking the availability of a username is a simple GET request to the SameName API. It requires a developer `api_key` and a `username` field to check and returns boolean on the status of the provided username.

    GET https://api.getsamename.com/username/status
    
#### Params

Param         | Description                             | Example
------------- | --------------------------------------- | --------------
api_key       | Your Developer API Key.                 | --
username      | The username being checked.             | pepsi

#### Result

    {
      result: {
        reserved: [Boolean]
      }
    }
    
----
### Username Identity Verification
#### `/username/verify`

Username verification is an additional call to compare the authorization code provided by the user registering for your service, and the code provided by SameName. Verification authorization codes are unique to your service and the trademarked username.

    POST https://api.getsamename.com/username/verify
    
**Note:** This call must be made from the server-side. Do not expose your `api_secret` to the public. This call is a one-way comparison, but preventing client-side exposure will reduce any unauthorized attemps to verify a username for your service.
    
#### Params

Param         | Description                                        | Example
------------- | -------------------------------------------------- | --------------
api_key       | Your Developer API Key.                            | --
api_secret    | Your Developer API Secret.                         | --
username      | The username being checked.                        | pepsi
code          | User-provided authorization code for the username. | X7ycA499eB

#### Result

    {
      result: {
        verified: [Boolean]
      }
    }

----
## Example Workflow & Tips

### Username Status

The easiest way to use the SameName API is to bake it directly into your own username verification system. When a new user registers with your system, you check the username they've chosen against your own system to check its availability. At the same time, make a simple GET request to `/username/status` with your `api_key` and the `username` in question. If it's taken, treat the result as if it were taken in your own system.

If the username has been reserved via SameName, toggle an input to the user, asking them to enter their "SameName Verification Code" for "xyz" username. If the user does in fact own rights to this username, they will sign into their SameName dashboard to retreive the code.

### Username Verifications

The user will then supply the verification code to the input you've provided. From the server side, verify the provided code by calling `/username/verify` with your `api_key`, `api_secret`, the `username` in question, and the authorization `code` provided by the user.

From that call, you'll receive a boolean result on whether or not the authorization code is correct.


