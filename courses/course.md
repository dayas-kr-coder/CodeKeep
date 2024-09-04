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

## Eloquent Models

#### 1. `CourseCategory` Model

```php
// get the courses
public function courses()
{
    return $this->hasMany(Course::class, 'category_id');
}
```

#### 2. `CourseType` Model

```php
// get the courses
public function courses()
{
    return $this->hasMany(Course::class, 'type_id');
}
```

#### 3. `Course` Model

```php
// get the category
public function category()
{
    return $this->belongsTo(CourseCategory::class, 'category_id');
}

// get the type
public function type()
{
    return $this->belongsTo(CourseType::class, 'type_id');
}

// get the colleges
public function colleges()
{
    return $this->belongsToMany(College::class, 'college_courses')->withTimestamps();
}
```

## Filament Resources

#### 1. `CourseCategoryResource` Resource

##### 1.1. CourseCategoryResource `Form`

```php
// form
public static function form(Form $form): Form
{
    return $form
    ->schema([
        TextInput::make('name')->required(),
    ]);
}
```

##### 1.2. CourseCategoryResource `Table`

```php
// table
public static function table(Table $table): Table
{
    return $table
        ->columns([
            TextColumn::make('id')->sortable(),
            TextColumn::make('name')->searchable()->sortable(),
            TextColumn::make('created_at')->dateTime()->toggleable(isToggledHiddenByDefault: true),
            TextColumn::make('updated_at')->dateTime()->toggleable(isToggledHiddenByDefault: true),
        ])
        ->filters([
            //
        ])
        ->actions([
            Tables\Actions\EditAction::make(),
        ])
        ->bulkActions([
            Tables\Actions\BulkActionGroup::make([
                Tables\Actions\DeleteBulkAction::make(),
            ]),
        ]);
}
```

#### 2. `CourseTypeResource` Resource

##### 2.1. CourseTypeResource `Form`

```php
public static function form(Form $form): Form
{
    return $form
        ->schema([
            TextInput::make('name')->required(),
        ]);
}
```

##### 2.2. CourseTypeResource `Table`

```php
public static function table(Table $table): Table
{
    return $table
        ->columns([
            TextColumn::make('id')->sortable(),
            TextColumn::make('name')->searchable()->sortable(),
            TextColumn::make('created_at')->dateTime()->toggleable(isToggledHiddenByDefault: true),
            TextColumn::make('updated_at')->dateTime()->toggleable(isToggledHiddenByDefault: true),
        ])
        ->filters([
            //
        ])
        ->actions([
            Tables\Actions\EditAction::make(),
        ])
        ->bulkActions([
            Tables\Actions\BulkActionGroup::make([
                Tables\Actions\DeleteBulkAction::make(),
            ]),
        ]);
}
```

#### 3. `CourseResource` Resource

##### 3.1. CourseResource `Form`

```php
public static function form(Form $form): Form
{
    return $form
        ->schema([
            TextInput::make('name')->required(),
            Select::make('category_id')
                ->label('Category')
                ->relationship('category', 'name')
                ->searchable()->preload(),
            Select::make('type_id')
                ->label('Type')
                ->relationship('type', 'name')
                ->searchable()->preload(),
        ]);
}
```

##### 3.2. CourseResource `Table`

```php
public static function table(Table $table): Table
{
    return $table
        ->columns([
            TextColumn::make('id'),
            TextColumn::make('name')->searchable()->sortable(),
            TextColumn::make('category.name'),
            TextColumn::make('type.name'),
            TextColumn::make('created_at')->dateTime()->toggleable(isToggledHiddenByDefault: true),
            TextColumn::make('updated_at')->dateTime()->toggleable(isToggledHiddenByDefault: true),
        ])
        ->filters([
            // filter by category
            Tables\Filters\SelectFilter::make('category_id')
                ->label('Category')
                ->relationship('category', 'name'),
            // filter by type
            Tables\Filters\SelectFilter::make('type_id')
                ->label('Type')
                ->relationship('type', 'name'),
        ])
        ->actions([
            Tables\Actions\EditAction::make(),
        ])
        ->bulkActions([
            Tables\Actions\BulkActionGroup::make([
                Tables\Actions\DeleteBulkAction::make(),
            ]),
        ]);
}
```
