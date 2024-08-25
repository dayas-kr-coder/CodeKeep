# Admin Table Schema and Model Methods

This document details the schema for the `admin` and `profile_colors` tables and describes the associated Eloquent model methods.

## Profile Colors Table

Stores information about profile colors used by admins.

```php
Schema::create('profile_colors', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('hex');
});
```

### ProfileColor Model

Defines the Eloquent model for the `profile_colors` table.

```php
// In the ProfileColor model
class ProfileColor extends Model
{
    public $timestamps = false;
}
```

## Admin Table

Stores information about admin users, including their profile color, roles, and authentication details.

```php
Schema::create('admin', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('email')->unique();
    $table->unsignedBigInteger('profile_color_id')->nullable(); // same the user migration
    $table->timestamp('email_verified_at')->nullable();
    $table->string('password');
    $table->enum('role', ['super_admin', 'admin', 'user'])->default('user');
    $table->timestamps();

    $table->foreign('profile_color_id')
        ->references('id')->on('profile_colors')->onDelete('set null'); // same the user migration
});
```

### Eloquent Model Methods

#### Retrieve Profile Color

Retrieves the hex value of the profile color associated with the admin user.

```php
/**
 * Retrieve the hex value of the profile color associated with the admin user.
 *
 * @return string The hex value of the profile color
 */
public function profileColor()
{
    // Return the hex value of the profile color if it exists, otherwise return the hex of the first color
    return ProfileColor::where('id', $this->profile_color_id)->pluck('hex')->first() ?? ProfileColor::first()->hex;
}
```

#### Check Super Admin Role

Checks if the admin user has a role of 'super_admin'.

```php
/**
 * Determine if the admin is a super admin.
 *
 * @return bool True if the admin is a super admin, otherwise false
 */
public function isSuperAdmin()
{
    return $this->role === 'super_admin';
}
```
