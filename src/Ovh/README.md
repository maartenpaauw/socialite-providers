# OVH

```bash
composer require socialiteproviders/ovh
```

## Installation & Basic Usage

Please see the [Base Installation Guide](https://socialiteproviders.com/usage/), then follow the provider specific instructions below.

## Specific configuration

Follow [this link](https://github.com/ovh/php-ovh#supported-apis) to create your application's credentials at OVH's.

First choose the "endpoint" (which is the service and the country) then click on the link "Create application credentials" under the right endpoint.

### Add configuration to `config/services.php`

```php
'ovh' => [
    'client_id' => env('OVH_APP_KEY'),
    'client_secret' => env('OVH_APP_SECRET'),
    'endpoint' => env('OVH_ENDPOINT'),
    'redirect' => env('OVH_REDIRECT_URI'),
],
```

### Add provider event listener

#### Laravel 11+

In Laravel 11, the default `EventServiceProvider` provider was removed. Instead, add the listener using the `listen` method on the `Event` facade, in your `AppServiceProvider` `boot` method.

* Note: You do not need to add anything for the built-in socialite providers unless you override them with your own providers.

```php
Event::listen(function (\SocialiteProviders\Manager\SocialiteWasCalled $event) {
    $event->extendSocialite('ovh', \SocialiteProviders\Ovh\Provider::class);
});
```
<details>
<summary>
Laravel 10 or below
</summary>
Configure the package's listener to listen for `SocialiteWasCalled` events.

Add the event to your `listen[]` array in `app/Providers/EventServiceProvider`. See the [Base Installation Guide](https://socialiteproviders.com/usage/) for detailed instructions.

```php
protected $listen = [
    \SocialiteProviders\Manager\SocialiteWasCalled::class => [
        // ... other providers
        \SocialiteProviders\Ovh\OvhExtendSocialite::class.'@handle',
    ],
];
```
</details>

### Usage

You should now be able to use the provider like you would regularly use Socialite (assuming you have the facade installed):

```php
return Socialite::driver('ovh')->redirect();
```

### Returned User fields

- `id` (being the consumerKey)
- `nickname` (The nic-handle)
- `name`
- `email`

More fields are available under the `user` subkey:

```php
$user = Socialite::driver('ovh')->user();

$phone = $user->user['phone'];
$country = $user->user['country'];
```
