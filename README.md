Vulnerability Description in Detail :

It was observed that the application improperly validate the brarer token this is security issue where the process of verifying bearer tokens in an authentication or authorization flow is not performed correctly.

Impact  :

This Improper token validation can lead to Unauthorized access to sensative data, unauthorized actions within the system, privilege escalation and some time attacker able to compromised user accounts.

Recommendation

1.It is recommended to implement strong  Validation on token on server side.
2.Set short token lifetimes.
3.Ensure the token grants the appropriate level of access.
