# AUTH SPEC

### POST /v1/auth/sign-up
- Headers:
    ```
    x-client-id: base-64(client-id:client-key)
    ```
- Request:
    ```json
    {
        "id": "string", // optional
        "email": "string", // must be filled in one of the email or phone number
        "phone_number": "string", // must be filled in one of the email or phone number
        "first_name": "string", // optional
        "last_name": "string", // optional
        "password": "string", // mandatory
        "confirm_password": "string", // mandatory
    }
    ```
- Reponse Success
    ```json
    {
        "message": "authentication successfully created"
    }
    ```
- Response Failed
    ```json
    {
        "error_message": "duplicate phone number"
    }
    ```
    messages that might appear:
    - unexpected client id
    - duplicate phone number
    - duplicate email
    - password not match

### POST /v1/auth/sign-in
- Headers:
    ```
    x-client-id: base-64(client-id:client-key)
    ```
- Request:
    ```json
    {
        "email": "string", // must be filled in one of the email or phone number
        "phone_number": "string", // must be filled in one of the email or phone number
        "password": "string", // mandatory
    }
    ```
- Reponse Success
    ```json
    {
        "access_token": "string",
        "refresh_token": "string", // default + 7 day
        "token_valid_until": "datetime", // default + 24 hour
    }
    ```
- Response Failed
    ```json
    {
        "error_message": "unexpected client id"
    }
    ```
    messages that might appear:
    - unexpected client id
    - user not found
    - wrong credential

### POST /v1/auth/sign-out
- Headers:
    ```
    x-client-id: base-64(client-id:client-key)
    Authorization: Bearer access_token
    ```
- Request:
   ```
   no request
   ```
- Reponse Success
    ```json
    {
        "message": "success"
    }
    ```
- Response Failed
    ```json
    {
        "error_message": "unexpected client id"
    }
    ```

### POST /v1/auth/refresh-token
- Headers:
    ```
    x-client-id: base-64(client-id:client-key)
    Authorization: Bearer refresh_token
    ```
- Request:
    ```
    no request
    ```
- Reponse Success
    ```json
    {
        "access_token": "string",
        "refresh_token": "string", // default + 7 day
        "token_valid_until": "datetime", // default + 24 hour
    }
    ```
- Response Failed
    ```json
    {
        "error_message": "unexpected refresh token"
    }
    ```
    messages that might appear:
    - unexpected client id
    - unexpected refresh token

### PUT /v1/auth/user/:id
- Headers:
    ```
    x-client-id: base-64(client-id:client-key)
    Authorization: Bearer access_token

    ```
- Request:
    ```json
    {
        "email": "string", 
        "phone_number": "string",
        "first_name": "string",
        "last_name": "string"
    }
    ```
- Reponse Success
    ```json
    {
        "access_token": "string",
        "refresh_token": "string", // default + 7 day
        "token_valid_until": "datetime", // default + 24 hour
    }
    ```
- Response Failed
    ```json
    {
        "error_message": "unexpected client id"
    }
    ```
    messages that might appear:
    - unexpected client id
    - user not found
    - wrong credential

### POST /v1/auth/user/current
- Headers:
    ```
    x-client-id: base-64(client-id:client-key)
    Authorization: Bearer access_token
    ```
- Request:
    ```
    no request
    ```
- Reponse Success
    ```json
    {
        "id": "string",
        "first_name": "string",
        "last_name": "string",
        "email": "string",
        "phone_number": "string",
        "status": "string"
    }
    ```
- Response Failed
    ```json
    {
        "error_message": "unexpected access token"
    }
    ```
    messages that might appear:
    - unexpected client id
    - unexpected access token

### PATCH /v1/auth/user/change-password
- Headers:
    ```
    x-client-id: base-64(client-id:client-key)
    Authorization: Bearer access_token

    ```
- Request:
    ```json
    {
        "old_password": "string", // mandatory
        "new_password": "string", // mandatory
        "confirm_password": "string" // mandatory
    }
    ```
- Reponse Success
    ```json
    {
        "message": "success"
    }
    ```
- Response Failed
    ```json
    {
        "error_message": "unexpected client id"
    }
    ```
    messages that might appear:
    - new password must be different from old password
    - unexpected client id
    - wrong credential