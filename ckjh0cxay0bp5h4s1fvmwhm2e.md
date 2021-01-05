## Send Welcome email to users on creating an account using gmail in Laravel.

**1) Set up laravel project using below command.**
```
composer create-project --prefer-dist laravel/laravel SendMail
```
*This will install the latest version of laravel. I am using laravel version 8.2*

**Install Auth using below commands**
```
composer require laravel/ui
```
```
php artisan ui vue --auth
```
```
npm install
```
```
npm run dev
```

*Create database and set database credentials in your .env file.*

**2) Set the mail drivers.**  
Go to .env file and set the credentials,* Visit this link *https://mrunali.in/how-to-get-app-password-from-gmail-for-laravel-mailing* <- in this blog I have explained how to get the app password and gmail setting.*
```
APP_NAME=SendMail
```
```
MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=your_email_id
MAIL_PASSWORD=your_password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=your_email_id
MAIL_FROM_NAME="${APP_NAME}"
```

**3) Run below command in your terminal to make a mail controller.**
```
php artisan make:mail WelcomeMail
```
**4) Go to WelcomeMail.php** in *app/Mail/* and add below code.
```
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;

class WelcomeMail extends Mailable
{
    use Queueable, SerializesModels;
    public $user;

    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct($user)
    {
        //
        $this->user = $user;
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
//        return $this->view('view.name');
        return $this->subject('Welcome')->markdown('email.welcome_email');
    }
}
```
**4) Create a folder name as email** in *resources/views* and inside email folder create a blade file name as **welcome_email** and* add code for custom design, you can also make your own design.*
```
<html>
<head>
    <title>Welcome {{$user->name}}</title>
</head>
<body style="background-color: bisque">
<center>
    <h1>
        Welcome {{$user->name}}
    </h1>
    <p>Warms greetings from 'your name'</p>
</center>
</body>
</html>
```

**5) Now go to RegisterController.php** in *app/Http/Controllers/Auth/* and below code
```
    protected function create(array $data)
    {
        $user = User::create([
            'name' => $data['name'],
            'email' => $data['email'],
            'password' => Hash::make($data['password']),
        ]);

        Mail::to($data['email'])->send(new WelcomeMail($user));
        return $user;

    }
```
*Add below line of top of controller*
```
use Illuminate\Support\Facades\Mail;
```

**6) Go to *config/mail.php* and make changes in mailer array as below**
```
  'mailers' => [
        'smtp' => [
            'transport' => 'smtp',
            'host' => env('MAIL_HOST', 'smtp.gmail.com'),
            'port' => env('MAIL_PORT', 587),
            'encryption' => env('MAIL_ENCRYPTION', 'tls'),
            'username' => env('MAIL_USERNAME'),
            'password' => env('MAIL_PASSWORD'),
            'timeout' => null,
            'auth_mode' => null,
        ],
```
*That's it we are all done.
Now just open browser click register and welcome mail is send to the user.*

