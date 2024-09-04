# Course Categories

<!-- links -->

- [Database Schema for `CourseCategory` Table](#1-database-schema-for-coursecategory-table)
- [Eloquent Model for `CourseCategory`](#2-eloquent-model-for-coursecategory)
- [Filament Resource for `CourseCategoryResource`](#3-filament-resource-for-coursecategoryresource)
  - [CourseCategoryResource Form](#31-coursecategoryresource-form)
  - [CourseCategoryResource Table](#32-coursecategoryresource-table)

#### 1. Database Schema for `CourseCategory` Table

```php
Schema::create('course_categories', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->timestamps();
});
```

#### 2. Eloquent Model for `CourseCategory`

```php
// get the courses
public function courses()
{
    return $this->hasMany(Course::class, 'category_id');
}
```

#### 3. Filament Resource for `CourseCategoryResource`

#### 3.1. CourseCategoryResource Form

```php
public static function form(Form $form): Form
{
    return $form
    ->schema([
        TextInput::make('name')->required(),
    ]);
}
```

#### 3.2. CourseCategoryResource Table

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
