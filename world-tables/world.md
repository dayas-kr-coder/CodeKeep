### Laravel Migrations

#### 1. `regions` Table

```php
Schema::create('regions', function (Blueprint $table) {
    $table->id();
    $table->string('name', 100);
    $table->text('translations')->nullable();
    $table->string('wikiDataId', 255)->nullable();
    $table->timestamps();
});
```

#### 2. `subregions` Table

```php
Schema::create('subregions', function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger('region_id');
    $table->string('name', 100);
    $table->text('translations')->nullable();
    $table->string('wikiDataId', 255)->nullable();
    $table->timestamps();

    $table->foreign('region_id')->references('id')->on('regions');
});
```

#### 3. `countries` Table

```php
Schema::create('countries', function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger('region_id')->nullable();
    $table->unsignedBigInteger('subregion_id')->nullable();
    $table->string('name', 100);
    $table->char('iso2', 2)->nullable();
    $table->char('iso3', 3)->nullable();
    $table->char('numeric_code', 3)->nullable();
    $table->string('phonecode', 255)->nullable();
    $table->string('capital', 255)->nullable();
    $table->string('currency', 255)->nullable();
    $table->string('currency_name', 255)->nullable();
    $table->string('currency_symbol', 255)->nullable();
    $table->string('tld', 255)->nullable();
    $table->string('native', 255)->nullable();
    $table->string('region', 255)->nullable();
    $table->string('subregion', 255)->nullable();
    $table->string('nationality', 255)->nullable();
    $table->text('timezones')->nullable();
    $table->text('translations')->nullable();
    $table->decimal('longitude', 11, 8)->nullable();
    $table->decimal('latitude', 10, 8)->nullable();
    $table->string('emoji', 191)->nullable();
    $table->string('emojiU', 191)->nullable();
    $table->timestamps();

    $table->foreign('region_id')->references('id')->on('regions');
    $table->foreign('subregion_id')->references('id')->on('subregions');
});
```

#### 4. `states` Table

```php
Schema::create('states', function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger('country_id');
    $table->string('name', 255);
    $table->string('country_name')->nullable();
    $table->char('country_code', 2);
    $table->char('state_code', 5);
    $table->string('type', 191)->nullable();
    $table->decimal('latitude', 10, 8)->nullable();
    $table->decimal('longitude', 11, 8)->nullable();
    $table->timestamps();

    $table->foreign('country_id')->references('id')->on('countries');
});
```

#### 5. `cities` Table

```php
Schema::create("cities", function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger("state_id");
    $table->unsignedBigInteger("country_id");
    $table->string("name", 255);
    $table->string("state_name", 255);
    $table->string("country_name", 255);
    $table->string("state_code", 255);
    $table->boolean("flag")->default(true);
    $table->char("country_code", 2);
    $table->decimal("latitude", 10, 8);
    $table->decimal("longitude", 11, 8);
    $table->string("wikiDataId", 255)->nullable();
    $table->timestamps();

    $table->foreign("state_id")->references("id")->on("states");
    $table->foreign("country_id")->references("id")->on("countries");
});
```

## Reversing the Migrations

To reverse the migrations for the tables `cities`, `states`, `countries`, `subregions`, and `regions`:

```php
Schema::dropIfExists('cities');
Schema::dropIfExists('states');
Schema::dropIfExists('countries');
Schema::dropIfExists('subregions');
Schema::dropIfExists('regions');
```

---
