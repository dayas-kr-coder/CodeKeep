# Course Type

<!-- links -->

- [Database Schema for `CourseType` Table](#1-database-schema-for-coursetype-table)
- [Eloquent Model for `CourseType`](#2-eloquent-model-for-coursetype)
- [Filament Resource for `CourseTypeResource`](#3-filament-resource-for-coursetyperesource)
  - [CourseTypeResource Form](#31-coursetyperesource-form)
  - [CourseTypeResource Table](#32-coursetyperesource-table)

#### 1. Database Schema for `CourseType` Table

```php
Schema::create('course_types', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->timestamps();
});
```

#### 2. Eloquent Model for `CourseType`

```php
// get the courses
public function courses()
{
    return $this->hasMany(Course::class, 'type_id');
}
```

#### 3. Filament Resource for `CourseTypeResource`

#### 3.1. CourseTypeResource Form

```php
public static function form(Form $form): Form
{
    return $form
        ->schema([
            TextInput::make('name')->required(),
        ]);
}
```

#### 3.2. CourseTypeResource Table

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
