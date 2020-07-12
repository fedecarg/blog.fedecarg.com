---
title: "Building a RESTful Web API with PHP and Apify"
date: "2011-09-11"
tags: [frameworks,open-source,software-architecture,web-services,webdev]
---

![](http://kewnode.files.wordpress.com/2011/09/logo02.png "apify")[Apify](https://github.com/apify/apify-library) is a small and powerful open source library that delivers new levels of developer productivity by simplifying the creation of RESTful architectures. You can see it in action [here](http://www.youtube.com/watch?v=7ptoB0yCsDo). Web services are a great way to extend your web application, however, adding a web API to an existing web application can be a tedious and time-consuming task. Apify takes certain common patterns found in most web services and abstracts them so that you can quickly write web APIs without having to write too much code.

Apify exposes similar APIs as the Zend Framework, so if you are familiar with the Zend Framework, then you already know how to use Apify. Take a look at the [UsersController](https://github.com/apify/apify-library/blob/master/app/controllers/UsersController.php) class.

## **Building a RESTful Web API**

In Apify, Controllers handle incoming HTTP requests, interact with the model to get data, and direct domain data to the response object for display. The full request object is injected via the action method and is primarily used to query for request parameters, whether they come from a GET or POST request, or from the URL.

Creating a RESTful Web API with Apify is easy. Each action results in a response, which holds the headers and document to be sent to the user's browser. You are responsible for generating the response object inside the action method.

```
class UsersController extends Controller
{
    public function indexAction($request)
    {
        // 200 OK
        return new Response();
    }
}
```

The response object describes the status code and any headers that are sent. The default response is always 200 OK, however, it is possible to overwrite the default status code and add additional headers:

```
class UsersController extends Controller
{
    public function indexAction($request)
    {
        $response = new Response();

        // 401 Unauthorized
        $response->setCode(Response::UNAUTHORIZED);

        // Cache-Control header
        $response->setCacheHeader(3600);

        // ETag header
        $response->setEtagHeader(md5($request->getUrlPath()));

        // X-RateLimit header
        $limit = 300;
        $remaining = 280;
        $response->setRateLimitHeader($limit, $remaining);

        // Raw header
        $response->addHeader('Edge-control: no-store');

        return $response;
    }
}
```

**Content Negotiation**

Apify supports sending responses in HTML, XML, RSS and JSON. In addition, it supports JSONP, which is JSON wrapped in a custom JavaScript function call. There are 3 ways to specify the format you want:

- Appending a format extension to the end of the URL path (.html, .json, .rss or .xml)
- Specifying the response format in the query string. This means a format=xml or format=json parameter for XML or JSON, respectively, which will override the Accept header if there is one.
- Sending a standard Accept header in your request (text/html, application/xml or application/json).

The acceptContentTypes method indicates that the request only accepts certain content types:

```
class UsersController extends Controller
{
    public function indexAction($request)
    {
    	// only accept JSON and XML
        $request->acceptContentTypes(array('json', 'xml'));

        return new Response();
    }
}
```

Apify will render the error message according to the format of the request.

```
class UsersController extends Controller
{
    public function indexAction($request)
    {
        $request->acceptContentTypes(array('json', 'xml'));

    	$response = new Response();
        if (! $request->hasParam('api_key')) {
            throw new Exception('Missing parameter: api_key', Response::FORBIDDEN);
        }
        $response->api_key = $request->getParam('api_key');

        return $response;
    }
}
```

**Request**

```
GET /users.json
```

**Response**

```
Status: 403 Forbidden
Content-Type: application/json
{
    "code": 403,
    "error": {
        "message": "Missing parameter: api_key",
        "type": "Exception"
    }
}
```

**Resourceful Routes**

Apify supports REST style URL mappings where you can map different HTTP methods, such as GET, POST, PUT and DELETE, to different actions in a controller. This basic REST design principle establishes a one-to-one mapping between create, read, update, and delete (CRUD) operations and HTTP methods:

**HTTP Method**

**URL Path**

**Action**

**Used for**

GET

/users

index

display a list of all users

GET

/users/:id

show

display a specific user

POST

/users

create

create a new user

PUT

/users/:id

update

update a specific user

DELETE

/users/:id

destroy

delete a specific user

 

If you wish to enable RESTful mappings, add the following line to the index.php file:

```
try {
    $request = new Request();
    $request->enableUrlRewriting();
    $request->enableRestfulMapping();
    $request->dispatch();
} catch (Exception $e) {
    $request->catchException($e);
}
```

The RESTful UsersController for the above mapping will contain 5 actions as follows:

```
class UsersController extends Controller
{
    public function indexAction($request) {}
    public function showAction($request) {}
    public function createAction($request) {}
    public function updateAction($request) {}
    public function destroyAction($request) {}
}
```

By convention, each action should map to a particular CRUD operation in the database.

## **Building a Web Application**

Building a web application can be as simple as adding a few methods to your controller. The only difference is that each method returns a view object.

```
class PostsController extends Controller
{
    /**
     * route: /posts/:id
     *
     * @param $request Request
     * @return View|null
     */
    public function showAction($request)
    {
        $id = $request->getParam('id');
        $post = $this->getModel('Post')->find($id);
        if (! isset($post->id)) {
            return $request->redirect('/page-not-found');
        }

        $view = $this->initView();
        $view->post = $post;
        $view->user = $request->getSession()->user

        return $view;
    }

    /**
     * route: /posts/create
     *
     * @param $request Request
     * @return View|null
     */
    public function createAction($request)
    {
        $view = $this->initView();
        if ('POST' !== $request->getMethod()) {
            return $view;
        }

        try {
            $post = new Post(array(
                'title' => $request->getPost('title'),
                'text'  => $request->getPost('text')
            ));
        } catch (ValidationException $e) {
            $view->error = $e->getMessage();
            return $view;
        }

        $id = $this->getModel('Post')->save($post);
        return $request->redirect('/posts/' . $id);
    }
}
```

The validation is performed inside the Post entity class. An exception is thrown if any given value causes the validation to fail. This allows you to easily implement error handling for the code in your controller.

**Entity Class**

You can add validation to your entity class to ensure that the values sent by the user are correct before saving them to the database:

```
class Post extends Entity
{
    protected $id;
    protected $title;
    protected $text;

    // sanitize and validate title (optional)
    public function setTitle($value)
    {
        $value = htmlspecialchars(trim($value), ENT_QUOTES);
        if (empty($value) || strlen($value) < 3) {
            throw new ValidationException('Invalid title');
        }
        $this->title = $title;
    }

    // sanitize text (optional)
    public function setText($value)
    {
        $this->text = htmlspecialchars(strip_tags($value), ENT_QUOTES);
    }
}
```

**Routes**

Apify provides a slimmed down version of the Zend Framework router:

```
$routes[] = new Route('/posts/:id',
    array(
        'controller' => 'posts',
        'action'     => 'show'
    ),
    array(
        'id'         => '\d+'
    )
);
$routes[] = new Route('/posts/create',
    array(
        'controller' => 'posts',
        'action'     => 'create'
    )
);
```

**HTTP Request**

```
GET /posts/1
```

Incoming requests are dispatched to the controller "Posts" and action "show".

## **Feedback**

- If you encounter any problems, please use the [issue tracker](https://github.com/apify/apify-library/issues).
- For updates follow [@fedecarg](https://twitter.com/fedecarg) on Twitter.
- If you like Apify and use it in the wild, let me know.
