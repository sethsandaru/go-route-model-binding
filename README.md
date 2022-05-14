# go-route-model-binding

Go Gin + gORM Route Model Binding

Inspired from [Route Model Binding of Laravel](https://laravel.com/docs/9.x/routing#route-model-binding), definitely that Route Model Binding will be a big help for me/you guys to bind a model instance for specific routes.

Eg: `users/:user` => Find the `User` model instance

- If exists: bind it to the Gin's context via `c.Set` so you can retrieve it 
- If not: return 404 resource not found

It definitely makes our life easier, doesn't it? I hate to do like: "find record, not exists/error => return 404" in the controller, cuz it's not fun and make my controller's methods a bit longer

## Inject via Middleware

Middleware: [route_model_binding.go](https://github.com/sethsandaru/go-route-model-binding/blob/main/route_model_binding.go)

You need to set this middleware to your Gin's routes. For my use case, I added this middleware to the authenticated route groups.

## Mapping - Explicit Binding

Since it won't be as smart as Laravel's way. We need to define a map in order to map your route param with a gORM model.

```go
var routeModelMapping = map[string]modelMapping{
	"entity": makeModelMapping(&models.Entity{}, "uuid"),
	"user": makeModelMapping(&model.User{}, "uuid"),
	// ...
}
```

## Access from Controllers

I love the term Controller and always using MVC in all of my applications.

```go
func (controller *entityController) Show(c *gin.Context) {
	entity, _ := c.Get("entity")
	respondOk(c, entity)
}
```

```go
func (controller *userController) Show(c *gin.Context) {
	user, _ := c.Get("user")
	respondOk(c, user)
}
```

In order to get the correct type, you have to do typeAssertion - guide [here](https://golangcode.com/convert-interface-to-number/)

## License
MIT

## Made by
@sethsandaru with Love - Base project: [Pheasant](https://github.com/sethsandaru/pheasant)
