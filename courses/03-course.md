# Course

<!-- links -->

- [Database Schema for `Course` Table](#1-database-schema-for-course-table)
- [Eloquent Model for `Course`](#2-eloquent-model-for-course)
- [Filament Resource for `CourseResource`](#3-filament-resource-for-courseresource)
  - [CourseResource Form](#31-courseresource-form)
  - [CourseResource Table](#32-courseresource-table)

#### 1. Database Schema for `Course` Table

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

#### 2. Eloquent Model for `Course`

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

#### 3. Filament Resource for `CourseResource`

#### 3.1. CourseResource Form

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

#### 3.2. CourseResource Table

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
