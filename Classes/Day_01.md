# Day 01 – Introduction to Web Application Security Testing

## Overview

Today's session focused on understanding the basics of web application security testing and learning how browser communication can be inspected using Burp Suite.

The session covered HTTP communication, request and response analysis, session handling, and basic access control concepts.

## 1. Introduction to Burp Suite

Burp Suite is a tool used for testing web applications by examining the communication between a browser and a web server.

It allows testers to view HTTP requests, inspect server responses, and understand how an application handles user actions.

### Important Features

| Feature | Basic Use |
|---|---|
| Proxy | Captures browser requests and server responses |
| HTTP History | Displays previously captured HTTP traffic |
| Repeater | Allows requests to be sent again with changes |
| Intruder | Helps perform automated request testing |
| Decoder | Helps encode and decode different data formats |
| Comparer | Helps compare two requests or responses |

## 2. Connecting the Browser with Burp Suite

To inspect browser traffic, the browser needs to communicate through Burp Suite's proxy.

FoxyProxy can be used to manage browser proxy settings.

### Basic Configuration

- Proxy Type: HTTP
- Host: `127.0.0.1`
- Port: `8080` (when using Burp's default listener)

Once configured, browser traffic can be viewed through Burp Suite.

For HTTPS inspection, the Burp CA certificate must also be configured in the dedicated testing browser.

## 3. Understanding HTTP Communication

HTTP is the protocol used for communication between a client and a web server.

When a user opens a website, the browser sends a request and the server returns a response.

### HTTP Request

A request generally contains:

- Request Method
- URL or Resource Path
- HTTP Version
- Headers
- Request Body (when required)

Example:

```http
GET /profile HTTP/1.1
Host: example.test
Accept: text/html
```

### HTTP Response

The server sends a response containing:

- Status Code
- Response Headers
- Response Body

By examining these details in Burp Suite, we can understand how the application processes user requests.

## 4. Difference Between GET and POST

GET and POST are commonly used HTTP methods.

| GET | POST |
|---|---|
| Commonly used to retrieve information | Commonly used to submit information |
| Parameters often appear in the URL | Data is commonly sent in the request body |
| Intended for safe retrieval | Can perform operations that change application state |

**Important:** Using POST does not automatically make information secure. HTTPS and proper application security controls are still required.

## 5. Understanding Cookies, Tokens and Sessions

These mechanisms help web applications manage user identity and maintain state between requests.

### Cookies

Cookies are small pieces of data stored by the browser and sent to matching websites with requests.

Some important cookie attributes:

- `Secure` – Restricts cookie transmission to secure HTTPS connections.
- `HttpOnly` – Prevents normal JavaScript access to the cookie.
- `SameSite` – Controls certain cross-site cookie requests.
- `Expires` – Defines when the cookie expires.

### Tokens

Tokens are used to prove identity or provide access to protected resources.

For example, an application may use a Bearer token or JWT for authentication.

Tokens should always be treated as sensitive information.

### Sessions

A session maintains information about a user's interaction with an application.

For example, after login, the application may use a session cookie to recognize the user across different requests.

## 6. Common HTTP Status Codes

During web application testing, status codes help us understand how a server responds to requests.

| Status Code | Meaning |
|---|---|
| 200 | Request successful |
| 201 | Resource created |
| 301 / 302 | Redirection |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Resource not found |
| 405 | Method Not Allowed |
| 429 | Too Many Requests |
| 500 | Internal Server Error |

A status code alone does not confirm whether a vulnerability exists. The response body and actual application behaviour must also be checked.

## 7. Authentication and Authorization

These are two important concepts in web application security.

**Authentication:** Verifies the identity of a user.

Example: Logging into an account using an email address and password.

**Authorization:** Determines what an authenticated user is allowed to access or perform.

Example: A normal user should not be able to access administrative functionality without the required permissions.

### Why They Matter

An application should not depend only on what is displayed in the browser. The server must independently check whether the user has permission to perform an action.

## 8. Understanding OTP Verification

OTP verification is commonly used to confirm a user's identity or ownership of an email address or phone number.

A typical workflow:

1. The user enters an email address.
2. The application sends an OTP.
3. The user enters the received OTP.
4. The application verifies the submitted code.
5. The application continues the registration or login process if verification succeeds.

During practical testing, I observed how Burp Suite can be used to inspect the OTP verification request and response.

One important concept I learned is that changing a response locally in the browser's traffic flow does not automatically mean that the server has accepted an invalid OTP. Actual server-side verification must be checked separately.

## 9. Key Learnings

- Understood the basic purpose of Burp Suite.
- Learned how browser traffic can be routed through a proxy.
- Understood the basic structure of HTTP requests and responses.
- Learned the difference between GET and POST.
- Understood cookies, tokens, and sessions.
- Studied common HTTP status codes.
- Learned the difference between authentication and authorization.
- Understood the basic OTP verification workflow.
- Learned why server-side validation is important.

## 10. Practice Checklist

- [ ] Explore the Burp Suite interface.
- [ ] Configure a browser proxy.
- [ ] Capture a basic HTTP request.
- [ ] Inspect request headers and response headers.
- [ ] Compare GET and POST requests.
- [ ] Identify cookies in HTTP traffic.
- [ ] Understand session behaviour.
- [ ] Review different HTTP status codes.
- [ ] Inspect an authentication request in a controlled lab.
- [ ] Understand the OTP verification request and response flow.

---

**Day 01 Complete – Web Application Security Testing Fundamentals**
