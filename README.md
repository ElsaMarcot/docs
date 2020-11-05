# InMemori API  

### Server

| Env        | Url                              |
|------------|----------------------------------|
| production | `https://api.inmemori.com`       |
| dev        | `https://api.inmemori-dev.com`   |

<br/>  

This API consist of a single endpoint `/pages` which only accepts `json` as content type.  

### Identification

On the `dev` api, you can provide your `jwt` token in the `body` or `query` of the requests, but on the `production` api we we recommend passing the `jwt` in the request `authorization` header (see exemple below).

### Create a Page

To create a page, just make a POST request on the `/pages` api endpoint.  

  ```curl
    curl -X POST 'https://api.inmemori-dev.com/pages' \
      -H 'authorization: JWT xxx'
      -H 'content-type: application/json' \
      -d '{ 
              "defunct": {
                  "firstname": "paul"
                , "lastname": "cezane"
                , "dod": "2018-12-19T00:00:00.000Z"
                , "dob": "1964-04-12T00:00:00.000Z"
                , "gender": "m"
              }
             , "places": [
                {
                    "type": "ceremony"
                  , "address": "3 rue de Rivoli, 75014 Paris"
                  , "name": "Cimetière de Montparnasse"
                  , "date": "2018-12-19T11:45:00.000Z"
                },
                {
                    "type": "internment"
                  , "address": "14 avenue des Rois, 75011 Paris"
                  , "name": "Chapelle Sixteen"
                  , "date": "2018-12-20T19:30:00.000Z"
                }
              ]
            , "contacts": [
                { 
                    "name": "alice cezane"
                  , "phone": "+33600000001"
                  , "email": "alice@mail.com"
                  , "relationship": "child"
                },
                { 
                    "name": "bob cezane"
                  , "phone": "+33600000002"
                  , "email": "bob@mail.com"
                }
              ]
            , "agency": {
                  "name": "PF d'aix en provence"
                , "code": "A13100"
                , "email": "pf.aix@mail.com"
              } 
            , "counselor": {
                  "firstname": "Bruce"
                , "lastname": "Wayne"
                , "code": "C420"
                , "email": "bruce.wayne@mail.com"
              } 
            , "segments": [
                {
                    "name": "PACA"
                  , "code": "CD345"
                },
                {
                    "name": "Bouches du rhone"
                  , "code": "ZS456"
                }
              ]
          }'
  ```
  
On success, the response will return a `200` with the json `page` object.  

```json
{ 
    "_ID": "cezane1234"
  , "firstname": "paul"
  , "lastname": "cezane"
  , "dod": "2018-12-19T00:00:00.000Z"
  , "dob": "1964-04-12T00:00:00.000Z"
  , ...
} 
```

You might want to save the `_ID` attribute in your database. 


## Data Schemas

#### Page (Main schema)

| Fields          | Type               | Info                              | ex:                            |
|-----------------|--------------------|-----------------------------------|--------------------------------|
| defunct         | `object(defunct)`  | defunct infos                     | see **defunct** schema |
| contacts        | `array(contact)`   | managers of the page              | see **contact** schema |
| places          | `array(place)`     | ceremonies infos                  | see **place** schema |
| agency          | `object(agency)`   | agency infos                      | see **agency** schema |
| counselor       | `object(counselor)`| counselor infos                   | see **counselor** schema |
| segments        | `object(segment)`  | CRM segmentations                 | see **segment** schema |


#### Defunct

| Fields          | Type               | Info                              | ex:                            |
|-----------------|--------------------|-----------------------------------|--------------------------------|
| firstname       | `string`           | firstname of the deceased         | |
| lastname        | `string`           | lastname of the deceased          | |
| dod             | `isodate`          | date of death                     | 2018-12-19T00:00:00.000Z |
| dob             | `isodate`          | date of birth                     | 1946-04-11T00:00:00.000Z |
| gender          | `string`           | `m` or `f`                        | |


#### Place (Array of place)

| Fields          | Type           | Info                | ex:                            |
|-----------------|----------------|---------------------|--------------------------------|
| date            | `isodate`      | date of ceremony at Zulu time (UTC-0)| ex: for an event the 03 dec at 10:30AM in Paris, the correct date would be `2020-12-03T09:30:00.000Z`       |
| tz            | `string`       | time zone| a valid [timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) `Europe/Paris`, `America/Mexico_City`...  |
| name            | `string`       | location of ceremony| Trinity Cemetery ; Cimetière de Montparnasse      |
| address         | `string`       | adress of ceremony  | West 155th Street, New York, NY, USA ; 3 rue de Rivoli, 75014 Paris 
| type            | `string`       | `ceremony`, `contemplation`, `interment`, `reception`, `gathering` or `cremation`|     |


#### Contact (Array of contact)

| Fields          | Type           | Info                              | ex:                            |
|-----------------|----------------|-----------------------------------|--------------------------------|
| name            | `string `      | Fullname of the claimant          | Bob Stuart |
| phone           | `string `      | mobile number                     | +33600000000 |
| landline        | `string `      | landline number (tel fixe)        | +33100000000 |
| email           | `string `      | contact email                     | bob@mail.com |
| relationship    | `string `      | relationship to the deceased      | child/parent/friend |
| address         | `string `      | contact personal address          | 20 rue du Louvre, 75001 Paris |

#### Agency

| Fields          | Type           | Info                           | ex:                            |
|-----------------|----------------|--------------------------------|--------------------------------|
| name            | `string `      | agency name                    | PF d'aix en provence |
| code            | `string `      | agency ID                      | A13100 |
| email           | `string `      | agency email                   | pf.aix@mail.com |
| phone           | `string `      | agency phone                   | +33608998877 |
| landline        | `string `      | landline number (tel fixe)     | +33100000000 |
| note            | `string `      | additional information         | any comment |

#### Counselor

| Fields          | Type        | Info                              | ex:                            |
|-----------------|-------------|-----------------------------------|--------------------------------|
| firstname       | `string `   | counselor firstname               | Bruce |
| lastname        | `string `   | counselor lastname                | Wayne |
| code            | `string `   | counselor ID                      | C420 |
| email           | `string `   | counselor email                   | bruce.wayne@mail.com |
| phone           | `string `   | counselor phone                   | +33608998877 |
| note            | `string `   | additional information            | any comment |

#### Segment

| Fields          | Type        | Info                              | ex:                            |
|-----------------|-------------|-----------------------------------|--------------------------------|
| name            | `string `   | segment name                      | Région PACA |
| code            | `string `   | segment ID                        | Z345 |

