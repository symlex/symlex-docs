# Routing and Rendering #

Matching requests to controller actions is performed based on convention instead of extensive configuration. There are three router classes included in the core library. They configure the Symfony router component to perform the actual routing, so you can expect the same high performance. After routing a request to the appropriate controller action, the router subsequently renders the response to ease controller testing (actions never directly return JSON or HTML):

 - `Symlex\Router\Web\RestRouter` handles REST requests (JSON)
 - `Symlex\Router\Web\ErrorRouter` renders exceptions as error messages (HTML or JSON)
 - `Symlex\Router\Web\TwigRouter` renders regular Web pages via Twig (HTML)
 - `Symlex\Router\Web\TwigDefaultRouter` is like TwigRouter but sends all requests to a default controller action (required for client-side routing e.g. with Vue.js)

It's easy to create your own custom routing/rendering based on the existing examples.

The application's [HTTP kernel class](https://github.com/symlex/symlex/blob/master/src/Kernel/WebApp.php)
initializes routing and sets optional URL/service name prefixes.

## Examples ##

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
