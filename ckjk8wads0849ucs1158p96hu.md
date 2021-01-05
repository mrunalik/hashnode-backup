## Insert Data Using Ajax In Laravel.

**1) Install laravel Project. Using below command.**
```
composer create-project --prefer-dist laravel/laravel Ajax
```
**
2) Set route as follows **
```
route::get('/',[App\Http\Controllers\AjaxController::class,'index'])->name('index');
route::post('/insert-chapter',[App\Http\Controllers\AjaxController::class,'insert_chapter'])->name('insert-chapter');
```

**3) Create AjaxController using below command.**
```
php artisan make:controller AjaxController
```
**
4)Go to AjaxController in *app/Http/Controllers/AjaxController.php***
```
 public function index(){
        return view('insert');
    }

  public function insert_chapter(Request $request){
        date_default_timezone_set('Asia/Kolkata');
        Chapter::insert(['name'=>$request->name,'created_at'=>date('Y-m-d h:i:s')]);
        return response()->json(
            [
                'success' => true,
                'message' => 'Data Inserted'
            ]
        );
    }
```

**5) Create a insert.blade.php in *resources/views* and add below code**
```
<html>
<body id="page-top">
<meta name="csrf-token" content="{{ csrf_token() }}">

  <form>
                                    <div class="form-group"><label>Enter Chapter Name</label>
                                        <input class="form-control form-control-lg" type="text" id="name" placeholder=" E.g Circular Motion" required=""></div>
                                    <div class="text-center mt-2">
                                        <button class="btn btn-outline-info border rounded" id="submit_chapter" type="submit">Add Chapter</button>
                                    </div>
                                </form>


<script src=
        "https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js">
</script>

<script>

    $.ajaxSetup({
        headers: {
            'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
        }
    });

    $( document ).ready(function() {
        $("#submit_chapter").click('submit',function() {
            // alert($('#name').val());
            $.ajax({
                type: "POST",
                url: '{{route('insert-chapter')}}',
                data: {
                    name : $('#name').val()
                },
                success: function(response) {
                    // alert(response.message);
                    if(response.message == 0){
                        $('#dataInserted').html("<span class=\"text-danger\"> Fill Details </span>");
                    }else{
                        $('#dataInserted').html("<span class=\"text-success\"> Set Added </span>");
                    }

                },
            });
        })
    });
</script>

</body>

</html>
```

**6) Create a Chapter Model using Below Command**
```
php artisan make:model Chapter -m
```
*
Add column name as below*
```
 protected $fillable = ['name'];
```

**7) Go to *database/migrations/create_chapter_table* and column names as below**
```
   public function up()
    {
        Schema::create('chapters', function (Blueprint $table) {
            $table->id();
            $table->longText('name');
            $table->timestamps();
        });
    }
```

*Now migrate the table using below command*

```
php artisan migrate
```

And that's it, you are all done.


