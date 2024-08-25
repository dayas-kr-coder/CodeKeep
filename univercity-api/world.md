# Database Schema

This document outlines the structure of the database tables, including regions, subregions, countries, states, and cities.

## Regions Table

Stores information about regions.

```php
Schema::create('regions', function (Blueprint $table) {
    $table->id();
    $table->string('name', 100);
    $table->text('translations')->nullable();
    $table->timestamps();
    $table->boolean('flag')->default(true);
    $table->string('wikiDataId', 255)->nullable()->comment('Rapid API GeoDB Cities');
});
```

## Subregions Table

Stores information about subregions, each linked to a region.

```php
Schema::create('subregions', function (Blueprint $table) {
    $table->id();
    $table->string('name', 100);
    $table->text('translations')->nullable();
    $table->unsignedBigInteger('region_id');
    $table->timestamps();
    $table->boolean('flag')->default(true);
    $table->string('wikiDataId', 255)->nullable()->comment('Rapid API GeoDB Cities');
    $table->foreign('region_id')->references('id')->on('regions');
});
```

## Countries Table

Stores information about countries.

```php
Schema::create('countries', function (Blueprint $table) {
    $table->id();
    $table->string('name', 100);
    $table->char('iso3', 3)->nullable();
    $table->char('numeric_code', 3)->nullable();
    $table->char('iso2', 2)->nullable();
    $table->string('phonecode', 255)->nullable();
    $table->string('capital', 255)->nullable();
    $table->string('currency', 255)->nullable();
    $table->string('currency_name', 255)->nullable();
    $table->string('currency_symbol', 255)->nullable();
    $table->string('tld', 255)->nullable();
    $table->string('native', 255)->nullable();
    $table->string('region', 255)->nullable();
    $table->unsignedBigInteger('region_id')->nullable();
    $table->string('subregion', 255)->nullable();
    $table->unsignedBigInteger('subregion_id')->nullable();
    $table->string('nationality', 255)->nullable();
    $table->text('timezones')->nullable();
    $table->text('translations')->nullable();
    $table->decimal('latitude', 10, 8)->nullable();
    $table->decimal('longitude', 11, 8)->nullable();
    $table->string('emoji', 191)->nullable();
    $table->string('emojiU', 191)->nullable();
    $table->timestamps();
    $table->boolean('flag')->default(true);
    $table->string('wikiDataId', 255)->nullable()->comment('Rapid API GeoDB Cities');
    $table->foreign('region_id')->references('id')->on('regions');
    $table->foreign('subregion_id')->references('id')->on('subregions');
});
```

### Eloquent Model Relationship for States Table

Defines a relationship where a state belongs to a country.

```php
// In the State model
public function country()
{
    return $this->belongsTo(Country::class);
}
```

## States Table

Stores information about states, which are linked to countries.

```php
Schema::create('states', function (Blueprint $table) {
    $table->id();
    $table->string('name', 255);
    $table->unsignedBigInteger('country_id');
    $table->char('country_code', 2);
    $table->string('country_name')->nullable();
    $table->char('state_code', 5);
    $table->string('type', 191)->nullable();
    $table->decimal('latitude', 10, 8)->nullable();
    $table->decimal('longitude', 11, 8)->nullable();
    $table->timestamps();
    $table->foreign('country_id')->references('id')->on('countries');
});
```

## Cities Table

Stores information about cities, which are linked to states and countries.

```php
Schema::create('cities', function (Blueprint $table) {
    $table->id();
    $table->string('name', 255);
    $table->unsignedBigInteger('state_id');
    $table->string('state_code', 255);
    $table->string('state_name', 255);
    $table->string('country_name', 255);
    $table->unsignedBigInteger('country_id');
    $table->char('country_code', 2);
    $table->decimal('latitude', 10, 8);
    $table->decimal('longitude', 11, 8);
    $table->boolean('flag')->default(true);
    $table->timestamps();
    $table->string('wikiDataId', 255)->nullable()->comment('Rapid API GeoDB Cities');
    $table->foreign('state_id')->references('id')->on('states');
    $table->foreign('country_id')->references('id')->on('countries');
});
```

### Eloquent Model Relationship for Cities Table

Defines relationships where a city belongs to a state and a country.

```php
// In the City model
public function state()
{
    return $this->belongsTo(State::class);
}

public function country()
{
    return $this->belongsTo(Country::class);
}
```
