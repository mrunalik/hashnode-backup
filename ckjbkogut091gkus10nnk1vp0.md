## Store and display the location of the user in laravel.

In this blog we will see how to store the location of the user when user logs in.


**1. Create a Project**. *Type below command in your terminal.*

```
composer create-project --prefer-dist laravel/laravel UsersOnline
``` 
*You will see something like below image after sucessfull installation of your project.*

![install 1.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609331487079/bhK2klIeB.png)

**2. Now Install Auth**. *To Install Auth type below command in your terminal.Install one by one. *

```
composer require laravel/ui
``` 

![auth 1.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609331749911/eYEiMbxqZ.png)


```
php artisan ui vue --auth
``` 
```
npm install
``` 

```
npm run dev
``` 

**3. Install the location package**. *Type below command in your terminal*. 

```
composer require stevebauman/location
``` 
*You can see something like below image.*

![location package.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609331987212/KdDRotbRx.png)

*You can the package has been installed in composer.json file.*


![composer.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609332083301/I5O5BhB7n.png)

**4.  Go to app.php file** in *config/app.php* add the service provider by typing the below line in providers array.

```
    'providers' => [
        /*
         * Laravel Framework Service Providers...
         */
          Stevebauman\Location\LocationServiceProvider::class,
    ],
``` 
Also the below line in aliases in app.php

```
    'aliases' => [
        'Location'=> Stevebauman\Location\Facades\Location::class,
    ],
``` 

**5. Type the below command in your terminal. **

```
php artisan vendor:publish
``` 
*If you see something like this and press 0(ZERO) or else continue will step 6.*

![vendor 1.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609332494754/PpiIPHtrO.png)
*Successfull publish looks like below image.*

![vendor 1.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609332540951/5BPzu5g0b.png)

**6. Add location column in your user table.* **Go to database/migrations/create_user_table.php*

```
 $table->longText('location')->nullable();
``` 
**run below command in your terminal for migration.**

```
php artisan migrate
``` 
**7. Go the HomeController** in *app/Http/Controllers/HomeController.php*. Add below code

```
use App\Models\User;
use Stevebauman\Location\Facades\Location;
use Illuminate\Support\Facades\Auth;
``` 
*Make sure add above code on top.*
```
    public function index()
    {
         $user = User::findOrFail(Auth::user()->id);
        $users = User::all();
        $ip = $_SERVER['REMOTE_ADDR'];
        $data = Location::get($ip);
        $location = serialize($data);  // serializing to store in database
         $user->update(['location'=>$location]);
        $user_location = unserialize($user->location); // unserialize to display the location
        return view('home',compact('user_location'));
    }
``` 

**8. Go to home.blade.php** in *resourse/views/home.blade.php*. Add below code.

```
The Location of login user is:
{{dd($user_location)}}
``` 


Register a user and you will see something like this in the browser.


![output 2.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1609341375441/wIhiFJaoL.png)
 

 













