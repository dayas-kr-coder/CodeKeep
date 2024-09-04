# College

#### 1. Database Schema for `College` Table

```php
Schema::create('colleges', function (Blueprint $table) {
    $table->id();
    $table->unsignedBigInteger('city_id');
    $table->unsignedBigInteger('state_id');
    $table->string('name');
    $table->integer('established_year');
    $table->timestamps();

    $table->foreign('city_id')->references('id')->on('cities')->onDelete('cascade');
    $table->foreign('state_id')->references('id')->on('states')->onDelete('cascade');
});
```

#### 2. Eloquent Model for `College`

```php
public function city()
    {
        return $this->belongsTo(City::class, 'city_id');
    }

    public function state()
    {
        return $this->belongsTo(State::class, 'state_id');
    }

    public function courses()
    {
        return $this->belongsToMany(Course::class, 'college_courses')->withTimestamps();
    }

    // one-to-one relationship (college_detail)
    public function detail()
    {
        return $this->hasOne(CollegeDetail::class, 'college_id');
    }
```

#### 3. Filament Resource for `CollegeResource`

##### 3.1. CollegeResource Form

```php
public static function form(Form $form): Form
{
    return $form
        ->schema([
            TextInput::make('name')->required(),

            Select::make('city_id')
                ->label('city')
                ->searchable()
                ->preload()
                ->relationship('city', 'name'),

            Select::make('state_id')
                ->label('state')
                ->searchable()
                ->preload()
                ->relationship('state', 'name'),

            TextInput::make('established_year')->required(),

            Select::make('courses')
                ->visibleOn('create')
                ->multiple()
                ->label('Courses')
                ->preload()
                ->relationship('courses', 'name'),

            // Handling the CollegeDetail fields
            Repeater::make('detail')
                ->relationship('detail')
                ->schema([
                    TextInput::make('address'),
                    TextInput::make('phone'),
                    TextInput::make('website'),
                    TextInput::make('email'),
                    TextInput::make('description'),
                ])
                ->columns(2)
                ->defaultItems(1)
                ->minItems(1)
                ->maxItems(1)
                ->columnSpanFull(),
        ]);
}
```

##### 3.2. CollegeResource Table

```php
public static function table(Table $table): Table
{
    return $table
        ->columns([
            TextColumn::make('id')->sortable(),
            TextColumn::make('name')->searchable()->sortable(),
            TextColumn::make('address')->searchable()->sortable(),
            TextColumn::make('established_year')->searchable()->sortable(),

            TextColumn::make('detail.address')->label('Address'),
            TextColumn::make('detail.phone')->label('Phone'),
            TextColumn::make('detail.website')->label('Website'),
            TextColumn::make('detail.email')->label('Email'),

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

#### 4. CollegeResource Relation Manager

##### 4.1. CollegeResource Relation Manager form

```php
public function form(Form $form): Form
{
    return $form
        ->schema([
            Forms\Components\TextInput::make('name')->required(),
            Forms\Components\Select::make('category_id')
                ->label('Category')
                ->relationship('category', 'name')
                ->searchable()->preload(),
            Forms\Components\Select::make('type_id')
                ->label('Type')
                ->relationship('type', 'name')
                ->searchable()->preload(),
        ]);
}
```

##### 4.2. CollegeResource Relation Manager table

```php
public function table(Table $table): Table
{
    return $table
        ->recordTitleAttribute('name')
        ->columns([
            Tables\Columns\TextColumn::make('name'),
            Tables\Columns\TextColumn::make('category.name'),
            Tables\Columns\TextColumn::make('type.name'),
        ])
        ->filters([
            //
        ])
        ->headerActions([
            Tables\Actions\CreateAction::make(),
            Tables\Actions\AttachAction::make()->preloadRecordSelect(),
        ])
        ->actions([
            Tables\Actions\DetachAction::make(),
        ])
        ->bulkActions([
            // Tables\Actions\BulkActionGroup::make([
            //     Tables\Actions\DeleteBulkAction::make(),
            // ]),
        ]);
}
```
