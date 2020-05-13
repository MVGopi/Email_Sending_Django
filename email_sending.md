## Email Sending in Django

Python provides a mail sending interface using smtplib(Simple Mail Transfer Protocol) module, Django provides an inbuilt classes over it. These classes are provided to make sending email extra quick, to help test email sending during development. Follow the procedure given below to send an email.


#### Enable Less Secure Apps Option in Google Account Settings

* Google Account Settings
    * Security
        * Turn on Less secure app access
        
![Github Image](https://github.com/MVGopi/Email_Sending_Django/blob/master/secure.JPG)

Note : After completion of email sending turn off less secure apps option

#### Add smtp and gmail account details in settings.py file existed in project folder

* EMAIL_USE_TLS to be enabled for security reasons. TLS (Transport Layer Security) encrypts all the SMTP commands.
* EMAIL_HOST is for mentioning which sever is to be connected i.e server location.
* EMAIL_PORT for giving port number of the gmsil server i.e 587 
* EMAIL_HOST_USER and EMAIL_HOST_PASSWORD are from which user the mail is going to be send and it's password. 

```python
EMAIL_USE_TLS = True  
EMAIL_HOST = 'smtp.gmail.com'  
EMAIL_PORT = 587  
EMAIL_HOST_USER = 'sender_username@gmail.com' # Your gmail ID
EMAIL_HOST_PASSWORD = '********' # Password of your gmail account
```


#### Create forms.py file in app folder for generating a form with email, subject, body, file fields by using below lines of code

```python
from django import forms  
class EmailForm(forms.Form):      
    email     = forms.EmailField(label = '', max_length = 40, 
    	        widget=forms.EmailInput(attrs={'placeholder':'Enter Sender Email'}))
    subject  = forms.CharField(label = '', max_length = 60, 
    	       widget=forms.TextInput(attrs={'placeholder':'Enter Subject of Email'}))
    body  = forms.CharField(label = '', max_length = 100, 
    	    widget = forms.Textarea(attrs = {'placeholder':'Enter Email Body'}))  
    file      = forms.FileField(label = '') # for creating file input
 
```


#### In views.py from app folder create a view for sending an email as shown below

* In this case myProject is projectname and email_sending is appname
	* Imported settings from myProject to access host email address and static file path
	* Imported EmailForm from forms.py in email_sending app
* EmailMessage is the classname in django.core.mail used for sending an email this class needs subject of email, body of email, sender mail, receiver mail parameters
	* attch_file() is function of EmailMessage class to send an attacment along with email, It requires file path
	
```python
from django.shortcuts import render
from django.http import HttpResponse
from email_sending.forms import EmailForm
from myProject import settings
from django.core.mail import EmailMessage

# Create your views here.

def sendMail(request):
	if request.method == 'POST':
		to_mail = request.POST['email']
		from_mail = settings.EMAIL_HOST_USER
		email_sub = request.POST['subject']
		email_body = request.POST['body']
		file_name = request.POST['file']
		mail = EmailMessage(email_sub, email_body, from_mail, [to_mail])
		mail.attach_file(settings.STATIC_ROOT+settings.MEDIA_URL+file_name)
		mail.send()
		return HttpResponse("<h3 style = 'color:green'>Email sent successfully..!!!</h3>")
	email_form = EmailForm()
	return render(request, 'email.html', {'form' : email_form})
```


#### Create email.html file in templates folder existed in app folder with below lines of html code

```html
<!DOCTYPE html>
<html>
<head>
	<title>Send Mail</title>
</head>
<body>
<h3>Email Sending</h3>
<form action="{% url 'send_mail' %}" method="POST">
	{% csrf_token %}
	{{ form.as_p }}
	<button type="submit">Send</button>  
</form>
</body>
</html>
```


#### Now include sendMail view in urls.py existed in project folder

```python
from django.contrib import admin
from django.urls import path
from email_sending import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.sendMail, name = 'send_mail')
]
```


#### Run the server and check in browser will get an form for entering details to send an email as shown below

![Github Image](https://github.com/MVGopi/Email_Sending_Django/blob/master/email_form.JPG)


#### Enter information to send an email in the form and click on send, So that your email is sent successfully

* Confirm that receiver got an email along with attachment, Thus how we can send an email using Django. For more information about this email sending can check the below references given


### Reference

[Email Sending Using Django Reference Link](https://docs.djangoproject.com/en/3.0/topics/email/)
