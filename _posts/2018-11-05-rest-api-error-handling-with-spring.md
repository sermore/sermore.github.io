---
layout: post
title:  "REST API Error handling with Spring"
date:   2018-11-05 12:00:00 +0100
categories: java spring rest
comments: true
---

This post is a follow up on the excellent article by Bruno Leite [Guide to Spring Boot REST API Error Handling](https://www.toptal.com/java/spring-boot-rest-api-error-handling), about the error handling implementation strategy in Spring MVC.

Here we'll apply some minor improvements over Bruno's implementation of the class `RestExceptionHandler`. 

The class is the focal point of the package, as its singleton instance will be called by Spring whenever a registered exception is raised during the flow of a web request. 
Exceptions are handled in 2 slightly different modes:
* Overriding a method: The class extends `ResponseEntityExceptionHandler`, which is an abstract base for `@ControllerAdvice` annotated classes; a number of exceptions are already handled by the parent class, therefore it's needed to override an existing method in order to change this default behavior; 
* Decorating a method with `@ExceptionHandler` annotation: Exceptions not managed in the parent class are handled by adding new methods decorated with the mentioned annotation;

### Simplification of @ExceptionHandler annotated methods

The first improvement is the substitution of the class annotation `@ControllerAdvice` with the more specific `@RestControllerAdvice`, which is simply a composition of `@ControllerAdvice` and `@ResponseBody` class annotations.
The result is that `@ResponseBody` is applied by default to all class's methods, and this permits a little simplification of the methods annotated with `@ExceptionHandler`.
These methods now return directly the `ApiError` object instead of using `ResponseEntity<Object>` and define the HTTP response status with the use of `@ResponseStatus` annotation.

### Removal of the HTTP status from ApiError

The second improvement is the removal of the HTTP status from the `ApiError` class.
This improves the decoupling between the class and the HTTP stack, making it more suitable for reuse in cases where the communication is not based on HTTP.

Find below a small excerpt of the changes applied:

{% highlight java %}
@Order(Ordered.HIGHEST_PRECEDENCE)
@RestControllerAdvice
@Slf4j
public class RestExceptionHandler extends ResponseEntityExceptionHandler {
...
{% endhighlight %}

{% highlight java %}
@ExceptionHandler(EntityNotFoundException.class)
@ResponseStatus(NOT_FOUND)
protected ApiError handleEntityNotFound(
        EntityNotFoundException ex) {
    ApiError apiError = new ApiError();
    apiError.setMessage(ex.getMessage());
    return apiError;
}
{% endhighlight %}

The code is available in the forked [github repository](https://github.com/sermore/spring-boot-exception-handling).