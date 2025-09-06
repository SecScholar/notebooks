**IDORS**
- Testing for IDORs requires two separate accounts w/ the same role
- Attacker can access data they should not have access to
**Access Control Violation**
- Testing for AC Bypass Requires Two (or more) Separate accounts w/different roles
- Attacker can use mechanism they should not be able to use.
# Real world examples
**IDORs**
"When a user visits their profile page, an API call sends the UserID value to the back-end server. The server responds with a larger set of data, including sensitive data, based on the UserID sent. If an attacker can send another user's UserID and recieve sensitive data they should not have access to, they have found an IDOR vulnerability."
**Access Control Violation**
"A user with the Admin role can access a page that allows them to see sensitive data for every active user. This mechanism is only available to users with the admin role. If a user outside of these roles is able to read sensitive data about other users, they have found an access control violation."