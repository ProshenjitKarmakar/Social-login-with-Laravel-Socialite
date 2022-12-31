
# Laravel Social Login with Socialite  - `Socialite` 

LOGIN with 




### LOGIN With Google - `Step - 1` 
- If we want to add Social login in our website using `Socialite`. First we have to install `Socialite package` using CLI command.
```php
CLI command : composer require laravel/socialite
```
### LOGIN With Google - `Step - 2` 
- Then goto `https://console.cloud.google.com/`. And setup all functionalities.
![App Screenshot]()
![App Screenshot]()
![App Screenshot]()

### LOGIN With Google - `Step - 3` 
- Goto frontend login page. And add buttons with `route`.
```php
<div class="form_item_wrap mt-3">
    <a href="{{ route('gotogoogle') }}" class="btn btn_primary">
      <i class="fab fa-google">
        Login With Google
      </i>  
    </a>
</div>
```

### LOGIN With Google - `Step - 4` 
- Goto `route > web.php` and add routes.
```php
use App\Http\Controllers\SocialLoginController;

//for Google
Route::get('/gotogoogle', [SocialLoginController::class, 'gotogoogle'])->name('gotogoogle');
Route::get('/google/login', [SocialLoginController::class, 'googleUserInfo'])->name('googleUserInfo');
```
### LOGIN With Google - `Step - 5` 
- Goto `config > services.php` and add drivers. Client ID and Client Secret provided by google console.
```php
'google' => [
        'client_id' => '692403423965-6d0ob6dn4rbrkreptmqgqu**********.apps.googleusercontent.com',
        'client_secret' => 'GOCSPX-yMDCt8hh2Hw_sjTmxE4iJeAz-***',
        'redirect' => 'https://ecomm.proshenjit.info/google/login',
    ],
```



### LOGIN With Google - `Step - 6` 
- Goto `app > Http > Controllers > SocialLoginController` and add methodes.
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\User;
use Laravel\Socialite\Facades\Socialite;
use Illuminate\Support\Facades\Auth;
use App\Providers\RouteServiceProvider;
use Exception;

class SocialLoginController extends Controller
{
    //For Google
    public function gotogoogle(){
        return Socialite::driver('google')->redirect();
    }
    
    public function googleUserInfo(){
        $googleUser = Socialite::driver('google')->user();
        $findUser = User::where('social_id', $googleUser->id)->first();

        if($findUser){
            Auth::login($findUser);
            return redirect()->intended(RouteServiceProvider::HOME);
        }else{
            $userdata = new User();
            $userdata->name = $googleUser->name; 
            $userdata->email = $googleUser->email; 
            $userdata->social_id = $googleUser->id;
            $userdata->password = encrypt($googleUser->id);
            $userdata->role_id = 4;
            $userdata->user_source = 'Google';
            // dd($userdata);
            $userdata->save();
            Auth::login($userdata);
            return redirect()->intended(RouteServiceProvider::HOME);
        }
    }
}
```


## Screenshots

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)


# Documented by :  `Proshenjit Karmakar - Full Stack Developer`.








