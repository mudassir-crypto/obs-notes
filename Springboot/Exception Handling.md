
##### ErrorDetails:
```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class ErrorDetails {
    private LocalDateTime timeStamp;
    private String message;
    private String details;
}
```

##### ResourceNotFound:
```java
@ResponseStatus(value = HttpStatus.NOT_FOUND)
public class ResourceNotFound extends RuntimeException{
    public ResourceNotFound(String message) {
        super(message);
    }
}
```

##### APIException:
```java
@Getter
@AllArgsConstructor
public class TodoAPIException extends RuntimeException{
    private HttpStatus httpStatus;
    private String message;
}
```

##### GlobalExceptionHandler
```java

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(TodoAPIException.class)
    public ResponseEntity<ErrorDetails> handleTodoAPIException(TodoAPIException todoAPIException, WebRequest webRequest){
        ErrorDetails errorDetails = new ErrorDetails(
                LocalDateTime.now(),
                todoAPIException.getMessage(),
                webRequest.getDescription(false)
        );
        return new ResponseEntity<>(errorDetails, todoAPIException.getHttpStatus());
    }
}

```

Usage:
```java
if(todoDto == null) {
    throw new TodoAPIException(HttpStatus.NOT_FOUND, "Todo not found");
}

Todo todo = todoRepository.findById(todoId)
            .orElseThrow(() -> new ResourceNotFound("Todo not found with id:"+todoId));
```