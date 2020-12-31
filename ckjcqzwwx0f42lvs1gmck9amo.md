## Razorpay Payment Gateway Integration In Laravel in just 8 steps and storing the payment details.

This time I will tell you guys how to integrate razorpay payment integration in laravel and store the payment details in our payment table only when payment is successful and return a message "Payment Successful".
We will also see how to return "Payment Failure" message when transaction is failed.

So us let is begin this tutorial.

**1. Install your laravel project.** *Use Below command to install latest version of laravel project.* **I am using laravel version 8.2.**

```
composer create-project --prefer-dist laravel/laravel RazorpayPaymentGateway
``` 

**2. Install Auth.** *Below are the commands to install auth in laravel 8.2. *
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
*If you are using using laravel version 5.6 or below use below command.*
```
php artisan make:auth
```
**3. Download the razorpay packages in your project using below command.**
```
composer require razorpay/razorpay
```
*After successfull installation you can check the composer.json file.*

![razorpay composer json file.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609409010190/OUO0MLvexg.png)

**4. Set routes** as below in *routes/web.php*.
```
route::get('/make-payment',[App\Http\Controllers\RazorpayController::class,'index']);
route::post('/do-payment/{amount}',[App\Http\Controllers\RazorpayController::class,'do_payment'])->name('do-payment');
route::post('/store-payment',[App\Http\Controllers\RazorpayController::class,'store_payment'])->name('store-payment');
```

**5. Create a controller and name it RazorpayController.** *Using the below command create a controller.*
```
php artisan make:controller RazorpayController
```
**6. Go to RazorpayController **which we created just now in *app/Http/Controllers/RazorpayController.php*. **Add the below code.**
```
<?php

namespace App\Http\Controllers;
use App\Models\Payment;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Razorpay\Api\Api;

class RazorpayController extends Controller
{
    //
    private $razorpayId = 'rzp_test_.........';  //Your Razorpay id here
    private $razorpayKey = '..................'; //Your Razorpay Key here

    public function index(){
        $amount = 100;  // we also take the amount from database.
        return view('pay',compact('amount'));
    }

    public function do_payment($amount){
//        return $amount;
        // Generate random receipt id
        $receiptId = sha1(time());

        // Create an object of razorpay
        $api = new Api($this->razorpayId, $this->razorpayKey);

        // In razorpay you have to convert rupees into paise we multiply by 100
        // Currency will be INR
        // Creating order
        $order = $api->order->create(array(
                'receipt' => $receiptId,
                'amount' => $amount * 100,
                'currency' => 'INR'
            )
        );

        // Return response on payment page
        $response = [
            'orderId' => $order['id'],
            'razorpayId' => $this->razorpayId,
            'amount' => $amount * 100,
            'name' => Auth::user()->name,
            'currency' => 'INR',
            'email' => Auth::user()->email,
            'description' => "Test Payment",
        ];

        return view('razorpay',compact('response'));

    }

    public function store_payment(Request $request){
        // Now verify the signature is correct . We create the private function for verify the signature
        $signatureStatus = $this->SignatureVerify(
            $request->rzp_signature,
            $request->rzp_paymentid,
            $request->rzp_orderid
        );

        // If Signature status is true We will save the payment response in our database
        // In this tutorial we send the response to Success page if payment successfully made
        $api = new Api($this->razorpayId, $this->razorpayKey);

        if($signatureStatus == true)
        {
            // You can create this page
            Payment::insert(['user_id'=>Auth::user()->id,'payment_id'=>$request->rzp_paymentid,'order_id'=>$request->rzp_orderid]);
            return "<h1 style='color: green'> Payment Successful </h1>";

        }
        else{
            // You can create this page
           return "<h1 style='color: red'> Payment Failed</h1>";
        }
    }

    // In this function we return boolean if signature is correct
    private function SignatureVerify($_signature,$_paymentId,$_orderId)
    {
        try
        {
            // Create an object of razorpay class
            $api = new Api($this->razorpayId, $this->razorpayKey);
            $attributes  = array('razorpay_signature'  => $_signature,  'razorpay_payment_id'  => $_paymentId ,  'razorpay_order_id' => $_orderId);
            $order  = $api->utility->verifyPaymentSignature($attributes);
            return true;
        }
        catch(\Exception $e)
        {
            // If Signature is not correct its give a excetption so we use try catch
            return false;
        }
    }
}
```
**7. Create 2 blades files** in your *resources/views *.

First file is **pay.blade.php** and write the below code to display button for payment.
```
<html>
<head>
    <style>
        .button {
            background-color: blueviolet;
            border: none;
            color: white;
            padding: 15px 32px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
        }
    </style>
</head>
<body>
<form method="POST" action="{{route('do-payment',$amount)}}">
    @csrf
    <input class="button" type="submit" value="Pay {{$amount}}">
</form>
</body>

</html>
```
Enter route in your browser in my case it is localhost/projectname/make-payment and you will see something like below.

![payment button.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609410127188/uF96QTZkQ.png)

Second file is **razorpay.balde.php**. And add the below code.
```
<!-- // Let's Click this button automatically when this page load using javascript -->
<!-- You can hide this button -->
<script src="https://checkout.razorpay.com/v1/checkout.js"></script>
<script src = "https://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
<script>
    var options = {
        "key": "{{$response['razorpayId']}}",
        "amount":"{{$response['amount']}}", // 2000 paise = INR 20
        "currency":"{{$response['currency']}}",
        "name": "{{$response['name']}}",
        "description": "{{$response['description']}}",
        "order_id": "{{$response['orderId']}}",
        "handler": function (response){
            // After payment successfully made response will come here
            // send this response to Controller for update the payment response
            // Create a form for send this data
            // Set the data in form
            document.getElementById('rzp_paymentid').value = response.razorpay_payment_id;
            document.getElementById('rzp_orderid').value = response.razorpay_order_id;
            document.getElementById('rzp_signature').value = response.razorpay_signature;

            // // Let's submit the form automatically
            document.getElementById('rzp-paymentresponse').click();
        },
        "prefill": {
            "email": "{{$response['email']}}",
        },
        "theme": {
            "color": "#f2a110"
        }
    };
    var rzp1 = new Razorpay(options);
    window.onload = function(){
        rzp1.open();
        e.preventDefault();
    };
</script>



<form action="{{route('store-payment')}}" method="POST" hidden>
    <input type="hidden" value="{{csrf_token()}}" name="_token" />
    <input type="text" class="form-control" id="rzp_paymentid"  name="rzp_paymentid">
    <input type="text" class="form-control" id="rzp_orderid" name="rzp_orderid">
    <input type="text" class="form-control" id="rzp_signature" name="rzp_signature">
    <button type="submit" id="rzp-paymentresponse" class="btn btn-primary">Submit</button>
</form>
```
*Basic steps to store the payment details in the database.*

1) Create a database.

2) Set the database credentials in the .env file.

**8) Create a payment table and model using below command.**
```
php artisan make:model Payment -m
```
Go to Payment.php in* models/Payment.php *and the columns name as below.
```
class Payment extends Model
{
    use HasFactory;

    protected $fillable = ['user_id','payment_id','order_id'];

}
```
And also the columns names in *database/imgrations/create_payment_table.php* file as below.
```
    public function up()
    {
        Schema::create('payments', function (Blueprint $table) {
            $table->id();
            $table->integer('user_id');
            $table->string('payment_id');
            $table->string('order_id');
            $table->timestamps();
        });
    }
```
Now run below command in your terminal.
```
php artisan migrate
```
That's it your ready to make your  payment via razorpay payment gateway and also storing the payment details in your database below are some screen shot.

![payment button.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609410835125/xjzlZpLFs.png)



*Click on the pay button and you will see*

![razorpay model.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609410868302/99sgj22Ip.png)

*fill the details click proceed and then enter 
the test card details and click on pay you will see below image*


![success.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609411072331/DNOeCS0KK.png)

*Click on success button and you will a message.*

![payment done.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609411129634/4bmkChOAt.png)

*Now you can check your payment table in database that an row is added that contains the payment details.*

![table payment.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609411190811/Qr2uaTXaM.png)

*And you can also check razorpay dashboard*

![razorpay authorized payment.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609411226415/uM9A998M-.png)
