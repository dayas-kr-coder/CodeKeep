### Laravel Migrations

#### 1. `course_categories` Table

```php
Schema::create('course_categories', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->timestamps();
});
```

#### 2. `course_categories` Table

```php
Schema::create('course_types', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->timestamps();
});
```

#### 3. `course_categories` Table

```php
Schema::create('courses', function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger('category_id');
    $table->unsignedBigInteger('type_id');
    $table->string('name');
    $table->timestamps();

    $table->foreign('category_id')->references('id')->on('course_categories');
    $table->foreign('type_id')->references('id')->on('course_types');
});
```

## Reversing the Migrations

To reverse the migrations for the tables `course_categories`, `course_types`, and `courses`.

```php
Schema::dropIfExists('college_categories');
Schema::dropIfExists('college_types');
Schema::dropIfExists('colleges');
```

---
