# User enumeration — Notes

## Definition
User enumeration is not a vulnerability in itself, but in tandem with other types of vulnerabilities -- like the ability to brute-force login.
It's very common and easy for exploit. 

## Impact and risks
What can go wrong if an application is vulnerable to user enumeration?
Gaining access to the list of usernames for a site provides attackers half the authentication information required for access to accounts.
- **Unauthorized account access** — attackers can read sensitive information 
- **Data exfiltration** — harvesting data silently and using it elsewhere (fraud, credential stuffing).  
- **Reputation and legal consequences** — leaks can harm users, damage trust, and cause regulatory penalties.

Brute-force approach for breaking into accounts is based on frequent passwords gessing. This technique can be reeinforced by deriving information from login, social engineering traps or by usage of most-common passwords database.
Real-world incidents show that SQLi has been used to compromise high‑profile services, leak credentials, and extract sensitive records. Even security-conscious organisations have suffered when SQLi was present.

## Prevention and mitigation
Key defensive measures to prevent credentials information leakage:

### Use generic error messages 
Don't use different error messages for unrecognized usernames and incorrect passwords - it can be used for valid usernames probing. 
``` javascript
app.post('/login', (request, response) => {
  const user = getUser(request.params.username)

  // The function returns early if the username is incorrect.
  if (!user) {
    response.status(401)
    return
  }
  
  // This code path will only get executed if the username is
  // correct, allowing an attacker to infer the existence of a
  // username by timing how long the HTTP response takes.
  bcrypt.compare(request.params.password, user.hashedPassword, (error, matched) => {
    if (matched) {
      request.session.username = request.params.username
      self.redirect('/')
    }
    else {
      response.status(401)
    }
  })
})
```
Instead make sure that all ligin code-paths take about the same time on average - preform time-consuming operations (like password-hashing even when username is wrong). 
``` javascript
app.post('/login', (request, response) => {
  const user = getUser(request.params.username)

  // Calculate the password hash regardless of whether the username exists,
  // so the attacker cannot use timing attacks to detect which users exist
  // in the database.
  const passwordHash = user ? user.hashedPassword : ''

  bcrypt.compare(request.params.password, passwordHash, (error, matched) => {
    if (user && matched) {
      request.session.username = username
      self.redirect('/')
    }
    else {
      response.status(401)
    }
  })
})
```
 ### Ensure that there are no informations about non-existing acounts (for example when reseting a password of unknown username)).
Instead provide information about potential reset link sent via email or checking user's inbox.

### Don't provide information about already existing accounts during registration (emails/usernames).
Instead send password reset eamil when a user accidentally tries to sing-up a second time 

### For unique usernames protect sing-up page with (captcha)
Remember to use captcha that is etical and sustainable - don't feed cyber-giants's algorythms, but einforce lockal platform coops or data commons cooperatives.

### Don't differentiate responses for users not found( 404) and forbidden (403)
''' bat
HTTP GET example.com/user/david       403
HTTP GET example.com/user/robin       404
HTTP GET example.com/user/maria       403
'''

## Sources
Hacksplaining User Enumeration
https://hacksplaining.com/lessons/user-enumeration/leaky-vessel
