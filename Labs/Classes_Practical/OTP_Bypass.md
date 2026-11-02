# OTP Verification Bypass via Response Handling

## Vulnerability Type

Email OTP Verification / Authentication Workflow Issue

## Summary

During testing of the TaskBear registration process, OTP verification requests were captured using Burp Suite.
The verification endpoint returned an `HTTP/2 201 Created` response containing the contact information and a `verifiedAt` timestamp.
During a second test, a different OTP value (`123456`) was submitted. The captured response also showed `201 Created`, and the registration process proceeded to the next step.
This behaviour requires further validation to determine whether invalid OTPs are being accepted by the backend or whether the registration flow is relying on client-side response handling.

## Vulnerable Endpoint

`POST https://[REDACTED_API_HOST]/api/v1/uwa/auth/otp/verify`

## Steps to Reproduce

1. Open the TaskBear registration page.
2. Start registration using a test email address.
3. Proceed to the OTP verification page.
4. Capture the OTP verification request using Burp Suite.
5. Observe the response returned after the first OTP submission.
6. Start a second test using another controlled email address.
7. Submit the OTP value `123456`.
8. Capture the corresponding request and response in Burp Suite.
9. Observe the `HTTP/2 201 Created` response.
10. Observe that the application proceeds to the registration details page.
11. Continue through the registration workflow and observe the next onboarding page.

## Payload / Response Observed

The captured response contained the following structure:

```http
HTTP/2 201 Created
Content-Type: application/json; charset=utf-8
X-Powered-By: Express
X-Ratelimit-Limit: 10
X-Ratelimit-Remaining: 9

{
  "contact": "[REDACTED_EMAIL]",
  "verifiedAt": "[REDACTED_TIMESTAMP]"
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


