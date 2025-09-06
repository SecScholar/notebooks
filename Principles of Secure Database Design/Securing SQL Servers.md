
**What are security objectives?**

There are two main types of security objectives.

1. Identify PII like names, email, usernames, passwords, date of birth, etc. and why this data must be protected.
2. The second involves identifying and defining security objectives. This is much like locating security holes in an application.

**PII**

When the PII is collected, a decision is made on that data whether or not it should be encrypted and plans to follow through with this encryption are executed. This means any information that can be used to identify a specific person. Which is why it's very important we strive to encrypt sensitive data in our application like. 

- Usernames
- Passwords
- Customers Names
- SSN
- Tax ID
- Email Addresses 
- and more

**When should security objectives be identified?**

It's important that proper db and app security begins at the first stage of development. 

_We do this by finding out what kinds of data will be stored and how people will access that data._

_From there we can identify risks associated with storing these types of information and those kinds of access patterns._

_Once we have identified the risks we can create technical solutions to mitigate those risks that can be implemented into the application design._

**How do we identify security objectives?**

_When using legacy software design methodologies (basically anything besides SCRUM and Code First)_ 

1. We need to examine our data sets and our tables to identify if there is anything within that data set that can identify a single customer and not just from a login perspective.
2. This same info should be reviewed by the companies legal department to see if their are any other fields that they feel should be encrypted. (best to provide descriptions and sample data related to the PII collected)
3. Go through and look for potential weaknesses in the application;
    1. UN-parameterized dynamically generated SQL 
    2. drop down menus
    3. inputs that are susceptible to buffer overflow
4. The error messages that are returned to the end user when a problem is found should be reviewed as you need to ensure that you aren't returning sensitive info to the end user.
5. If the application has an API this further extends the attack surface of the application because an API will be used to exchange information with other applications within the company.
    1. _When working with APIs for data exchange, don't assume that there wouldn't be an attack against the API. Test all entry points into the API and ensure there are no overflows or under-flows that would allow an attacker access to the system._ **Even if the DB is in a secure environment with firewalls and proper permissions and group control.**
    2. 