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
			$user = User::find($id);

			return View::make('user.profile', array('user' => $user));
		}

	}

Të gjithë kontrolluesit duhet të zgjerojnë klasën `BaseController`, e ruajtur gjithashtu në direktorinë `app/controllers` dhe mund të përdoret për të vendosur logjikë të ndarë nga kontrolluesit. Klasa `BaseController` zgjeron klasën `Controller`. Tani mund të ndërtojmë një route për një kontrollues si më poshtë:

	Route::get('user/{id}', 'UserController@showProfile');

Nëse vendosni ti organizoni kontrolluesit me namespaces, duhet thjeshtë të përdorni emrin e plotë të klasës kur krijoni route-at:

	Route::get('foo', 'Namespace\FooController@method');

Mundet gjithashtu të përcaktoni emra për route-at:

	Route::get('foo', array('uses' => 'FooController@method',
											'as' => 'name'));

Për të gjeneruar një URL nga një veprim i kontrolluesit, mund të përdorni metodën `URL::action`:

	$url = URL::action('FooController@method');

Mundet gjithashtu të merrni emrin e veprimit të kontrolluesit që po egzekutohet, duke përdorur metodën `currentRouteAction`:

	$action = Route::currentRouteAction();

<a name="filtrat-e-kontrollueseve"></a>
## Filtrat e Kontrolluesëve

[Filtrat](/docs/routing#route-filters) mund të përcaktohen në route-at e kontrolluesëve, njësoj si route-at "normale":

	Route::get('profile', array('before' => 'auth',
				'uses' => 'UserController@showProfile'));


Megjithatë, mund ti përcaktoni filtrat edhe brenda kontrolluesëve:

	class UserController extends BaseController {

		/**
		 * Nis nje instance te re te kontrolluesit.
		 */
		public function __construct()
		{
			$this->beforeFilter('auth');

			$this->beforeFilter('csrf', array('on' => 'post'));

			$this->afterFilter('log', array('only' =>
								array('fooAction', 'barAction')));
		}

	}

Përmbajtja e filtrit mund të përcaktohet edhe brenda një funksioni anonim:

	class UserController extends BaseController {

		/**
		 * Nis nje instance te re te kontrolluesit.
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

Laravel ju lejon të përcaktoni një route të vetme që merret me të gjitha veprimet e një kontrolluesi, duke përdorur konvencione REST. Fillimisht, shkruani një route duke përdorur metodën `Route::controller`:

**Përcaktimi i një Kontrolluesi RESTful**

	Route::controller('users', 'UserController');

Metoda `controller` pranon dy argumenta. I pari është URI ku aplikohet kontrolluesi, ndërsa i dyti është emri i klasës së kontrolluesit. Tani ju mbetet vetëm të krijoni metoda në kontrollues me parashtesë foljen HTTP që duhet ti përgjigjet:

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

Veprimi `index` do ti përgjigjet URI-së bazë të kontrolluesit, që në rastin tonë është `users`.

Nëse veprimi i kontrolluesit përmban disa fjalë, URI-ja mund të aksesohet duke i ndarë fjalët me minus (-). Për shembull, veprimi më poshtë i kontrolluesit `UserController` do ti përgjigjej URI-së `users/admin-profile`:

	public function getAdminProfile() {}

<a name="kontrolluesit-resource"></a>
## Kontrolluesit Resource

Kontrolluesit resource e bëjnë edhe më të lehtë ndërtimin e kontrolluesëve RESTful. Për shembull, mund t'ju duhet të krijoni një kontrollues që menaxhon "fotot" e ruajtura nga aplikacioni. Duke përdorur komandën Artisan `controller:make` dhe metodën `Route::resource`, mund të ndërtojmë shumë shpejt një kontrollues të tillë.

Për ta krijuar kontrolluesin përmes Artisan, egzekutoni komandën më poshtë:

	php artisan controller:make PhotoController

Tani mund të regjistrojmë një route:

	Route::resource('photo', 'PhotoController');

Kjo route e vetme krijon disa route-a që i përgjigjen një sërë veprimesh REST në kontrolluesin resource. Gjithashtu, kontrolluesi i gjeneruar do të ketë metoda të paravendosura për secilin nga këto veprime, me shënime për URI-të që u përgjigjen dhe foljet HTTP.

**Veprimet e një Kontrolluesi Resource**

Folja     | URI                   | Aksioni      | Emri i route-ës
----------|-----------------------|--------------|---------------------
GET       | /resource             | index        | resource.index
GET       | /resource/create      | create       | resource.create
POST      | /resource             | store        | resource.store
GET       | /resource/{id}        | show         | resource.show
GET       | /resource/{id}/edit   | edit         | resource.edit
PUT/PATCH | /resource/{id}        | update       | resource.update
DELETE    | /resource/{id}        | destroy      | resource.destroy

Ndonjëherë mund t'ju duhet të përdorni vetëm disa veprime të caktuara:

	php artisan controller:make PhotoController --only=index,show

	php artisan controller:make PhotoController --except=index

Në të njëjtën formë, mund të përcaktoni vetëm disa veprimet që duhet të route-ohen:

	Route::resource('photo', 'PhotoController',
					array('only' => array('index', 'show')));

<a name="trajtimi-metodave-qe-mungojne"></a>
## Trajtimi i Metodave që Mungojnë

Mund të përcaktohet një metodë që i kap të gjitha, e cila do të egzekutohet vetëm kur asnjë metodë tjetër nuk është thërritur. Metoda duhet të quhet `missingMethod` dhe merr vektorin `parameter` si argumentin e vetëm:

**Përcaktimi i një Metode që i Kap të Gjitha**

	public function missingMethod($parameters)
	{
		//
	}