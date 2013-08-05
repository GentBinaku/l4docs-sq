# Kontrolluesit

- [Kontrolluesit Bazë](#kontrolluesit-baze)
- [Filtrat e Kontrolluesëve](#filtrat-e-kontrollueseve)
- [Kontrolluesit RESTful](#kontrolluesit-restful)
- [Kontrolluesit Resource](#kontrolluesit-resource)
- [Trajtimi i Metodave që Mungojnë](#trajtimi-metodave-qe-mungojne)

<a name="kontrolluesit-baze"></a>
## Kontrolluesit Bazë

Në vend që të vendosni të gjithë logjikën e route-imit në skedarin `routes.php`, mund ta organizoni duke përdor klasa Kontrolluesish. Krahas organizimit, këta të fundit përfitojnë nga [Dependency Injection](/docs/ioc) automatik.

Kontrolluesit ruhen në direktorinë `app/controllers`, e cila është e regjistruar në opsionin `classmap` të skedarit `composer.json`.

Një shembull i një klase Kontrolluesi:

	class UserController extends BaseController {

		/**
		 * Shfaq profilin e një anëtari.
		 */
		public function showProfile($id)
		{
			$anetari = Anetari::find($id);

			return View::make('anetari.profili', array('anetari' => $anetari));
		}

	}

Të gjithë kontrolluesit duhet të zgjerojnë klasën `BaseController`, e ruajtur gjithashtu në direktorinë `app/controllers` dhe mund të përdoret për të vendosur logjikë të ndarë nga kontrolluesit. Klasa `BaseController` zgjeron klasën `Controller`. Tani mund të ndërtojmë një route për një kontrollues si më poshtë:

	Route::get('anetari/{id}', 'UserController@showProfile');

Nëse vendosni ti organizoni kontrolluesit me namespaces, duhet thjeshtë të përdorni emrin e plotë të klasës kur krijoni route-at:

	Route::get('foo', 'Namespace\FooController@method');

Mundet gjithashtu të përcaktoni emra për route-at:

	Route::get('foo', array('uses' => 'FooController@method',
											'as' => 'emri'));

Për të gjeneruar një URL nga një veprim i kontrolluesit, mund të përdorni metodën `URL::action`:

	$url = URL::action('FooController@method');

Mundet gjithashtu të merrni emrin e veprimit të kontrolluesit që po egzekutohet, duke përdorur metodën `currentRouteAction`:

	$veprimi = Route::currentRouteAction();

<a name="filtrat-e-kontrollueseve"></a>
## Filtrat e Kontrolluesëve

[Filtrat](/docs/routing#route-filters) mund të përcaktohen në route-at e kontrolluesëve, njësoj si route-at "normale":

	Route::get('profile', array('before' => 'auth',
				'uses' => 'UserController@showProfile'));


However, you may also specify filters from within your controller:

	class UserController extends BaseController {

		/**
		 * Instantiate a new UserController instance.
		 */
		public function __construct()
		{
			$this->beforeFilter('auth');

			$this->beforeFilter('csrf', array('on' => 'post'));

			$this->afterFilter('log', array('only' =>
								array('fooAction', 'barAction')));
		}

	}

You may also specify controller filters inline using a Closure:

	class UserController extends BaseController {

		/**
		 * Instantiate a new UserController instance.
		 */
		public function __construct()
		{
			$this->beforeFilter(function()
			{
				//
			});
		}

	}

<a name="kontrolluesit-restful"></a>
## Kontrolluesit RESTful

Laravel allows you to easily define a single route to handle every action in a controller using simple, REST naming conventions. First, define the route using the `Route::controller` method:

**Defining A RESTful Controller**

	Route::controller('users', 'UserController');

The `controller` method accepts two arguments. The first is the base URI the controller handles, while the second is the class name of the controller. Next, just add methods to your controller, prefixed with the HTTP verb they respond to:

	class UserController extends BaseController {

		public function getIndex()
		{
			//
		}

		public function postProfile()
		{
			//
		}

	}

The `index` methods will respond to the root URI handled by the controller, which, in this case, is `users`.

If your controller action contains multiple words, you may access the action using "dash" syntax in the URI. For example, the following controller action on our `UserController` would respond to the `users/admin-profile` URI:

	public function getAdminProfile() {}

<a name="kontrolluesit-resource"></a>
## Kontrolluesit Resource

Resource controllers make it easier to build RESTful controllers around resources. For example, you may wish to create a controller that manages "photos" stored by your application. Using the `controller:make` command via the Artisan CLI and the `Route::resource` method, we can quickly create such a controller.

To create the controller via the command line, execute the following command:

	php artisan controller:make PhotoController

Now we can register a resourceful route to the controller:

	Route::resource('photo', 'PhotoController');

This single route declaration creates multiple routes to handle a variety of RESTful actions on the photo resource. Likewise, the generated controller will already have stubbed methods for each of these actions with notes informing you which URIs and verbs they handle.

**Actions Handled By Resource Controller**

Verb      | Path                  | Action       | Route Name
----------|-----------------------|--------------|---------------------
GET       | /resource             | index        | resource.index
GET       | /resource/create      | create       | resource.create
POST      | /resource             | store        | resource.store
GET       | /resource/{id}        | show         | resource.show
GET       | /resource/{id}/edit   | edit         | resource.edit
PUT/PATCH | /resource/{id}        | update       | resource.update
DELETE    | /resource/{id}        | destroy      | resource.destroy

Sometimes you may only need to handle a subset of the resource actions:

	php artisan controller:make PhotoController --only=index,show

	php artisan controller:make PhotoController --except=index

And, you may also specify a subset of actions to handle on the route:

	Route::resource('photo', 'PhotoController',
					array('only' => array('index', 'show')));

<a name="trajtimi-metodave-qe-mungojne"></a>
## Trajtimi i Metodave që Mungojnë

A catch-all method may be defined which will be called when no other matching method is found on a given controller. The method should be named `missingMethod`, and receives the parameter array for the request as its only argument:

**Defining A Catch-All Method**

	public function missingMethod($parameters)
	{
		//
	}