 Security Base 2019-2020 is a course by University of Helsinki and MOOC.fi in collaboration with F-Secure.

* Project 1 description

In the first course project, your task is to create a web application that has at least five different flaws from the OWASP top ten list (https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf). Starter code for the project is provided on Github at https://github.com/cybersecuritybase/cybersecuritybase-project.

You may do the project without using the starter template (in a language of your own choosing). In that case, however, you must also provide guidelines for installing and running the web application on Windows, Linux and Mac (including guidelines for installing any possible required dependencies).

* Running instructions
 - git clone https://github.com/findungho/securitybase19.git
 - cd /securitybase19/project1_dungho/
 - mvn install -e 
 - java -jar target/ project1_dungho-1.0-SNAPSHOT.jar
 - Visit http://localhost:9091/ on the web browser.

--------------------------------------------------------------------------------------------------------------------------------
* Issues

FLAW 1: A2:2017 - Broken Authentication
* Description: No verification for password resetting. The attacker could get an access to user account by resetting password 
* Step to reproduce:
  1. Run java project
  2. Go to http://localhost:9091/
  3. Sign in with username "ted" and password "ted" 
  4. Go to reset password page.
  5. Enter new password and confirming.
  6. Click Submit
* How to fix: Multi-factor authentication such as FIDO. An original password must be asked from user for verification before reset.

--------------------------------------------------------------------------------------------------------------------------------
FLAW 2: A3: 2017 - Sensitive Data Exposure
* Description: Re-entering person's name for the Registration form will show the address if that person already registered for the event. 
* Step to reproduce:
  1. Run java project
  2. Go to http://localhost:9091/
  3. Sign in with username "ted" and password "ted"
  4. In the Form page enter your name and address, for example "MOOC" for Name and "Helsinki" for Address.
  5. Click Submit. A new page appears said that you have signed up for the event.
  6. Go back Form page. Enter again "MOOC" as Name then click Submit.
  7. A new page will show the MOOC's address.
* How to fix: validate user's session by adding HttpSession.validate().

--------------------------------------------------------------------------------------------------------------------------------
FLAW 3: A5:2017 - Broken Access Control / Insecure Direct Object References
* Description: A malicious user could modifies name parameter "/duplicate?name=" to view another personss address.
* Step to reproduce:
  1. Run java project
  2. Go to http://localhost:9091/
  3. Sign in with username "ted" and password "ted"
  4. In the Form page enter your name and address, for example "MOOC" for Name and "Helsinki" for Address.
  5. Click Submit. A new page appears said that you have signed up for the event.
  6. Go to http://localhost:9091/duplicate?name=MOOC
  7. A message will show that MOOC already signed at his address.
* How to fix: The parameter "name" identifies the username and it must be verified that is belong to username.

--------------------------------------------------------------------------------------------------------------------------------
FLAW 4: A7:2017 - Cross-Site Scripting / Stored XSS Attack
* Description: Attacker can upload malicious javascript to an unsuspecting user via the text field. 
* Step to reproduce:
  1. Run java project 
  2. Go to http://localhost:9091/
  3. Sign in with username "ted" and password "ted"
  4. In the form page, input your name in the Name fiel and "<script>alert('XSS')</script>" in the Address field, the click Submit.
  5. New page will appear with the message "Thank you! You have been signed up to the event!". This stores the script as an executable that will send to a victim’s browser.
  6. Go back http://localhost:9091/
  7. Type again your name in the Name field and anything in the Address field. 
  8. This will send a duplicate page as your name and browser will run the script that was stored.
* How to fix: Validate all input without accepting characters "</>".

--------------------------------------------------------------------------------------------------------------------------------
FLAW 5: A8:2013 - Cross-Site Request Forgery (CSRF)
* Description: This application does not prevent CSRF attack so that an attacker can create a malicious webpage and redirect end user to that webpage when they are authenticated. In this situation, the issue can be identified by creating a malicious webpage using POST method for Reset Password (ZAP).
* Step to reproduce:
  1. Run java project
  2. Go to http://localhost:9091/
  3. Sign in with username "ted" and password "ted"
  4. In the Form page enter your name and address, for example "MOOC" for Name and "Helsinki" for Address.
  5. Click Submit. A new page appears said that you have signed up for the event.
  6. Create a new page pwd_changed.html 
  7. Enter the location of html file into the address bar and Enter.
  8. Webpage will post the data needed for resetting password without user's virification.
* How to fix: Remove http.csrf().disable() from SecurityConfiguration.java

--------------------------------------------------------------------------------------------------------------------------------
FLAW 6: A6:2017 - Security Misconfiguration

+ Description: SecurityConfiguration has http.headers disabled. This results in OWASP Zap showing lot of header errors. 
+ How to fix: This can be fixed by removing http.headers().disable() (line 25) from code.


--------------------------------------------------------------------------------------------------------------------------------
FLAW 7: A9:2017 - Using Components with Known Vulnerabilities

The application uses an old version of the spring framework (1.4.2) which can be seen from pom.xml. This can be fixed by changing 1.4.2.RELEASE to 1.5.9.RELEASE on line 16 of pom.xml file.



# Security Base 2019-2020 / Project 1
