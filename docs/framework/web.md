# Web Controller

Symlex controllers are plain PHP classes by default. They have to be added as service to `app/config/web.yml` (Twig) or `app/config/rest.yml` (REST):

```yaml
controller.rest.v1.users:
    class: App\Controller\Rest\V1\UsersController
    public: true
    arguments: [ "@service.session", "@model.factory", "@form.factory" ]
    calls:
        - [ setMailService, [ "@service.mail" ]]
```

*Note: In Symfony and many other frameworks, controllers aren't services by default. Some developers are used to give 
controllers direct access to the service container instead of using dependency injection, which makes testing more 
difficult and leads to less portable code (framework lock-in).*

The routers pass on the request instance to each matched controller action as last argument. It contains request parameters and headers: http://symfony.com/doc/current/book/http_fundamentals.html#requests-and-responses-in-symfony

**Web controller actions** can either return nothing (the matching Twig template will be rendered), an array (the Twig template can access the values as variables) or a string (redirect URL). Twig's template base directory can be configured in `app/config/twig.yml` (`twig.path`). The template filename is matching the request route: `[twig.path]/[controller]/[action].twig`. If no controller or action name is given, `index` is the default (the response to `/` is therefore rendered using `index/index.twig`).

Example: [IndexController](https://github.com/symlex/symlex/blob/master/src/Controller/Web/IndexController.php)
