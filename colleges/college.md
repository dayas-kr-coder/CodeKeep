#### 1. `colleges` Table

```php
Schema::create('colleges', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->unsignedBigInteger('city_id');
    $table->unsignedBigInteger('state_id');
    $table->unsignedBigInteger('country_id');
    $table->timestamps();

    $table->foreign('city_id')->references('id')->on('cities')->onDelete('cascade');
    $table->foreign('state_id')->references('id')->on('states')->onDelete('cascade');
    $table->foreign('country_id')->references('id')->on('countries')->onDelete('cascade');
});
```

#### 2. `college_details` Table

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

    $table->foreign('college_id')->references('id')->on('colleges')->onDelete('cascade')
    $table->unique('college_id');
});
```

#### 3. `college_courses` Table

```php
Schema::create('college_courses', function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger('college_id');
    $table->unsignedBigInteger('course_id');
    $table->timestamps();

    $table->foreign('college_id')->references('id')->on('colleges')->onDelete('cascade');
    $table->foreign('course_id')->references('id')->on('courses')->onDelete('cascade');
    $table->unique(['college_id', 'course_id']); // Ensures each combination is unique
});
```

#### 4. `reviews` Table

```php
Schema::create('reviews', function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger('college_id');
    $table->unsignedBigInteger('user_id');
    $table->text('comment');
    $table->integer('rating')->unsigned()->default(0); // Rating out of 5 or 10, adjust as needed
    $table->timestamps();

    $table->foreign('college_id')->references('id')->on('colleges')->onDelete('cascade');
    $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
});
```
