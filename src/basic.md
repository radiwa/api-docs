# Radiwa API

## Before

Default encoding - UTF-8.

DateTime written in [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) in UTC.

API based on [JSON:API](https://jsonapi.org/), but has some specific differences: for `Content-Type` can be used `application/json`.

## Read and Write Access

Access type by roles

Access type | Description
-------------------------
all         | can access all users and users without authorization
users       | can access only authorized users
rj          | can access users with role `RJ` and `admin`
admin       | can access users with role `admin` 

### Example of `Include`

Include returns entity with relations

```bash
curl https://radiwa.host/api/v1/radio-shows?include=rj \
  -X GET
  -H Content-Type: application/json
```

```
Status: 200 OK
Content-Type: application/json

{
    "data": [
        {
            "id": 1,
            "type": "radio-shows",
            "attributes": {
                "name": "Вечер с админом",
                "planned-at": "2019-06-18T08:37:39+00:00",
                "repeatable": false
            },
            "relationships": {
                "rj": [
                    {
                        "id": 1,
                        "type": "users"
                    }
                ]
            }
        },
    ],
    "includes": [
        {
            "id": 1,
            "type": "users",
            "attributes": {
                "username": "хуядмин",
                "role": "admin"
            }
        }
    ]
}
```