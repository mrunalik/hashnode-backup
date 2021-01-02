## Laravel Pagination in 4 just steps.

In this tutorial we will see pagination. We will display only 5 records fetched from database and when user clicks on next button remaining 5 records will be display and also when there are no more records next button will be displayed and previous button will be enabled.

Let's start this tutorial.

**
1) Create a laravel project. Run below command in your terminal.**
```
composer create-project --prefer-dist laravel/laravel PaginationProject
``` 
*The above command will install latest version of laravel.*

*Basic Steps*

*1) Create a Database.*

*2)Set Database credentials in the .env file.*

*Now we will create a table that contains some records to display in pagination.*

**2) Create a table using below command.**
```
php artisan make:model Records -m
``` 
Go to Records.php file in *app/Models/Records.php file.*
Add the name of the columns as mentioned below.

```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Records extends Model
{
    use HasFactory;

    protected $fillable = ['title'];

}
```
Go to *database/migrations/create_records_table.php* and the columns name.
```
  public function up()
    {
        Schema::create('records', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->timestamps();
        });
    }
```
Now the migrate the table using below command.
```
php artisan migrate
```
**3) Now set the route** as below in *routes/web.php*
```
Route::get('/', function () {
    $titles = \App\Models\Records::simplePaginate(3);
    return view('welcome',compact('titles '));
});
```
**4) Go to welcome.blade.php **file in *resources/views/welcome.blade.php* and add below code
```
<!DOCTYPE html>
<html>
<head>
    <style>
        #customers {
            font-family: Arial, Helvetica, sans-serif;
            border-collapse: collapse;
            width: 100%;
        }

        #customers td, #customers th {
            border: 1px solid #ddd;
            padding: 8px;
        }

        #customers tr:nth-child(even){background-color: #f2f2f2;}

        #customers tr:hover {background-color: #ddd;}

        #customers th {
            padding-top: 12px;
            padding-bottom: 12px;
            text-align: left;
            background-color: #4CAF50;
            color: white;
        }
    </style>
</head>
<body>

<table id="customers">
    <tr>
        <th>ID</th>
        <th>Title</th>
    </tr>
    @foreach($titles as $row)
    <tr>
        <td>{{$row->id}}</td>
        <td>{{$row->name}}</td>
    </tr>
    @endforeach
</table>
{{$titles->render()}}

</body>
</html>

```
*Open the website and you will see output as below with next and previous button.*

![Screenshot_2021-01-02 Screenshot.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1609590699782/_LFlgV0DV.png)





