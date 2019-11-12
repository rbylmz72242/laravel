KAYNAK: https://www.youtube.com/watch?v=9ugzTTvNvkM&list=PLgFB6lmeXFOqRC4Sc-RST38jboldiQdds&index=7


# LARAVEL NOTLARI

### Faydalı Kaynaklar
- [laravel-cheat-sheet](https://github.com/piotr-jura-udemy/laravel-cheat-sheet)
- [Laravel Cheatsheet](https://learninglaravel.net/cheatsheet/)
- [Laravel for Beginners 2019 by: Mr Digital](https://www.youtube.com/watch?v=9ugzTTvNvkM&list=PLgFB6lmeXFOqRC4Sc-RST38jboldiQdds)
- [Eloquent ORM](https://www.tutsmake.com/laravel-eloquent-cheat-sheet-eloquent-orm/)
- [Laravel Relationships](https://www.tutsmake.com/laravel-eloquent-relationships-inverse-relationships/)
- []()


# KURULUM

### LAMP
```BASH
sudo apt install curl vim git php apache2 mysql-server mysql-client
sudo apt install php-pear php-fpm php-dev php-zip php-curl php-xmlrpc php-gd php-mysql php-mbstring php-xml libapache2-mod-php -y
```

### NodeJS
12.11.2019 tarihi itibariyle NodeJS v13 desteklenmediği için v10 yükleniyor.
```BASH
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
nodejs -v
npm -v
```

### COMPOSER
```BASH
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
composer global require laravel/installer
```

### PATH Ayarları:
- `sudo vi /etc/environment`
- Şöyle görünebilir:  **PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"**
- Bu PATH değişkeninin başına şu kod eklenir: `$HOME/.config/composer/vendor/bin:`
- PATH="**$HOME/.config/composer/vendor/bin:**/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
- Değişikliğin devreye girmesi için şu komut çalıştırılır: `source /etc/environment`

# LARAVEL

### Yeni bir Laravel Projesine Başlama
- `laravel new /var/www/html/blog`
- veya
- `composer create-project --prefer-dist laravel/laravel /var/www/html/blog`

### Yeni Laravel Uygulamamızın ayarları
- **config/app.php** dosyası içinde timezone ve locale için ayarlar vardır.
- `.env` dosyası içinde şu sahalar doldurulur:
- `APP_NAME="Laravel Blog Uygulaması"`
- `APP_URL=http://localhost.development`   DİKKAT: Sonunda '/' karakteri YOK!
- `APP_DEBUG=true`

### APP KEY değiştirme/oluşturma
- KEY tanımı, `.env` adlı dosya içinde yer alır.
- `.env` adlı bir dosya yoksa `.env.example` adlı dosya kopyalanarak oluşturulur
- KEY oluşturma komutu: `php artisan key:generate`
- Eğer, key oluşturulmazsa uygulama güvenli olmaz.

### DB için ön hazırlık
- `vi app/Providers/AppServiceProvider.php`
- Dosya başına: `use Illuminate\Support\Facades\Scheme;`
- `boot()` bölümüne: `Schema::defaultStringLength(191);`  Böylece "Specified key was too long" hatası engellenecektir

### PROJE DİZİNİMİZ:
- `/var/www/html/lara/blog`

### Proje için APACHE ayarı
- `vi /etc/apache2/sites-enabled/000-default.conf`
- **Şunu ekle:**
- <VirtualHost *:80>
- 	ServerName laravel.development
- 	DocumentRoot /var/www/html/lara/blog/public
- </VirtualHost>
- **VEYA**
- Yukarıdaki VirtualHost tanımını içeren yeni bir dosyayı aynı dizinde oluştur. 
- Örnek: laravel-blog-deneme.conf içeriği:
- <VirtualHost *:80>
- 	ServerName laravel.development
- 	DocumentRoot /var/www/html/lara/blog/public
- </VirtualHost>
- Devreye girmesi için servisi tekrar başlat: `sudo service apache2 restart`
- Veya Devreye girmesi için ayarları tekrar okut: `sudo service apache2 reload`
- NOT: Windows'da bunun için **Host File Editor** adlı program kullanılabilir

### Gerekli dizinlere YAZMA yetkisi verilmesi
```BASH
cd /var/www/html/lara/
chmod -R g+w storage/
chmod -R g+w bootstrap/cache/
chown -R $USER:www-data /var/www/html/lara/
```

### Proje için HOSTS ayarı
- `vi /etc/hosts`
- **Şunu ekle:**
- `127.0.0.1     laravel.development`

### Tarayıcıdan Test Edelim
- http://laravel.development

### Yerel laravel Sunucusunu Başlatma
Proje dizininde olduğunuza emin olun!
```BASH
cd /var/www/html/lara/blog
php artisan serve
http://127.0.0.1:8000
```

# VERİTABANI

### DB İçin Bağlantı Kurulması
- Adminer'e girilir ve `laravel_blog` adında yeni bir DB oluşturulur
- Proje dizinine geçilir
- `.env` dosyası içinde şu sahalar doldurulur:
- `DB_DATABASE=laravel_blog`
- `DB_USERNAME=root`
- `DB_PASSWORD=root`

### SQLite kullanmak istenirse DB Ayarı
- BOŞ sqlite oluşturmak için: `touch database/myBlogs.sqlite`
- **config/database.php** içine şunlar yazılır:
- DB_CONNECTION=sqlite
- DB_DATABASE=/absolute/path/to/myBlogs.sqlite

### Migration hazırlama
- Faydalı Kaynak: https://kodumunblogu.net/detail/laravelde-veritabani-iliskileri-eloquent-relationships-islemleri
- **ORM**: Object Relational Mapper ile DB işlemleri kolayca yapılır
- **MODEL**: DB içindeki TABLO olarak düşünülmelidir
- `User` modeli, veritabanındaki `users` adlı tabloyu işaret eder.
- `Post` modeli, veritabanındaki `posts` adlı tabloyu işaret eder.
- **MIGRATION**: Veritabanındaki Tablonun Yapısıdır (Table Structure)
- **Artisan**: Laravel'in Komut Satırı arayüzüdür (CLI)
- Laravel, otomatik olarak "User" modelini tanımlar
- Tanımlar sırasında kullanılabilecek [veri tipleri burada](https://laravel.com/docs/6.x/migrations#columns)
- database/migrations/xxxxxx_xxx_create_users_table.php içine ekle:
```PHP
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('fname');
            $table->string('lname')->nulable();
            $table->string('email')->unique();
            $table->string('password');
            $table->timestamps();
        });
    }
```
Diğer dosyalar siliniyor
- Tanımladığımız **users** tablosunu işleme alabilmek için:
- Proje dizinine geçilir ve `php artisan migrate` komutu uygulanır

Tabloda değişiklik olursa, 
- önce database/migrations dizinine gidilir
- `php artisan migrate:rollback` yapılır. DİKKAT! Tablodaki tüm data silinir!
- database/migrations/xxxxxx_xxx_create_users_table.php içinde değişiklikler yapılır.
- Proje dizinine geçilir ve `php artisan migrate` komutu uygulanır

# MODEL

### Projeye yeni bir Tablo (model) ekleme
- `php artisan make:model Post -mc` yapılır. Dikkat!!! Çoğul değil!! (s yok, Posts değil)
- Böylece hem Model, migration ve controller dosyaları oluşturulmuş olur. Örnek:
  - app/Post.php
  - migrations/xxxxx.Post.php
- Kullanıcı tablosu ile migration tablosu arasında ilişki tanımlanacaksa bu Model içinde yapılır
database/migrations dizini içindeki xxx_xxx_posts_table'da:
```PHP
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('title');
            $table->mediumtext('content');
            $table->integer('user_id');  // user: tablo adı, id: o tablodaki id adlı saha
            $table->timestamps();
        });
    }
```
- `php artisan migrate` yapılır
Artık, veritabanında posts (ve users) adlı tabloları görebiliriz

### Diğer tablolarla ilişkilendirme
- Bir tablonun başka bir tablo ile olan bağlantısı için **belongTo()** kullanılır.

### Model Binding
- TODO: Yazılacak


# ROUTE

### Rooting tanımı sonrasında 404 hatası oluşursa:
- `sudo a2enmod rewrite` komutu çalıştırılır
- `vim  /etc/apache2/apache2.conf` komutu ile aşağıdaki bölümün olması sağlanır  **AllowOverride All** olduğuna emin ol!
```
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```
- Değişikliklerin devreye girmesi için: `sudo service apache2 restart`

### Rooting normal çalışırsa:
- `routes/web.php` içinde çalışılır
- `dd("mesaj");` Uygulama akışını keser (die komutu) ve mesajı gösterir
- Route'lar MODEL ve GÖRÜNÜM arasında yer alırlar
- Route'lar Controller'lara gider.
- Örnek route tanımı: `Route::get('/admin/users/{id}', function ($id) {`
- Bir değişken ile View'e yönelecek route tanımı örneği: (Aslında sayfalar/home.blade.php dosyasını çağıracak)
```PHP
Route::get('/home', function () {
	$isim = "Nuri Akman"
    return view('sayfalar.home', compact('isim'));
});


// Controller'in parametre ile çağrılması:
Route::get('/users/user/{id}', 'UserController@showUsers'});

// Route için isim verilmesi:
Route::get('/users/user/{id}', 'UserController@showUsers'})->name("KullaniciGoster");
// Bunu view içinde kullanmak için: <a href={{ route('KullaniciGoster') }}>GÖSTER</a>


// Controller tanımı:
public function showUsers($id) {
    $user = User::findOrFail($id); // ilgili ID'ye sahip kaydı getir
    return view('home', compact('user')); // view'i çağır
}

```


# VIEW

- Ekranda görüntülenen çıktı view ile sağlanır
- Route'ın sonucunda bir sayfa gösterilecekse, bu sayfa view ile yapılır
- **resources/views** dizini altındadırlar
- **SAYFAADI.blade.php** olarak isimlendirilir
- Örnekteki "isim" adlı değişkeni gösterecek view dosyası içeriği:
```
Merhaba {{ $isim }}
```

# BLADE

- Master Template
- Blade içinde değişken saha tanımlama: `@yield('BLOGICERIGI')`
- KULLANIMI:
```PHP
@extends("includes.master")   // Site ana şablonu

@include("includes.navbar")   // Bağımsız dosyadan NAVBAR çektik

@section('BLOGICERIGI')
   {{-- Bölüm içinde gösterilecekler buraya yazılır --}} 
@endsection
```
- Proje dizinine css ekleme ve kullanma:
- Kendi css ve js dosyalarımızı **public** klasörüne minified ederek atmak için:
- webpack.mix.js projede kullanılan css ve js dosyalarının minified edilmesini sağlar
- Kodu compile etmek için: `npm run dev`
- Kod değiştikçe compile etmek için: `npm run watch`
- View içinde kullanım örneği: `<link href="{{ asset('css/blog.css') }}" rel="stylesheet">`



# SCSS ve JS dosyalarının derlenmesi (webpack.mix.js=

- webpack.mix.js dosyası içinde sürüm eklendiğinde:
- `mix.js('resources/js/app.js', 'public/js').version()`
- `.sass('resources/sass/NURI.scss', 'public/css').version();`
- `.sass('resources/sass/app.scss', 'public/css').version();`  Kendi CSS dosyalarımızı böyle ekliyoruz
- View içinde kullanım örneği: `<link href="{{ mix('css/app.css') }}" rel="stylesheet">` 
- `nmp run dev` yapılınca mix.js'de belirtilen dosyalar derlenir
- `nmp run prog` yapılınca mix.js'de belirtilen dosyalar minified edilerek derlenir



# CONTROLLER

- DB'den veriyi çeker ve View içine yerleştirir
- `php artisan make:controller UserController`
- Yukarıdaki komut **app/http/Controller** dizini altına **UserController.php** dosyasını oluşturur
- **UserController.php** çağrılırken yazılacak route tanımı:
- `Route::get('/home', 'UserController@ShowUsers')`
- **UserController.php** örnek içeriği: (class içeriği)
```PHP
public function ShowUsers() {
	$sehir="Ankara";
    return view("sayfalar.kullanicilar", compact('sehir') );
}
```
Yukarıdaki kod, **resources/views/sayfalar/kullanicilar.blade.php** sayfasını cevap olarak döner

- User tablosunda kayıt arama:
```PHP
	$arrSonuc = User::where('yas', '27')->first();
    return view("sayfalar.kullanicilar", compact('arrSonuc') );
```

- Olmayan bir kayda erişim:
```PHP
    $arrSonuc = User::find(1); // ID'si 1 olan kaydı getirir
    $arrSonuc = User::findOrFail(1111); // 1111 ID'li kayıt yok. 404 hatası verecek
    return view("sayfalar.kullanicilar", compact('arrSonuc') );
```

- Controller'den View'a SONUÇ bildirerek çaşrı yapma: `return redirect()->back()->with('success', 'Kayıt başarılı');`
- View'den, bu çağrının karşılanması: 
```PHP
	@if(session()->has('success'))
		{{ session()->get('success') }} // Session değişkeni kontrolü
	@endif

	@if($errors()->any())
		// $request->validate() tarafında oluşan hatalar burada yakalanır...
		<ul>
		@foreach($errors()->all() as $error)
			<li>{{ $error }}</li>
		@endforeach
		</ul>
	@endif


	<form method="post" action="{{ route('createUser') }}">
		<input type="text" name="fname" value="{{old('fname')}}" required> // Hata sonrası forma eski değerleri doldurabilmek için
	</form>

```
- Yukarıdaki kod bağımsiz bir view olarak sisteme kaydedilerek BAŞARILI mesajı verilecek her yerde `@include('validation')` edilerek kullanılabilir.

**app/http/request** klasörüne bu request için controller oluşturma: `php artisan make:request CreateUser` Böylece, form gönderme sonrasındaki her kontrol bağımsız olarak bu dosyada yapılabilir. Bu dosyanın **Rules** başlığı kuralları tanımlar, **authorize** bölümü ise kurallar sağlandığında nasıl bir cevap döneceğini tanımlar




### Controller içinden tablodan veri çekme
- Sayfa başına ekle: `use App\User`   (Veri çekeceğimiz tablo için)
- **UserController.php** örnek içeriği: (class içeriği)
```PHP
use App\User
public function ShowUsers() {
	$arrKullanicilar = User::get(); // Tüm tablo içeriğini getirir // use App\User yazdığımız için böyle kullandık
	// VEYA: $arrKullanicilar = \App\User::get(); // Tüm tablo içeriğini getirir

	$arrKullanicilar = User::get(); // Tüm tablo içeriğini getirir
	$arrKullanicilar = User::all(); // Tüm tablo içeriğini getirir
	$arrKullanicilar = User::where('adi', 'nuri')->get(); // where adi = 'nuri'
	$arrKullanicilar = User::paginate(20); // Her ekranda 20 kayıt olacak biçimde getir
    return view("sayfalar.kullanicilar", compact('arrKullanicilar') );
}
```

- Yukarıdaki kodun, Blade şablonu üzerinden listelenmesi için:
```PHP
@section('content')
	
	@foreach($arrKullanicilar as $Kullanici)

		{{ $kullanici->adi }}		{{ $kullanici->soyadi }}   <br>

	@endforeach

	{{ $arrKullanicilar->links() }}  // Paginating yapıldığında, diğer sayfaların erişim linklerini göstermesi için
	{{ $arrKullanicilar->links('vendor.paginations.bootstrap-4') }}  // Paginating için kullanılacak view'i belirledik

@endsection
```
- Yukarıdaki Bootstrap pagination view'lerini oluşturabilmek için:
- `php artisan vendor:publish --tag=laravel-pagination`





# FACTORY
Veritabanına dummy kayıt ekleme işini yapar.

### Dummy kayıt oluşturma
- Önce sınıf oluşturalım: `php artisan make:factory PostFactory`
- **database/factories/PostFactory.php** oluştu.
- Kullanılabilecek bazı faker'ler:
```
    'name' => $faker->name,
    'email' => $faker->unique()->safeEmail,
    'email_verified_at' => now(),
    'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
    'remember_token' => Str::random(10),
    'title' => $faker->sentence,
    'body' => $faker->paragraph,
```
- `php artisan tinker`
- `factory('App\User', 10)->create();`
- 10 set USER oluşturmak için:
- `$users = factory('App\User', 10)->create();`
- Her kullanıcı için 10 adet de POST oluşturmak için:
- `$users->each(function ($user){ factory('App\Post', 10)->create([user_id]=>$user->id)};);`






# FORM Nesnesi
TODO: EKSİK
- protected guarded = ['id'];
- protected fillable = ['title', 'body'];
- **419 - Page Expired** hatasını önlemek için FORM etiketi içine içine şu yazılır: `@csrf`
- Insert İçin 
- Update İçin 
- Delete İçin 

# FORM Validation
```PHP
	$request()->validate([
		'fname' >= 'required',
		'lname' >= 'required',
		'email' >= 'required|email',
	]);

	$user = new User;
	$user->fname = $request->fname;
	$user->lname = $request->lname;
	$user->email = $request->email;
	$user->save();
	return redirect()->back()->with('success', 'Kayıt başarılı');
```


# HELPERS

- auth()
- request()

