



# Create-Module
Module Create Documentation 

# Create Module Name and File 
```PHP
modules
├── CRM
│   ├── Controllers
│   ├── database
│   ├── Models  
│   ├── Providers
│   ├── Views
│   ├── Routes
│   └── Service
```


## Module seed 
ModuleTableSeeder
File Location in Permission Module ,
```PHP
 private function getModules()
    {
        return [
        
            ['id' => '150001',  'status' => '1', 'name' => 'CRM'],
        ];
    }
```
N:B php artisan Permission:seed 








# Create Service Provider 
command -> php artisan make:provider CRMServiceProvider
# Provider Use 
  File Location :: APP -> Providers -> AppServiceProvider.php 


  
  confic / app.php 
  File Location: Regi -> Module\CRM\Providers\CRMServiceProvider::class,

  
  work :: 
  ```PHP
    use Module\CRM\Providers\CRMServiceProvider;
  
    public function configureModuleProvider()
    {
        $modules = collect([]);

        if(Schema::hasTable('modules')) {
            $modules = Module::active()->get();
        }
        if($modules->where('name', 'CRM')->first() && file_exists(base_path() . '/module/CRM/routes/web.php')) {
            $this->app->bind(CRMServiceProvider::class);
        }
    }
```

    # Service Include in Module 
```PHP
    ├── Service
           └── CRMServiceProvider.php 
```
  code -> 
```PHP
<?php
namespace Module\CRM\Providers;
use Illuminate\Support\ServiceProvider;
class PermissionServiceProvider extends ServiceProvider
{
    public function register()
    {
      
    }
    public function boot()
    {
        $this->loadMigrationsFrom([
            base_path().DIRECTORY_SEPARATOR.'module'.DIRECTORY_SEPARATOR.'CRM'.DIRECTORY_SEPARATOR.'database'.DIRECTORY_SEPARATOR.'migrations',
        ]);
    }
}
```
N:B ::::: namespace Importent issue












# ROUTe LOcation  in  RouteServiceProvider
```PHP
    protected $CRN   = '';

 protected function mapModuleRoutes(){
       if ($modules->where('name', 'CRM')->first() && file_exists(base_path() . '/module/CRM/routes/web.php')) {

                Route::group(['middleware' => ['web', 'auth']], function () {
                    Route::namespace($this->CRM)->group(base_path('module/CRM/routes/web.php'));
                });
            }

 }



   ```         



  # view path add 
  confic/view 
```PHP
   'paths' => [
        resource_path('views'), //Root
        base_path('module/Account/views'), //Account Module
        base_path('module/Permission/views'), //Permission Module
        base_path('module/Production/views'), // Production Module
        base_path('module/Nagarik/views'), // Nagarik Module
        base_path('module/CRM/views'), // CRM Module
    ],

```
 


#Side bar SetUP 


Main Resource Sidebar in 
```PHP
 @if($modules->where('name', 'CRM')->first())
                @include('partials.__sidebar_CRM')
  @endif
```

  :::N:B  This siderbar location (partials.__sidebar_CRM) in CRM MOdule custom Sidebar 









# In Composer.json
```PHP
	"psr-4": {
            "Module\\": "module/",
            "App\\": "app/"
        }

```
