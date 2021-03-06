---
title: Scopes
template: documentation.twig::content_inner
chapter: 6
---
Scopes are a way to change the repository of the query by applying specific conditions according to your needs. You can add multiple Criteria in your repository.

``` php
<?php

namespace App\Repositories;

use Torann\LaravelRepository\Repositories\AbstractRepository;

class UsersRepository extends AbstractRepository
{
    /**
     * Specify Model class name
     *
     * @return string
     */
    protected $model = \App\User::class;

    /**
     * Filter by author attribute
     *
     * @return self
     */
    public function scopeAuthorsOnly()
    {
        return $this->addScopeQuery(function($query) {
            return $query->where('is_author', '=', true);
        });
    }
}
```

### Using the Scope in a Controller

``` php
<?php

namespace App\Http\Controllers;

use App\Repositories\UserRepository;

class UsersController extends Controller
{
    /**
     * @var UserRepository
     */
    protected $repository;

    /**
     * Create a new Controller instance.
     *
     * @param UserRepository $repository
     */
    public function __construct(UserRepository $repository)
    {
        $this->repository = $repository;
    }

    /**
     * Display a listing of authors.
     *
     * @return Response
     */
    public function index()
    {
        $authors = $this->repository->authorsOnly()->all();

        return \Response::json($authors);
    }
}
```
