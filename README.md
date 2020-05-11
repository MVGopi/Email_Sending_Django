## Email Sending in Django

Python provides a mail sending interface using smtplib(Simple Mail Transfer Protocol) module, Django provides an inbuilt classes over it. These classes are provided to make sending email extra quick, to help test email sending during development. Follow the procedure given below to send an email.

#### Enable Less Secure Apps Option in Google Account Settings
* Google Account Settings
    * Security
        * Turn on Less secure app access
**Note : After completion of email sending turn off less secure apps option**

The email sending along with file attachments class of django can be accessed as shown below

```python
from django.core.mail import EmailMessage
email = EmailMessage(
    'Subject of Email',
    'Body goes here',
    'from@example.com',
    ['to@example.com'])
email.attach_file('file_path')
email.send()
```
