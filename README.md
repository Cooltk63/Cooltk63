To return a `List<UserLogin>` inside the `getUserDetails` method, you need to modify the method's return type to `ResponseEntity<List<UserLogin>>` and adjust the implementation accordingly. Here's how you can do it:

1. Change the return type of the method.
2. Adjust the `ResponseVO` to handle a list of `UserLogin` objects.
3. Make sure the `userService.findUsersDetailsByCapacity(capability)` returns a `List<UserLogin>`.

Here's the updated code:

```java
@RequestMapping("/index")
public ResponseEntity<List<UserLogin>> getUserDetails(@RequestParam("capability") String capability) {
    ResponseVO<List<UserLogin>> responseVO = new ResponseVO<>();
    
    try {
        List<UserLogin> userLoginList = userService.findUsersDetailsByCapacity(capability);
        
        responseVO.setStatusCode(HttpStatus.OK.value());
        responseVO.setMessage("Data fetched successfully");
        responseVO.setResult(userLoginList);
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
        responseVO.setStatusCode(HttpStatus.INTERNAL_SERVER_ERROR.value());
        responseVO.setMessage("An error occurred");
    }
    
    return new ResponseEntity<>(responseVO, HttpStatus.OK);
}
```

Ensure that your `ResponseVO` class can handle a list of `UserLogin` objects. If not, you'll need to adjust it like so:

```java
public class ResponseVO<T> {
    private int statusCode;
    private String message;
    private T result;
    
    // getters and setters
}
```

Make sure your service method `findUsersDetailsByCapacity` returns a `List<UserLogin>`:

```java
public List<UserLogin> findUsersDetailsByCapacity(String capability) {
    // Your implementation to fetch the list of UserLogin based on capability
}
```

This way, the `getUserDetails` method will correctly return a list of `UserLogin` objects wrapped in a `ResponseEntity`.