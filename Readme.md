# **diet-mail**
Send Emails with diet. 

Based on:
- [Nodemailer][1] for handling SMTP
- [ECT][2] for HTML templates.

## **Install**
```
npm install diet-mail
```

## **Usage**
Let's make a simple folder structure like this:
```
/yourApp
    index.js
    /node_modules
    /mail
        test.html
```
Setup the app in `index.js`:
```
// Require Diet
require('diet');

// Create App
var app = new App();

// Plugin diet-mail
var Mail = app.plugin('diet-mail');

// Setup Mail Instance
app.mail = new Mail({
	host	: 'smtp.mandrillapp.com', 		// smtp hostname
	port	: 587, 							// port for secure smtp
	debug	: true,							// display debug log
	from	: 'MyCompany',
	auth	: {
	    user: "you@example.com",	        // smtp username
	    pass: "123123"		                // smtp password
	},
	templates: app.path+'/mail/'            // template directory
});

// Start Application
app.start('http://localhost:8000');
```
After you made Mail Instance you can send emails with it's return value `app.mail`:
```js
app.mail({
    to: 'you@example.com',
    subject: 'Testing Diet-Mail',
    template: 'test.html',
    customKey: 'John Doe'
});
```
You can create html templates in the template directory. `/yourApp/mail/test.html` could look like this:
```html
Hello {{-this.customKey}}!
<br>
This is a test mail!
```
Notice the `{{-this.customKey}}`. Everything passed to `app.mail` is accessible from the template.

## **Options for Mail Instance**
```js
new Mail(nodemailer_options, ect_options);
```
- You can use any option that [Nodemailer][1] has by passing it as an object for the *first* argument. 
- You can use any option that [ECT][2] has by passing it as an object for the *second* argument. 

## **Options for app.mail()**
This is a shorthand for `mail.sendMail` from nodemailer. Passing anything as an object for the *first* argument will change the defaults. 
```
    app.mail({
        from: 'you@example.com',    // it's auth.user if not specified
        to: 'me@example.com',       // required. it's the recipient
        author: 'Your Name',        // Your Name <you@example.com>
        template: 'text.html'       // file name from the template directory
        generateTextFromHTML: true, // text version from html template
        customKey: 'hello world'    
            // any key is accesible from the template
            // you can assign account informations etc. to
            // customize emails.
    });
```

## **License**
(The MIT License)

Copyright (c) 2014 Halász Ádám <mail@adamhalasz.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.


  [1]: https://github.com/andris9/Nodemailer
  [2]: http://ectjs.com/