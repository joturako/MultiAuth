# MultiAuth For Laravel 5.1
---
## Installation
First, pull in the package through Composer.
```PHP
"require": {
    "kbwebs/multiauth": "dev-master"
}
```
Now you'll want to update or install via composer.
```
composer update
```
Next you open up config/app.php and replace the AuthServiceProvider with:
```
AuthServiceProvider -> Kbwebs\MultiAuth\AuthServiceProvider
```
## Authentication
Take config/auth.php and remove:
```PHP
'driver'  => 'eloquent'
'model'   => App\User::class,
'table'   => 'users'
```
and replace it with:
```PHP
'multi-auth' => [
    'admin' => [
        'driver' => 'eloquent',
        'model'  => 'App\Admin'
    ],
    'user' => [
        'driver' => 'eloquent',
        'model'  => 'App\User'
    ]
]
```
If you want to use Database instead of Eloquent you can use it as:
```PHP
'user' => [
    'driver' => 'database',
    'table'  => 'users'
]
```
## Password Resets
Next you open up config/app.php and replace the PasswordResetServiceProvider with:
```
PasswordResetServiceProvider -> Kbwebs\MultiAuth\PasswordResets\PasswordResetServiceProvider 
```
If you  want to use the password resets from this Package you will need to change this in each Model who use password resets:
```PHP
use Illuminate\Auth\Passwords\CanResetPassword;
use Illuminate\Contracts\Auth\CanResetPassword as CanResetPasswordContract;
```
to
```PHP
use Kbwebs\MultiAuth\PasswordResets\CanResetPassword;
use Kbwebs\MultiAuth\PasswordResets\Contracts\CanResetPassword as CanResetPasswordContract;
```
If you want to change the view for password reset for each auth type you can add this to the multi-auth array in config/auth.php:
```PHP
'email' => 'emails.users.password'
```
If you dont add this line, Laravel will automatically use the default path for emails.password like its defined in the password array.

**NOTE** It is very important that you replace the default service providers. 
If you do not wish to use Password resets, then remove the original Password resets server provider as it will cause errors.
