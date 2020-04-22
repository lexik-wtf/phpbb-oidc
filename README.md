# features of the fork:
* Compatible with phpBB 3.3.0+ (`user_email_hash` column got removed)

# phpbb-oidc
phpBB OpenID Connect plugin, based on [jumbojett/OpenID-Connect-PHP](https://github.com/jumbojett/OpenID-Connect-PHP).

## OpenID Connect
Per the [OpenID Connect specifications](https://openid.net/specs/openid-connect-core-1_0.html), phpbb-oidc should use `sub` as the sole identifier for the Service Provider (see [#5.1](https://openid.net/specs/openid-connect-core-1_0.html#IDToken)). However because PHPBB uses usernames as user identifiers, this plugin currently uses the preferredUsername attribute to identify users.


## Installation
* Unzip the folder and place it into `ext/ojathelonius/oidc`.
* Configure `config/oidc.yml` :
```yaml
# URL of the identity provider
# The following URI should show the OpenID discovery endpoint : http://idp/.well-known/openid-configuration
url: 'http://idp/'
# ID of the client in the IdP
clientId: 'clientId'
# Secret, if the client is set to private
secret: 'secret'
# Set to true if the client handles SSL/TLS on its own
ssl: false
# Set to true to create users automatically if they do not exist in PHPBB's database
createIfMissing: true
```

## Extend
phpbb-oidc uses standard OpenID user attributes. There are several additional attributes that could be retrieved the same way, such as groups, roles, user info...

In order to do this, the `OIDCUser` should be modified, and the field mapping must be done in both `createDefaultUserRow` and `updateUser`.

## Examples
### Example with Keycloak

```yaml
url: 'https://keycloak/auth/realms/my-realm/'
clientId: 'clientId'
secret: 'secret'
ssl: false
createIfMissing: true
```

## Caveats
* This plugin currently uses autologin for authentication : users are redirected to the IdP as soon as they land on phpBB
* OpenID-Connect-PHP does not implement Single Sign-Off yet : users will not be logged out automatically from phpBB when logging out from another SP
* Configuration still needs to be done through oidc.yml rather than through the ACP
* Still working on finding a way to show user friendly errors in the autologin auth flow, since `trigger_error` and `Exception` are not caught, unlike with the login flow

