# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
  So you can reference another table based on id rather than having to reference
  by name or some other attribute. This way if there are changes made, they
  will automatically update on the join table too.
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
Profiles

```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
has_many: :movies, through: :favorites
has_many: :favorites
end
```

```rb
class Movie < ActiveRecord::Base
has_many: :profiles, through: :favorites
has_many: :favorites, #dependent: :destroy
end
```

```rb
class Favorite < ActiveRecord::Base
belongs_to :profile, inverse_of: :favorites
belongs_to :movie, inverse_of: :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
  Serializer filters what is viewable on returned data.
```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes: :id, :given_name, :surname, :email, :movies
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
  bundle exec rails -g scaffold Favorites profile:references, movie:references
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
We would use it in the Profiles Model and Movies Model to ensure that if
a profile is deleted, their favorites are deleted since they are dependent on
each other. Aso, if a movie is deleted, then it also has to be deleted from
favorites. It would inhibit being able to delete things as well.
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
a user has many favorites, favorites belong to one user.
```
