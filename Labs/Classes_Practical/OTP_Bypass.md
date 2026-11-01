# OTP Bypass – Practical Report

## 1. Practical Overview

This practical involved testing the OTP verification process during account registration on a web application.

The main focus was to understand how the application handles invalid OTP submissions and how HTTP responses can be inspected and modified using Burp Suite.

**Testing Status:** Potential Finding – Pending Validation

## 2. Objective

- Understand how OTP verification works during registration.
- Observe how the application handles invalid OTP submissions.
- Inspect HTTP requests and responses using Burp Suite.
- Understand the difference between client-side response handling and server-side verification.

## 3. Tools Used

| Tool | Purpose |
|---|---|
| Burp Suite Professional | Inspecting HTTP requests and responses |
| Firefox | Accessing the web application |
| TempMail | Receiving registration verification emails |

## 4. Testing Steps

1. Opened the TaskBear registration page.
2. Started the registration process using a temporary email address.
3. Reached the OTP verification page.
4. Received the OTP through the temporary email inbox.
5. Entered an invalid OTP for testing.
6. Captured and inspected the HTTP response using Burp Suite.
7. Observed that the original response returned `401 Unauthorized`.
8. Modified the HTTP response locally in Burp Suite to use `201 Created` with a modified response body.
9. Forwarded the modified response to the browser.
10. Observed that the registration process proceeded.
11. Account creation and successful login were subsequently reported.

**Note:** The exact OTP value, email address and sensitive request data are intentionally excluded.

## 5. Observations

During testing, the following behaviour was observed:

- The application provided an OTP verification step during registration.
- An invalid OTP submission returned `401 Unauthorized`.
- After local response modification, the browser proceeded with the registration flow.
- Account creation and successful login were reported after the modified response.

## 6. Technical Analysis

The testing demonstrated that modifying an HTTP response locally can affect how the browser handles the registration flow.

However, a successful browser-side transition does not necessarily mean that the server accepted an invalid OTP.

The actual email verification status and server-side OTP validation behaviour have not yet been independently confirmed.

## 7. Expected Result

The application should reject an invalid OTP and prevent the email verification process from being completed.

OTP verification should be performed and enforced by the server rather than relying on the response displayed by the browser.

## 8. Actual Result

The original invalid OTP submission returned `401 Unauthorized`.

After modifying the response locally to use `201 Created`, the registration flow proceeded.

Account creation and successful login were reported, but the server-side email verification status remains unconfirmed.

## 9. Potential Security Impact

If the server accepts an account as verified without validating the submitted OTP, an unauthorized user may be able to bypass email ownership verification.

Possible consequences include:

- Registration without successfully verifying email ownership.
- Circumvention of email verification requirements.
- Access to functionality restricted to verified accounts, depending on the application's implementation.

These impacts have not yet been independently established.

## 10. Result

**Current Classification:** Potential OTP Verification Issue

The test demonstrated that local HTTP response manipulation affected the registration flow.

Further authorized testing is required to determine whether the server independently enforces OTP verification.

No confirmed server-side OTP bypass is being claimed at this stage.

## 11. Evidence

Screenshots and video evidence have not been attached to this initial report.

Evidence can be added later, including:

- OTP verification page.
- Original HTTP response.
- Locally modified HTTP response.
- Registration flow after modification.

All evidence must be sanitized before being included in a public repository.

## 12. Suggested Remediation

- Validate OTPs on the server.
- Do not trust client-side response modifications.
- Maintain verification status on the server.
- Ensure OTPs expire after their validity period.
- Prevent successful reuse of an OTP.
- Apply suitable limits to repeated OTP attempts.
- Verify account permissions on protected server-side operations.

## 13. What I Learned

- How OTP verification works during account registration.
- How to inspect HTTP responses using Burp Suite.
- What `401 Unauthorized` and `201 Created` indicate.
- How local response manipulation can affect browser behaviour.
- Why server-side validation is important.
- Why additional testing is necessary before confirming a vulnerability.

---

**Practical Status:** Initial Testing Completed – Further Validation Pending

**Disclosure Status:** Not Submitted
