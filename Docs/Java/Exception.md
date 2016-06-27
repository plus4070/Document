## GlobalException.java

```java
public class GlobalException extends RuntimeException {

    /**
     * 
     */
    private static final long serialVersionUID = 1L;

    private ResponseType responseType;
    private String exceptionMessage;
    private Exception exception;

    public GlobalException(ResponseType responseType) {
        this.responseType = responseType;
        this.exceptionMessage = responseType.getMessage();
    }

    public GlobalException(ResponseType responseType, Exception exception) {
        this.responseType = responseType;
        this.exceptionMessage = responseType.getMessage();
        this.exception = exception;
    }

    public GlobalException(ResponseType responseType, String exceptionMessage) {
        this.responseType = responseType;
        this.exceptionMessage = exceptionMessage;
    }

    public ResponseType getResponseType() {
        return responseType;
    }

    public String getMessage() {
        return exceptionMessage;
    }

    public Exception getOriginalException() {
        return exception;
    }

}
```

## ResponseType.java

```java
public enum ResponseType {
    WORK_BOOK_CLOSE_EXCEPTION(-1111, "Workbook closing exception"),
    EXCEL_FILE_READ_EXCEPTION(-1112, "Excel file reading exception"),
    JSON_PARSING_EXCEPTION(-1113, "Json parsing exception");    
    
    public Integer code;
    public String message;
    
    ResponseType(Integer code, String message) {
        this.code = code;
        this.message = message;
    }
    
    public Integer getCode() {
        return code;
    }
    
    public String getMessage() {
        return message;
    }
}
```
