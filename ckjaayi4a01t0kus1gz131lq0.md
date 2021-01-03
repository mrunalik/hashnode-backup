## How to display online users in laravel.

Now we will list the user that are online using Laravel. Follow the below steps.


**1**.  **Install Laravel Project**. *Type the below code in your terminal*. 
```
composer create-project --prefer-dist laravel/laravel UsersOnline
``` 
*After installation you can see something like below image.*
![install 2.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609252840658/WFWTeLl0q.png)


**2**. **Now Install Auth**. *To Install Auth type below command in your terminal*.

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

**3**. **Create Middleware**. *To create middleware type below command in terminal*.

```
php artisan make:middleware OnlineStatus
```

**4**. **Add the below code in Middleware OnlineStatus** *in app/Http/Middleware/OnlineStatus*.

```
  public function handle(Request $request, Closure $next)
    {
        if(Auth::check()){
            $expiresAt = Carbon::now()->addMinutes(5);
            Cache::put('user-online-'.Auth::user()->id,true,$expiresAt);
        }
        return $next($request);
    }
``` 
 

**5**. **Go to Kernel.php* . *You will find kernel.php in **app/Http/Kernel.php*.  Write the below line in web array.

```
 'web' => [
            \App\Http\Middleware\OnlineStatus::class,
        ],
``` 

**6**. **In User Model add the below function** in *app/Models/User.php.*

```
 public function isOnline(){
        return Cache::has('user-online-'.$this->id);
    }
``` 

**7**. **Define route**.Add the below code routes/web.php. 

```
route::get('/login','[App\Http\Controllers\userController::class, 'index']');
``` 

**8**. **Create a controller**. *To create controller type below command in your terminal*.

```
php artisan make:controller userController
``` 

**9**. **Add below code in UserController.**

```
public function index(){
$users = User::where('role',0)->simplePaginate(8);
return view('home',compact('users'));
}
``` 


10. **Go to home.blade.php**. *In resources/views/home.blade.php*. 

```
  <table class="table">
                    <thead>
                    <tr>
                        <th>Name</th>
                        <th>Email</th>
                        <th>Account Created At</th>
                    </tr>
                    </thead>
                    <tbody>
                    @foreach($users as $user)
                        @if($user->isOnline())
                            <tr>
                                <td>{{$user->name}}</td>
                                <td>{{$user->email}}</td>
                                <td>{{$user->created_at->diffForHumans()}}</td>
                            </tr>
                        @endif
                    @endforeach

                    </tbody>
                </table>
``` 





















 











