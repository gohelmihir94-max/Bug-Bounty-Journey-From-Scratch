# OTP Verification Bypass via Response Handling

## Vulnerability Type

Email OTP Verification / Authentication Workflow Issue

## Summary

During TaskBear registration testing, Burp Suite captured `201 Created` responses for OTP submissions, including an incorrect OTP (`123456`). The registration process proceeded to the next step. Server-side OTP validation still requires confirmation.

## Vulnerable Endpoint

`POST https://master.prod.api.taskbear.com.au/api/v1/uwa/auth/otp/verify`

## Steps to Reproduce

1. Open the TaskBear registration page and start registration using a temporary email address.
2. Submit the OTP and capture the verification request and successful response using Burp Suite.
3. Start a new registration using another controlled email address and proceed to the OTP verification page.
4. Submit an incorrect OTP (`123456`) and capture the verification request and response in Burp Suite.
5. Compare the captured responses and inspect the verification status and response body. Document any response manipulation performed during authorized testing.
6. Forward the response and observe whether the application proceeds to the registration details page and subsequent onboarding process.

## Payload / Response Observed

The captured response contained the following structure:

```http
HTTP/2 201 Created
Content-Type: application/json; charset=utf-8
X-Powered-By: Express
X-Ratelimit-Limit: 10
X-Ratelimit-Remaining: 9

{
  "contact": "[bravepanda57454@patchsec.site]",
  "verifiedAt": "[2026-11-02T16:34:00.936Z]"
}
```

## Proof of Concept (PoC)

### Screenshot 1: OTP Verification Request
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/ec19c07d-2c9a-4733-aee3-ed7c99f63698" />


### Screenshot 2: OTP Verification Response
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/b3917e5c-099c-4a44-8019-7574b2c640a3" />


### Screenshot 3: Second  OTP Verification Request
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/ae55a561-b78f-4c5e-8a20-6560229359f5" />


### Screenshot 4: Response to Second Verification Request
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/70b4eed6-2206-461e-ae50-edbeff66ccc5" />


### Screenshot 5: Registration Page
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/c542016c-58bc-4299-a907-289aad6800ae" />

### Screenshot 7: Onboarding Page
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/cbb421fd-3fb9-48ad-ba23-87a7fcf83177" />


## Impact

- The registration workflow proceeded after the OTP verification requests.
- Email verification enforcement may be affected.
- If the backend accepts an invalid OTP, users may be able to register without proving ownership of their email address.


## Recommendation for Fix

1. Validate OTPs on the server before accepting verification.
2. Do not rely on client-side response handling for authentication decisions.
3. Maintain verification status on the server.
4. Bind OTPs to the correct email address and registration session.
5. Enforce OTP expiration and single-use restrictions.
6. Apply rate limits to OTP verification attempts.


