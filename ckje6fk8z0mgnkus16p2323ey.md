## Reordering drag and drop in Laravel in just 5 steps.

In this tutorial we will see how to re order the data using laravel framework and also store the order in database using ajax.

Let's start this tutorial.

**1) Install laravel framework using below command.** *Type the below command in terminal.*
```
composer create-project --prefer-dist laravel/laravel Reordering
```
*This command will download the latest version of laravel framework.*

**2) Create a model and table using below command.** *Type the below command in terminal.*
```
php artisan make:model items -m
```
**3) Go to items.php** in *app/Models/items.php* and write the name of the columns.
```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class items extends Model
{
    use HasFactory;

    protected $fillable = ['name','order_id'];

}
```
Go to *database /migrations/create_items_table.php* and the columns of the database as below.
```
    public function up()
    {
        Schema::create('items', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->integer('order_id')->default(0);
            $table->timestamps();
        });
    }
```
*Basic steps
1) Create database.
2) Set database credentials in the .env file.*

*now migrate the table using below command. Type the below command in your terminal.*
```
php artisan migrate
```
**4) Make a controller using below command.** *Type below command in your terminal.*
```
php artisan make:controller OrderController
```
**Add below in your OrderController. ***app/Https/Controllers/OrderController.php*
```
<?php

namespace App\Http\Controllers;

use App\Models\items;
use Illuminate\Http\Request;

class OrderController extends Controller
{
    //
    

    public function update_order(Request $request){

        $items = items::all();
        foreach ($items as $item){
            foreach ($request->order as $order){
                if($order['id'] == $item->id){
                    $item->update(['order_id'=>$order['position']]);
                }
            }
        }
        return response('Update Successfully.', 200);

    }


}
```
**4) Now go to route** and add below route in *route/web.php*
```
<?php

use App\Models\items;
use Illuminate\Support\Facades\Route;

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/', function () {
    $items = items::orderBy('order_id','ASC')->get();
    return view('welcome',compact('items'));
});


Route::post('/update-order', [App\Http\Controllers\HomeController::class, 'update_order'])->name('update-order');
```

**5) Go to Welcome.blade.php** in *resources/views/welcome.blade.php* and add below code.
```
<!DOCTYPE html>

<html>

<head>

    <title>Create Drag and Droppable Datatables Using jQuery UI Sortable in Laravel</title>

    <meta name="csrf-token" content="{{ csrf_token() }}">

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">


</head>

<body>

<div class="row mt-5">

    <div class="col-md-10 offset-md-1">

        <table id="table" class="table table-bordered">

            <thead>

            <tr>

                <th width="30px">ID</th>

                <th>Name</th>

            </tr>

            </thead>

            <tbody id="tablecontents">

            @foreach($items as $item)

                <tr class="row1" data-id="{{$item->id }}">

                    <td class="pl-3">{{$item->id}}<i class="fa fa-sort"></i></td>

                    <td>{{$item->name }}</td>

                </tr>

            @endforeach

            </tbody>

        </table>

        <hr>


    </div>

</div>

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>

<script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.10.3/jquery-ui.min.js"></script>

<script type="text/javascript" src="https://cdn.datatables.net/v/dt/dt-1.10.12/datatables.min.js"></script>

<script type="text/javascript">

    $(function () {

        $("#table").DataTable();


        $( "#tablecontents" ).sortable({

            items: "tr",

            cursor: 'move',

            opacity: 0.6,

            update: function() {

                sendOrderToServer();

            }

        });


        function sendOrderToServer() {

            var order = [];

            var token = $('meta[name="csrf-token"]').attr('content');

            $('tr.row1').each(function(index,element) {

                order.push({

                    id: $(this).attr('data-id'),

                    position: index+1

                });

            });


            $.ajax({

                type: "POST",

                dataType: "json",

                url: "{{ url('update-order') }}",

                data: {

                    order: order,

                    _token: token

                },

                success: function(response) {

                    if (response.status == "success") {

                        console.log(response);

                    } else {

                        console.log(response);

                    }

                }

            });

        }

    });

</script>

</body>

</html>
```
*Now Open your browser and enter your project link and you will see something like below.*

![drag.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609499290565/9IO7Nbafx.png)


*Just click and drag the table row above or below and you are all set.*