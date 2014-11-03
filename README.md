# SameName Developer API

SameName is an API to reserve usernames for corporate trademarks. SameName reduces the need for startups to worry about username disputes and
exposes your business to top brands. Here's how the developer integration works.

  - You run a simple API call to check if a username has been reserved. If it's available, it's busienss as usual.
  - If it's been reserved, you ask the user for their SameName code.
  - Busiensses access their unique code from their SameName dashboard.
  - You make a second API call to verify the owner of the domain.

## Register an Account

To use the SameName API, you'll need to register as a SameName developer:
http://samename.co/#/developers

Once you've registered, make note of your API key, which can be found from your dashboard. It will be referenced
in this documentation as `api_key`.

## Integraation

### Trademark Verification

Trademark verification is a simple GET request that returns a boolean result on a provided username.

    POST https://api.samename.co/trademark/check
    
#### Params

Param         | Description                                      | Example
------------- | ------------------------------------------------ | --------------
api_key       | Your Developer API Key.                          | XXXXXXXXXXXXXXXXXXXX
trademark     | A string of the requested trademarked username.  | pepsi

- **api_key**: Your developer API key.
- **trademark**: The requested username (ie. "pepsi").
    
#### Result

    {
      result: {
        reserved: [Boolean]
      }
    }
