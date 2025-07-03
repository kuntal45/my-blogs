[comment]: <> (difference between access token and refresh token)


# Why tokens?

Authentication and Authorization are the crucial steps for protecting user data. Hear token based authentication comes into the picture. There are many authentication methodologies are available but took in based authentication is most popular in current days. Using token based authentication instead of other authentication techniques offers several advantages:

- ### Stateless
Tokens are stateless, meaning they do not require the server to store session data. This makes it easier to scale applications horizontally, as any server can handle a request without needing access to a centralized session store.

- ### Cross-Domain Support 
Tokens, especially JWTs, can be used across different domains and services, which is beneficial in microservices architectures and when integrating with third-party services.

- ### Security 
Tokens can be signed and encrypted, ensuring the integrity and confidentiality of the data they carry. They can also include claims that help in making authorization decisions.

- ### Decoupling 
Tokens allow for a decoupled architecture where authentication and authorization can be handled by separate services, improving modularity and flexibility.

- ### Mobile and Single Page Applications 
Tokens are well-suited for mobile and single-page applications where maintaining a session on the server side is not practical.

- ### Expiration and Refresh 
Tokens can have built-in expiration times, enhancing security by limiting the time a token is valid. Refresh tokens can be used to obtain new access tokens without requiring the user to log in again. 

# Access Token 
The token name says, Access token used to gives access of a specific resource for a specific user. Basically Access Token is used to authorize an user. It gives specific access or permission to a specific user.

### Lifespan 
Access tokens usually have very short lifespans; in many cases, they last only a few hours. For example, access token lifespans in [Microsoft’s identity platform](https://learn.microsoft.com/en-us/entra/identity-platform/configurable-token-lifetimes) are between 30-90 minutes by default. These are also variable, assigned randomly to a value in the range. Its provide authorization to a specific user for a certain amount of time 

### Uses
Access token usually used to authorize an user to access a specific resource. Usually access token assigned when user start a session.

### How it works 
1. A client app requires authorization from a user to grant them access
1. The user permits an authorization server to send an authorization code to the client
1. The authorization server issues an access token upon client request
1. The client validates the token and grants the user access to resources

![How it works](https://images.ctfassets.net/xqb1f63q68s1/5KRSBrWiRfJRaFicCsU5zB/c0a166d01c500a303cb02dcf9ee8a03d/How_access_token_works.png)

# Refresh Token
 Refresh tokens are used to refresh user sessions, so user don't need to login again and again. Refresh tokens are mainly used in user Authentication. All details of refresh token is discussed below.

### Lifespan 
Another major differentiating factor for refresh tokens is that they last much longer than access tokens. For example, refresh tokens in [Microsoft’s identity platform](https://learn.microsoft.com/en-us/entra/identity-platform/refresh-tokens) have default and rigid (non-configurable) lifespans of 90 days for most scenarios and 24 hours for single-page apps. 

### uses 
Refresh tokens work with access tokens to facilitate long-lived sessions without repeated logins. They’re usually stored securely on the authorization server itself.

### Limitations
Refresh tokens are not useful independently from access tokens – they are used exclusively in relation to them. They’re not strictly necessary, and apps can use OAuth without them, but they do provide benefits in terms of greater UX and security.

# Comparison between Access token and Refresh Token
Baseline access is provided in the form of access tokens, whereas refresh tokens play a key role in maintaining sessions through extended periods of use.

As noted, access tokens and refresh tokens are not separate entities. They work together in cases where refresh tokens are used to power seamless and secure long-term authentication.

That said, they are distinct. Here’s how the two tokens stack up across three critical factors:

|   | Access tokens | Refresh tokens |
|  :---:  |  :---:  |  :---:  |
| User experience/interface | Authenticate and authorize users up-front | Users are re-authorized without re-authenticating |
| Transmission and storage | Tokens are sent over secure channels (HTTPS) and then stored on client servers | Tokens are sent over HTTPS, like access tokens, but stored on the authorization server |
| Security considerations | Short life spans, encryption, and the ability to revoke access power strong security | Longer life spans are offset by less frequent refreshes and greater revocation ability |

	![Access Token vs Refresh Token A Breakdown](https://media.geeksforgeeks.org/wp-content/uploads/20240927165533/Access-Token-vs-Refresh-Token-A-Breakdown.webp)

# When to use each type of token
Access tokens are one of the best options if you’re considering a passwordless auth solution for your next software project. Some of their optimal use cases are situations where users require access to shared resources. 

When users require authentication to access or modify files owned by others, access tokens can streamline this process.

In most cases, when you use access tokens, it’s also sensible to use refresh tokens. For example, if users are likely to remain logged in for extended periods (or indefinitely) forcing them to re-authenticate regularly could cause friction. Refresh tokens remove that necessity without compromising on security. 

However, there are also cases wherein extended access is optional. In these cases, refresh tokens might not be needed.


# Conclusion
Access and refresh tokens are indispensable parts of modern authentication and authorization processes. By understanding and implementing these tokens, organizations can provide a frictionless and secure experience for their end users.