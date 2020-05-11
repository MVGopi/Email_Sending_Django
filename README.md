## Email Sending in Django
____
Python provides a mail sending interface using smtplib(Simple Mail Transfer Protocol) module, Django provides an inbuilt classes over it. These classes are provided to make sending email extra quick, to help test email sending during development. Follow the procedure given below to send an email.
____
### Enable Less Secure Apps Option in Google Account Settings

* Google Account Settings
    * Security
        * Turn on Less secure app access
        
**Note : After completion of email sending turn off less secure apps option**
____
### Add smtp and gmail account details in settings.py file existed in project folder
```python
EMAIL_USE_TLS = True  
EMAIL_HOST = 'smtp.gmail.com'  
EMAIL_PORT = 587  
EMAIL_HOST_USER = 'sender_username@gmail.com'
EMAIL_HOST_PASSWORD = '********'
```
____
### Create forms.py file in app folder for generating a form with subject, email, message, file fields by using below lines of code
```python
from django import forms  
class EmailForm(forms.Form):      
    email     = forms.EmailField(label="Enter Email")
    subject  = forms.CharField(label="Enter subject", max_length = 30)
    message  = forms.CharField(label="Enter message", max_length = 100)  
    file      = forms.FileField() # for creating file input  
```
____
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
