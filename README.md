# laravel_links_uteis
Links úteis que me ajudaram ao longo da caminhada do aprendizado e aprimoramente do uso no framework Laravel.

### - Email customizado - Utilizando banco de dados para entradas de configurações
https://laravel-news.com/allowing-users-to-send-email-with-their-own-smtp-settings-in-laravel

### - Excelente curso na udemy que me ajudou a entender o funcionamento do Laravel
#### por mais que se saiba muito de PHP para utilizar frameworks é muito importante entender seus fundamentos.
https://www.udemy.com/course/curso-completo-do-desenvolvedor-laravel

### - Projetos Opensource em LARAVEL
#### Vitrine que está se transformado aos poucos em um Ecommerce
https://github.com/andmarruda/sbshowcase

#### Blog pessoal com excelentes recursos
https://github.com/andmarruda/sbblog

### - Códigos úteis
#### Verificando se em alguma rota existe parametros a serem setados.
```php
Route::getRoutes()->getByName('route-name')->hasParameters(); //retorna true se existe e false se não existir
```

Caso tenha sugestões para a lista por favor me envie no meu email: contato@sysborg.com.br

### - Email customizado - Utilizando banco de dados para entradas de configurações - Laravel 9.x
#### Exemplo bem simples retirado do meu repositório sbshowcase. Da um caminho para todos que como eu precisou de um recurso similar no laravel 9.x
```php
<?php

namespace App\Providers;

//use App
use Illuminate\Mail\Mailer;
use Symfony\Component\Mailer\Transport;
use Illuminate\Support\ServiceProvider;
use App\Models\EmailProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {
        $this->app->bind('custom.mailer', function($app, $parameters){
            $ep = EmailProvider::where('email', '=', $parameters['email'])->first();
            if(is_null($ep))
                throw new \Exception('Email provider not found with parameter email: '. $parameters['email']);

            $mailer = new Mailer('custom-mailer', $app->get('view'), transport::fromDsn('smtp://'. $ep->email. ':'. $ep->password. '@'. $ep->host. ':'. $ep->port), $app->get('events'));
            $mailer->alwaysFrom($ep->email, $parameters['name']);
            $mailer->alwaysReplyTo($ep->email, $parameters['name']);

            return $mailer;
        });
    }

    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
    }
}
```
