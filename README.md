Built in:
    - composer v2.8.2
    - xampp 8.2.12
    - php 8.2.12
    - mariaDB v10.4.32
    - filament v3.2

Langkah-langkah:
//buka folder root dengan vscode/cursor lalu buka terminal

01. Cek dan update composer
    - composer -v
    - composer self-update

02. Install Laravel 10x
    - composer create-project laravel/laravel="10.*" . (langsung tentukan versi laravelnya dan install di folder root yang sedang kita buka)

03. Buat database
    - phpmyadmin > new > create database (dalam case ini nama database dibuat "larament-finance")

04. Koneksikan Laravel dengan database
    - .env > DB_DATABASE= (isi sesuaikan dengan nama database yang baru dibuat)
    - .env > APP_URL= (isi sesuaikan dengan base url aplikasi tanpa ada "/" diakhir, dalam case ini base url http://127.0.0.1:8000)

05. Input skema database bawaan Laravel ke database yang sudah dibuat
    - php artisan migrate

06. Jalankan server localhost
    - php artisan serve (terminal biarkan terbuka, buat terminal baru untuk perintah lainnya)

07. Install Filament
    - composer require filament/filament:"^3.2" -W

08. Install dashboard admin FIlament
    - php artisan filament:install --panels

09. Buat user admin Filament
    - php artisan make:filament-user

10. Buat database and models
    - php artisan make:model Owner -m ("Owner" ganti dengan nama tabel yang mau dibuat, dalam case ini "Category")

11. Buat nama kolom table dan jumlah kolom yang diinginkan
    - database/migrations/create_categories_table.php
    - input:
        public function up(): void
    {
        Schema::create('categories', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->boolean('is_expense')->default(true);
            $table->string('image');
            $table->timestamps();
            $table->softDeletes();
        });
    }

12. Input perubahan/penambahan ke database
    - php artisan migrate

13. Menambahkan script untuk membuat kolom-kolom mana saja yang bisa diisi
    - app > Models > Category.php:
        - class Category extends Model
          {
            use HasFactory;

            protected $fillable = [
            'name',
            'is_expense',
            'image'
            ];
          }

14. Membuat Filament Resource (agar tampil di web)
    - php artisan make:filament-resource Patient --generate ("Patient" ganti dengan nama tabel yang sudah dibuat sebelumnya, dalam case ini "Category")

15. Membuat symbolic link
    - php artisan storage:link

16. Merubah urutan kolom di function table (tukar tempat Tables\Colums dan sesuaikan urutan yang diinginkan)
    - app > FIlament > Resource > CategoryResource.php
        Tables\Columns\ImageColumn::make('image'),
        Tables\Columns\TextColumn::make('name')
            ->searchable(),
        Tables\Columns\IconColumn::make('is_expense')
            ->boolean(),
        Tables\Columns\TextColumn::make('created_at')
            ->dateTime()
            ->sortable()
            ->toggleable(isToggledHiddenByDefault: true),
        Tables\Columns\TextColumn::make('updated_at')
            ->dateTime()
            ->sortable()
            ->toggleable(isToggledHiddenByDefault: true),
        Tables\Columns\TextColumn::make('deleted_at')
            ->dateTime()
            ->sortable()
            ->toggleable(isToggledHiddenByDefault: true),

17. 