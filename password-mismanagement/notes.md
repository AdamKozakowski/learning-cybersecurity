# Password Mismanagement — Notes

## Definition
Password mismanagement leads to vulnerabilities and opens the door for cybercriminals aiming to exploit your information. Password mismanagement covers a variety of behaviors.

## Impact and risks

--If your user accounts get hacked easily, you quickly won't have any users. 
Ensuring strong authentication is a mix of pushing your users into good habits, and following them yourself. Attackers are constantly trying to find ways to bypass authentication, so you need to make sure you do not permit any vulnerabilities.
Prevalence - common, exploitabibity - moderate, and impact - devastating. 

## Prevention and mitigation
Key defensive measures to prevent password mismanagement:

### Use third-party service providers (like Auth0)
It's secure way to store credentials. It offlads the complexity of authentication management to a trusted entity and makes it convinient for users.

### Integrate with identity provider (like Okta)
Allows users to keep control of their identity data.

### Define password complexity rules
Define minimum length of password (8 - 10), request for mixed case, numeric characters or special symbols. That makes passwords less guessable. 
Too strict rules will afect memorability of passwords and users will resort to writing their passwords down, reducing theirs security.

### Implement secure password reset system:
When reseting the password, request for new, not previously used one. When sending reset link to user via email, links should time-out in a resonable time-frame.
Remember that users, in case of frequent reseting of passwords,  tend to append a number at the end of the password.

 ### Implement password reset screen for logged-in users
 Ensure that it requires re-entering of the user's old password, in case they leave themselves logged in on a shared computer. 

 ### Implement password lockout
 Thats period after too many failed login attempts - this discoutage brute forcing of passwords. 
 To give possibility to user to log in during brute-force attack, CAPTCHA test can be implemented.

 ### Secure Password storage.
 Never store passwords in plain-test - use one way hashing. 
 During hashing remember to "salt" - add an element of randomness to each encrypted password after hashing, and to "pepper" - to add additional separate system to encryprion in form of adding secret random value to a password before hashing it. Pepper is usually longer that salt and kept separate from hashed passwords. 
 Example of hashing:
 ``` python
 import bcrypt

def hash_password(password, salt, pepper):
    # Concatenate pepper and password
    combined_password = pepper + password

    # Generate a salted hash
    hashed_password = bcrypt.hashpw(combined_password.encode('utf-8'), salt)

    return hashed_password

# Example usage
password = "user_password"
salt     = bcrypt.gensalt()
pepper   = "your_secret_pepper"

hashed_password = hash_password(password, salt, pepper)
print(f"Hashed Password: {hashed_password.decode('utf-8')}")
```
### Use multi-factor authentication (MFA)
Like time-based one-time passwords (TOTP) or biometric authentication. 

### Use HTTPS for Secure Communication
Make sure you use encrypted communication when asking a user for their login details, or else their password can be stolen by a monster-in-the-middle attack. Similarly, make sure all communication between your server and their browser after login is done over HTTPS, so their session cannot be hijacked.

## Bonus - OAuth vs SAML
OAuth and SAML are both authentication and authorization protocols used for enabling Single Sign-On (SSO), but they were designed for different use cases and operate differently.

### OAuth
Primarily for authorization, allowing users to grant third-party apps limited access to their resources without sharing credentials. Ideal for mobile apps, modern web apps, and API access (e.g., “Login with Google”). Uses access tokens (often JWT) in JSON format. RESTful, uses HTTP headers. Involves Resource Owner, Client, Authorization Server, and Resource Server. Often used with OpenID Connect (OIDC) for authentication. 
Popular in consumer-facing applications.

### SAML
Primarily for authentication, enabling Single Sign-On (SSO) across enterprise systems. Best for enterprise or legacy apps needing federated identity (e.g., employee access to multiple internal tools). Uses SAML Assertions in XML format. Uses browser redirects (POST/Redirect bindings), typically SOAP or HTTP. Involves Identity Provider (IdP) and Service Provider (SP). Popular in enterprise environments, education, government.

Third-party authentication isn't suitable for all applications. 

## Sources
Hacksplaining Password Mismanagement https://www.hacksplaining.com/lessons/password-mismanagement/start
