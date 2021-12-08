## How to get GitHub OAuth Credentials and Integrate Sign In with GitHub in Laravel.

In this blog I will be explaining how to get the GitHub oauth credentials as I mentioned in my previous blog for sign in with gmail in laravel. Here is the link
*https://mrunali.in/step-by-step-guide-to-integrate-sign-with-gmail-in-laravel*
We will add some line of code to the existing code. Let's begin

First we will see how to get the oauth credential from GitHub

**1) Login in GitHub Developer Console or click on the link below**
*https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fdevelopers*
I will adding some snapshot for your references as well. Be with me till end.

![Screenshot 2021-12-08 113302.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1638945273128/TUK548C8v.jpeg)

After clicking on the link you will see a login page. Enter your email id and password of github account or you can create a new account if you don't have it already.

Once you get logged in you will see a dashboard

![Screenshot 2021-12-08 113351.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1638945400836/BNYpt4gv2.jpeg)

If you have already created app then you will see list of the app that you have created so far like me. Click on the **New OAuth App**

If you have never created any app then you **register a new application** like below snapshot

![register new.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1638945679623/CkGsDMcvS.jpeg)

Now enter few details like 

**Application Name**- *you can give any name, it is for your reference and the name will not display anywhere on your login page.*

**Homepage URL**- *enter the URL where the user will be redirected after successful sign in.*

**Application Description**- *Give some details of the application you just created like for what purpose you will be using it or anything else.*

**Call Back URL**- *Enter the callback URL. This is the URL which we will be mentioning in the .env file. GitHub returns the users to this call URL on our website after successful authentication of the user from the GitHub. And inside our project we can store the store the username of the client in our database. *

The code I have shown in my previous blogs. Link I have mentioned above.

![Screenshot 2021-12-08 113641.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1638946800990/A11YPkGaI.jpeg)

Once you are done click on ***register application***
And that's it you will get the client ID

![Screenshot 2021-12-08 113917.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1638946776348/4ZNpfMIxB.jpeg)

If you click on Generate new client secret you will receive the client secret as well


![Screenshot 2021-12-08 114024.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1638947050841/RcOT2HGHD.jpeg)

Save the details somewhere in your system.Now we will see how to use those in our laravel project. Open your peoject and go to .env file and add following code. Do not make any changes to the code which I told in my previous blog. Simply add these 3 lines in .env file.

```
GITHUB_CLIENT_ID = aad.......................................
GITHUB_SECRET = 08..............................................
GITHUB_URL = http://frontend.com/social-login/github/callback
```
*make sure the callback is same as written in route as well as mentioned in the GitHub application.*

Now open the services.app file in ***config/services.app***
add below code
```
    'github' => [
        'client_id' => env('GITHUB_CLIENT_ID'),
        'client_secret' => env('GITHUB_SECRET'),
        'redirect'      => env('GITHUB_URL'),
    ],

```
and in your login page just another a tag for sign in with GitHub as we did in my previous blog.

```
   <a href="{{url('social-login/github')}}" class="btn btn-block btn-outline-primary"><img width="25px" src="https://img.icons8.com/color/48/000000/github--v3.png"/>&nbsp;Sign In with Github</a>

```

And that's it now we have both sign in google as well as GitHub on our web application guy's. You are good to go.
