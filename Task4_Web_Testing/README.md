# Task 4 — Web Security Testing

Manually tested OWASP Juice Shop for three vulnerabilities — SQL Injection, XSS, and Broken Authentication — using the browser instead of just relying on a scanner.

## Objective

Find and manually demonstrate real vulnerabilities in OWASP Juice Shop, understand how each one works, and see what impact it can have.

## Environment

* **Target:** OWASP Juice Shop
* **URL:** `http://localhost:3000`
* **Setup:** Local Docker instance
* **Testing:** Firefox browser
* **Internship:** Indravex Technologies — Cyber Security Internship

## What was done

1. Tested the login form for SQL Injection
2. Tested the search bar for XSS
3. Tested the login system for Broken Authentication
4. Recorded the results and their possible impact

## Findings

| **Vulnerability**     | **Where**      | **Result**                                            |
| --------------------- | -------------- | ----------------------------------------------------- |
| SQL Injection         | Login form     | Login bypass without knowing the password             |
| XSS                   | Search bar     | Attacker-controlled JavaScript executed               |
| Broken Authentication | Login attempts | No lockout, delay, or CAPTCHA after repeated failures |

## SQL Injection

Used the following payload in the email field:

```text
' OR 1=1--
```

Any value was entered as the password.

The login was successful without knowing a valid password, showing that the authentication could be bypassed.

## XSS

First tried:

```html
<script>alert('XSS')</script>
```

The browser blocked this attempt.

Then tried:

```html
<img src=x onerror=alert('XSS')>
```

This worked and the alert box appeared, confirming JavaScript execution through the search input.

## Broken Authentication

Tried several incorrect logins repeatedly against a known account.

There was no:

* Account lockout
* Noticeable delay
* CAPTCHA

This means the login endpoint allowed repeated password attempts without effective protection.

## Screenshots

Screenshots included for:

* SQL Injection login bypass
* Failed XSS `<script>` attempt
* Successful XSS `<img onerror>` payload
* Repeated failed login attempts

## Recommendations

* Use parameterized queries instead of raw SQL input.
* Properly sanitize and encode user input.
* Add Content Security Policy (CSP).
* Add rate limiting to the login endpoint.
* Add account lockout after multiple failed attempts.
* Use CAPTCHA where required.

## Conclusion

All three vulnerabilities were tested manually rather than just taking the output from a scanner.

The testing showed how improper handling of user input can lead to login bypass, JavaScript execution, and unlimited login attempts.

## Note

This testing was performed only on the local OWASP Juice Shop instance running in Docker for the internship task.
