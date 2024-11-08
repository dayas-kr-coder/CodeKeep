# College Reviews

#### 1. Database Schema for `CollegeReview` Table

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
