# Routing and Rendering #

Matching requests to controller actions is performed based on convention instead of extensive configuration. There are three router classes included in the core library. They configure the Symfony router component to perform the actual routing, so you can expect the same high performance. After routing a request to the appropriate controller action, the router subsequently renders the response to ease controller testing (actions never directly return JSON or HTML):

- `Symlex\Router\Web\RestRouter` handles REST requests (JSON)
- `Symlex\Router\Web\ErrorRouter` renders exceptions as error messages (HTML or JSON)
- `Symlex\Router\Web\TwigRouter` renders regular Web pages via Twig (HTML)
- `Symlex\Router\Web\TwigDefaultRouter` is like TwigRouter but sends all requests to a default controller action (required for client-side routing e.g. with Vue.js)

It's easy to create your own custom routing/rendering based on the existing examples.

The application's HTTP kernel class initializes routing and sets optional URL/service name prefixes:
```php
<?php

namespace App\Kernel;

use DIMicroKernel\Kernel;

class WebApp extends Kernel
{
    public function __construct($appPath, $debug = false)
    {
        parent::__construct('web', $appPath, $debug);
    }

    public function init()
    {
        if ($this->debug) {
            ini_set('display_errors', 1);
        }
    }
    
    protected function setUp()
    {
        $container = $this->getContainer();

        // The error router catches errors and displays them as error pages
        $container->get('router.error')->route();

        // Routing for REST API calls
        $container->get('router.rest')->route($this->getUrlPrefix('/api/v1'), 'controller.rest.v1.');

        // All other requests are routed to a default controller action (client-side routing e.g. with Vue.js)
        $container->get('router.twig_default')->route($this->getUrlPrefix(), 'controller.web.index', 'index');

        // Uncomment the following line to enable server-side routing
        // $container->get('router.twig')->route($this->getUrlPrefix(), 'controller.web.');
    }
}
```

Routing examples based on the default configuration in `App\Kernel\WebApp`:
- All request (except those starting with `/api/v1`) will be routed to `controller.web.index` service's `indexAction(Request $request)`
- `GET /api/v1/users` will be routed to `controller.rest.v1.users` service's `cgetAction(Request $request)`
- `POST /api/v1/users` will be routed to `controller.rest.v1.users` service's `postAction(Request $request)`
- `OPTIONS /api/v1/users` will be routed to `controller.rest.v1.users` service's `coptionsAction(Request $request)`
- `GET /api/v1/users/123` will be routed to `controller.rest.v1.users` service's `getAction($id, Request $request)`
- `OPTIONS /api/v1/users/123` will be routed to `controller.rest.v1.users` service's `optionsAction($id, Request $request)`
- `GET /api/v1/users/123/comments` will be routed to `controller.rest.v1.users` service's `cgetCommentsAction($id, Request $request)`
- `GET /api/v1/users/123/comments/5` will be routed to `controller.rest.v1.users` service's `getCommentsAction($id, $commendId, Request $request)`
- `PUT /api/v1/users/123/comments/5` will be routed to `controller.rest.v1.users` service's `putCommentsAction($id, $commendId, Request $request)`

If you uncomment the `router.twig` router in exchange for `router.twig_default`, requests (except those starting with `/api/v1`) 
will be routed to matching web controller actions e.g.:
- `GET /` will be routed to `controller.web.index` service's `indexAction(Request $request)`
- `GET /foo` will be routed to `controller.web.foo` service's `indexAction(Request $request)`
- `GET /foo/bar` will be routed to `controller.web.foo` service's `barAction(Request $request)`
- `POST /foo/bar` will be routed to `controller.web.foo` service's `postBarAction(Request $request)`
