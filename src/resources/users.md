## Users

Basic Path: `/api/v1/users`

### Attributes

Attribute name | Type   | Example  | Read  | Write
-------------- | ------ | -------- | ----- | -------
username       | String | `pisos`  | all   | owner
password       | String | `qweqwe` | none  | owner
access_token   | String | `qweqwe` | owner | all
role           | String | `admin`  | all   | admin

### Relationships

TODO: Пока здесь ничего нет, но скоро появится

### [GET] /users

Returns of users list

Access level: all

#### Request

```bash
curl https://radiwa.host/api/v1/users \
  -X GET
  -H "Content-Type: application/json"
```

#### Response

```
Status: 200 OK
Content-Type: application/json

{
    "data": [
        {
            "id": 1,
            "attributes": {
                "username": "хуядмин",
                "role": "admin"
            }
        }
    ]
}
```

### [GET] /users/{:id}

Returns attributes for one specific user

#### Request

```
curl https://radiwa.host/api/v1/users/1 \
  -X GET
  -H Content-Type: application/json
```

#### Response

```
Status: 200 OK
Content-Type: application/json

{
    "data": {
        "id": 1,
        "attributes": {
            "username": "хуядмин",
            "role": "admin"
        }
    }
}
```

### [POST] /users

Creates new user

#### Request
```
curl https://radiwa.host/api/v1/users \
  -X POST
  -H Content-Type: application/json
  -d @- << EOF
    {
       "data":{
            "type":"users",
            "attributes":{
                "username":"SomeUser",
                "password":"MyStrongPassowrd",
            }
        }
    }
EOF
```
#### Response
```
Status: 201 OK
Content-Type: application/json

{
    "data": {
        "id": 2,
        "attributes": {
            "username": "SomeUser",
            "role": "user"
        }
    }
}
```

### [POST] /users/{:id}

Authorize users

#### Request
```
curl https://radiwa.host/api/v1/users \
  -X POST
  -H Content-Type: application/json
  -d @- << EOF
    {
       "data":{
            "type":"users",
            "attributes":{
                "username":"SomeUser",
                "password":"MyStrongPassowrd",
            }
        }
    }
EOF
```

#### Response
```
Status: 200 OK
Content-Type: application/json

{
    "data": {
        "id": 2,
        "attributes": {
            "username": "SomeUser123",
            "access-token": "AccessTokenForSomeUser"
        }
    }
}
```

### [PATCH] /users/{:id}

Updates user info (or changes password).

Access Restriction: Only for yourself.

**NOTE:** Changing password revoke all old tokens

#### Request
```
curl https://radiwa.host/api/v1/users/2 \
  -X POST
  -H Content-Type: application/json
  -H Authorization: Bearer AccessTokenForSomeUser
  -d @- << EOF
    {
       "data":{
            "type":"users",
            "id": 2,
            "attributes":{
                "username":"SomeUser123",
                "password":"MyStrongPassowrd",
            }
        }
    }
EOF
```

### Response
```
Status: 200 OK
Content-Type: application/json

{
    "data": {
        "id": 2,
        "attributes": {
            "username": "SomeUser123",
            "role": "user"
        }
    }
}
```

### [DELETE] /users/{:id}

Destroy user.

#### Request

```
curl https://radiwa.host/api/v1/users/2 \
  -X DELETE
  -H Content-Type: application/json
  -H Authorization: Bearer AccessTokenForSomeUser
EOF
```