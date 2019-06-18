## Radio Shows

Basic Path: `/api/v1/radio-shows`

### Attributes

Attribute name  | Type     | Example  | Read  | Write           |
--------------- | --------- | ------- | ------ | ----------------- | -----------
repeatable      | Boolean  | true     | all   | owner and admin | Show repeats every week
planned-at      | DateTime | 2019-06-18T08:37:39+00:00 | all | owner and admin | **Only if not repeatable** Strict date to show
planned-at-days | Array\<String> | ['Monday', 'Sunday'] | all | owner and admin | **Only if repeatable** Days, when show appears

### Relationships

Relation name | Relation Type | JSON:API type | Read
------------- | ------------- | ------------- | ------
author        | One           | users         | All
RJ            | Many          | users         | All

### [GET] /radio-shows

#### Request

```bash
curl https://radiwa.host/api/v1/radio-shows \
  -X GET
  -H Content-Type: application/json
```

#### Response

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
            }
        },
        {
            "id": 2,
            "type": "radio-shows",
            "attributes": {
                "name": "Podpivasik",
                "planned-at-days": [
                    'Monday', 'Saturday', 'Friday'
                ],
                "repeatable": true
            }
        }
    ]
}
```

#### filters

##### by-week

Returns shows appeared in week with specific date. Work with a numbers of week and with dates.

with numbers of week:

```
curl -G "https://radiwa.host/api/v1/radio-shows" \
  -X GET \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer api_token" \
  --data-urlencode "filter[by-week]=28"
```

with dates:

```
curl -G "https://radiwa.host/api/v1/radio-shows" \
  -X GET \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer api_token" \
  --data-urlencode "filter[by-week]=2019-06-18T08:37:39+00:00"
```

##### from-planned-at

Returns shows appeared from specific date

```
curl -G "https://radiwa.host/api/v1/radio-shows" \
  -X GET \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer api_token" \
  --data-urlencode "filter[from-planned-at]=2019-06-18T08:37:39+00:00"
```

##### to-planned-at

Returns shows appeared to specific date

```
curl -G "https://radiwa.host/api/v1/radio-shows" \
  -X GET \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer api_token" \
  --data-urlencode "filter[to-planned-at]=2019-06-18T08:37:39+00:00"
```

### [GET] /radio-shows/1

#### Request

```bash
curl https://radiwa.host/api/v1/radio-shows/1 \
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
        "type": "radio-shows",
        "attributes": {
            "name": "Вечер с админом",
            "planned-at": "2019-06-18T08:37:39+00:00",
            "repeatable": false
        }
    },
}
```

### [POST] /radio-shows

#### Request

Creates new shows

Access Restriction: Only for admin and RJ's

```bash
curl https://radiwa.host/api/v1/radio-shows \
  -X POST
  -H Content-Type: application/json
  -H Authorization: Bearer AccessTokenForSomeUser
  -d @- << EOF
    {
       "data":{
            "type":"radio-shows",
            "attributes":{
                "name":"New Radio Show",
                "planned-at-days": [
                  'Monday', 'Sunday'
                ],
                "repeatable": false
            }
        }
    }
```

#### Response

```
Status: 201 OK
Content-Type: application/json

{
    "data": {
        "id": 3,
        "type": "radio-shows",
        "attributes": {
            "name":"New Radio Show",
            "planned-at-days": [
              'Monday', 'Sunday'
            ],
            "repeatable": false
        }
    }
}
```

### [PATCH] /radio-shows/{:id}

#### Request

Creates new shows

Access Restriction: Only for admin and author.

```bash
curl https://radiwa.host/api/v1/radio-shows/1 \
  -X PATCH
  -H Content-Type: application/json
  -H Authorization: Bearer AccessTokenForSomeUser
  -d @- << EOF
    {
       "data":{
            "type":"radio-shows",
            "attributes":{
                "name":"New Radio Show",
                "planned-at-days": [
                  'Monday', 'Friday'
                ],
                "repeatable": false
            }
        }
    }
```

#### Response

```
Status: 201 OK
Content-Type: application/json

{
    "data": {
        "id": 3,
        "type": "radio-shows",
        "attributes": {
            "name":"New Radio Show",
            "planned-at-days": [
              'Monday', 'Friday'
            ],
            "repeatable": false
        }
    }
}
```

### [DELETE] /radio-shows/{:id}

Destroy radio show.

#### Request

```
curl https://radiwa.host/api/v1/users/2 \
  -X DELETE
  -H Content-Type: application/json
  -H Authorization: Bearer AccessTokenForSomeUser
EOF
```

#### Response

```
Status: 200 OK
Content-Type: application/json
```