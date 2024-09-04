# College Courses

#### 1. Database Schema for `CollegeCourse` Table

```php
Schema::create('college_courses', function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger('college_id');
    $table->unsignedBigInteger('course_id');
    $table->timestamps();

    $table->foreign('college_id')->references('id')->on('colleges')->onDelete('cascade');
    $table->foreign('course_id')->references('id')->on('courses')->onDelete('cascade');

    // To ensure that a course is not assigned multiple times to the same college
    $table->unique(['college_id', 'course_id']);
});
```

#### 2. Eloquent Model for `CollegeCourse`

```php
// not required
```

#### 3. Filament Resource for `CollegeCourseResource`

#### 3.1. CollegeCourseResource Form

```php
// not required
```

#### 3.2. CollegeCourseResource Table

```php
// not required
```
