# College Detail

#### 1. Database Schema for `CollegeDetail` Table

```php
Schema::create('college_details', function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger('college_id');
    $table->text('address')->nullable();
    $table->string('phone')->nullable();
    $table->string('website')->nullable();
    $table->string('email')->nullable();
    $table->integer('established_year')->nullable();
    $table->text('description')->nullable();
    $table->timestamps();

    $table->foreign('college_id')->references('id')->on('colleges')->onDelete('cascade');
    $table->unique('college_id');
});
```

#### 2. Eloquent Model for `CollegeDetail`

```php
public function college()
    {
        return $this->belongsTo(College::class);
    }
```

#### 3. Filament Resource for `CollegeDetailResource`

#### 3.1. CollegeDetailResource Form

```php
// not required
```

#### 3.2. CollegeDetailResource Table

```php
// not required
```
