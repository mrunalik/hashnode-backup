## Stripe Payment Gateway integration in Laravel 6.2 (PHP 7.2) using JavaScript.


Install Laravel Project using Following command 

```
composer create-project --prefer-dist laravel/laravel Projectname 6.2

```

Install stripe package in your laravel project

```
composer require stripe/stripe-php
``` 
Add below line in your **composer.json** file

```
"require": {
    "php": "^7.2",
     ...........
    "cartalyst/stripe-laravel": "11.0.*"
},
``` 
run composer update command in terminal.

Then set routes

```
route::post('/stripe','StripeController@index')->name('stripe');
route::post('/stripe/payment','StripeController@stripepost')->name('stripe.post');
``` 
Create Controller 

```
php artisan make:controller StripeController
``` 
 Write below code in Stripe Controller

```
<?php

namespace App\Http\Controllers;

use App\Course;
use App\Payment;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Validator;
use Illuminate\Http\Request;
use Stripe\Customer;
use Stripe\Stripe;
use Stripe\Charge;
use Stripe\Token;
use willvincent\Rateable\Tests\models\User;


class StripeController extends Controller
{
    //

    public function index(Request $request){
        $course = Course::findOrFail($request->course_id) // product_id;
        $coupon = $request->coupon // coupon product from database;
        $amount = $request->amount //pric of product from database;

        return view('stripe',compact('coupon','amount','course'));
    }

    public function stripepost(Request $request){
        \Stripe\Stripe::setApiKey(env('STRIPE_SECRET'));
        try {

            $token = $_POST['stripeToken'];
            $customer = Customer::create(array(
                'email' => Auth::user()->email,
                'source' => $request->stripeToken
            ));
            $charge = Charge::create(array(
                'customer' => $customer->id,
                'amount' => $request->amount * 100,
                'currency' => 'INR',
                "description" => "KEET Classroom"
            ));

               $user = Course::findOrfail($request->course_id)->user;
               Payment::insert([
                    'instructor_id'=>$user->id,
                   'user_id'=>Auth::user()->id,
                   'course_id'=>$request->course_id,
                   'coupon'=>$request->coupon,
                   'amount'=>$request->amount,
                   'payment_status'=>1,
                   'mode'=>1
               ]);

            return "payment Completed";
        }catch (\Exception $ex) {
            return $ex->getMessage();
        }


    }


}
``` 

Set Stripe key and Secret in **.env** file

```
STRIPE_KEY=pk_test_s....
 STRIPE_SECRET=sk_test_...
``` 

 Create Blade files in view folder as **stripe.blade.php**

*Add below code in **stripe.blade.php***

```
<html>

<body>
<style>

    body{
        background-color: #5b5b5b;
    }

    .spacer {
        margin-bottom: 24px;
    }


    /**
     * The CSS shown here will not be introduced in the Quickstart guide, but shows
     * how you can use CSS to style your Element's container.
     */
    .StripeElement {
        background-color: white;
        padding: 10px 12px;
        border-radius: 4px;
        border: 1px solid #ccd0d2;
        box-shadow: inset 0 1px 1px rgba(0,0,0,.075);
        -webkit-transition: box-shadow 150ms ease;
        transition: box-shadow 150ms ease;
    }

    .StripeElement--focus {
        box-shadow: 0 1px 3px 0 #cfd7df;
    }

    .StripeElement--invalid {
        border-color: #fa755a;
    }

    .StripeElement--webkit-autofill {
        background-color: #fefde5 !important;
    }

    #card-errors {
        color: #fa755a;
    }


</style>
<form method="POST" action="{{route('stripe.post')}}" id="payment-form">@csrf
    <input hidden id="course_id" name="course_id" value="{{$course->id}}">
    <input hidden id="coupon" name="coupon" value="{{$coupon}}">
    <input hidden id="amount" name="amount" value="{{$amount}}">


<div class="container justify-content-center align-items-center" style="margin-top: 10%;">
    <div class="row">
        <div class="col">
            <h4 class="text-white text-center">Congratulations! You are about to buy {{$course->title}}</h4><br>
            <div id="card-element">
                <!-- Elements will create input elements here -->
            </div>
            <div id="card-errors StripeElement" role="alert"></div>
<div class="text-center mt-3">
    <button type="submit" class="btn btn-warning btn-lg">Pay <i class="fa fa-rupee"></i> {{$amount}}</button>
</div>

        </div>

    </div>

</div>


    <!-- We'll put the error messages in this element -->





</form>
<script src="https://js.stripe.com/v3/"></script>
<script>

    var stripe = Stripe('pk_test_...');
    var elements = stripe.elements();
    var style = {
        base: {

            color: "#32325d",
        }
    };

    var card = elements.create("card", {
        style: style,
        hidePostalCode: true
    });
    card.mount("#card-element");

    // Create a token or display an error when the form is submitted.
    var form = document.getElementById('payment-form');
    form.addEventListener('submit', function(event) {
        event.preventDefault();

        stripe.createToken(card).then(function(result) {
            if (result.error) {
                // Inform the customer that there was an error.
                var errorElement = document.getElementById('card-errors');
                errorElement.textContent = result.error.message;
            } else {
                // Send the token to your server.
                stripeTokenHandler(result.token);
            }
        });
    });

    function stripeTokenHandler(token) {
        // Insert the token ID into the form so it gets submitted to the server
        var form = document.getElementById('payment-form');
        var hiddenInput = document.createElement('input');
        hiddenInput.setAttribute('type', 'hidden');
        hiddenInput.setAttribute('name', 'stripeToken');
        hiddenInput.setAttribute('value', token.id);
        form.appendChild(hiddenInput);

        // Submit the form
        form.submit();
    }
</script>
</body>
</html>
``` 

