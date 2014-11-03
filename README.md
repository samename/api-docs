# SameName Developer API

SameName is an API to reserve usernames for corporate trademarks. SameName reduces startup workload for username disputes and exposes your business to top brands. We've create a simple work-flow to make it simple for developers to integrate the SameName API. 

Businesses provide SameName with a list of their trademarked usernames (such as "pepsi", or "adobe"). We then automatically reserve their usernames across all developers using the SameName API.

  - During registration, developers use an API check if a given username is reserved.
  - If it's reserved, developers prompt users to enter their SameName passcode.
  - Businesses access their passcode via their SameName dashboard.
  - Developers make a final API call to verify the provided passcode is correct.

## Register an Account

To use the SameName API, you'll need to register as a SameName Developer:
http://samename.co/#/developers

Once you've registered, make note of your API credentials, which can be found from your dashboard. It will be referenced in this documentation as `api_key` and `api_secret`.

## API Keys

Param         | Description                             | Privacy
------------- | --------------------------------------- | --------------
api_key       | Public API Key used for all calls.      | Public; OK for client side calls.
api_secret    | Private API Secret.                     | Private; **DO NOT** expose to the public.

## API

----
### `/username/status`

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
### `/username/verify`

Username verification is an additional call to compare the passcode provided by the user registering for your service, and the passcode provided by SameName. Verification passcodes are unique to your service and the trademarked username.

    POST https://api.getsamename.com/username/verify
    
**Note:** This call must be made from the server-side. Do not expose your `api_secret` to the public. This call is a one-way comparison, but preventing client-side exposure will reduce any unauthorized attemps to verify a username for your service.
    
#### Params

Param         | Description                              | Example
------------- | ---------------------------------------- | --------------
api_key       | Your Developer API Key.                  | --
api_secret    | Your Developer API Secret.               | --
username      | The username being checked.              | pepsi
passcode      | User-provided passcode for the username. | X7ycA499eB

#### Result

    {
      result: {
        verified: [Boolean]
      }
    }

----
## Example Workflow

