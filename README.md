# InMemori Http REST API  

### Server

| Env        | Url                              |
|------------|----------------------------------|
| production | `https://api.inmemori.com`       |
| dev        | `https://api.inmemori-dev.com`   |

<br/>
This Api accepts only `json`.

### Identification

Add your `jwt` in the `body` or the `query` for each query. 

example: `/endpoint?jwt={yourjwt}`
  
  
<br/>

## Create a page

### `POST /users`


| Fields          | required| Type           | Info                | ex:                            |
|-----------------|---------|----------------|---------------------|--------------------------------|
| firstname       |    *    | `string`       |firstname of the deceased                     |                                |
| lastname        |    *    | `string`       |lastname of the deceased                     |                                |
| email           |    *    | `string`       |email of the claimant                     | sophiedupont1289@mail.com      |
| phone             |         | `string`      | phone number      |       |
| db             |         | `string`      | database's location       | `us`or `eu`       |
| zone             |         | `string`      | zone 's location       | `fr`, `us`, `mx`,`de`, `es`, `be`, `ch`       |
| dod             |         | `isodate`      | date of death       | 2018-12-19T00:00:00.000Z       |
| dob             |         | `isodate`      | date of birth       | 1946-04-11T00:00:00.000Z       |
| gender          |         | `string`       | `m` or `f`          |                                |
| intro          |         | `string`       | announcement          | free text                               |
| places          |         | `array(place)` |                     | see **place** schema           |
| contacts        |         | `array(contact)`|                    | see **contact** schema         |
| meta            |         | `object`       |                     | see **meta** schema            |



#### Place Schema : information on the ceremonies


| Fields          | Type           | Info                | ex:                            |
|-----------------|----------------|---------------------|--------------------------------|
| date            | `isodate`      | date of ceremony at Zulu time (UTC-0)| ex: for an event the 03 dec at 10:30AM in Paris, the correct date would be `2020-12-03T09:30:00.000Z`       |
| tz            | `string`       | time zone| a valid [timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) `Europe/Paris`, `America/Mexico_City`...  |
| name            | `string`       | location of ceremony| Trinity Cemetery ; Cimetière de Montparnasse      |
| address         | `string`       | adress of ceremony  | West 155th Street, New York, NY, USA ; 3 rue de Rivoli, 75014 Paris 
| type            | `string`       | `ceremony`, `contemplation`, `interment` or `cremation`|     |
| privacy         | `boolean`      | default `false`     |     |



#### Contact Schema : information on the organizer of the memorial services


| Fields          | Type           | Info                | ex:                            |
|-----------------|----------------|---------------------|--------------------------------|
| name            | `string `      |first and last names of the claimant| Sophie Dupont                  |
| phone           | `string `      |number of the claimant                     |818 257 1190 ; 06 01 02 03 04                     |
| email           | `string `      |contact email        |familledupont@mail.com    |
| relationship    | `string `      |relationship to the deceased      |Child    |
| address         | `string `      | contact perosnal address                     |20 rue du Louvre, 75001 Paris         |



#### Meta Schema : information on the page creator


| Fields          | Type           | Info                | ex:                            |
|-----------------|----------------|---------------------|--------------------------------|
| author          | `string `      | counselor's name    | Marc Leblanc                   |
| agency          | `string `      | agency's name       | Smith Funeral services ; Pompes Funèbres République|
| agency_code     | `string `      | identification number if any       | AGC01 |
| note            | `string `      | additional information       | comments if any|



### Example

  ```curl
    curl -X POST 'https://api.inmemori-dev.com/users?jwt=xxx' \
      -H 'content-type: application/json' \
      -d '{ 
              "firstname": "paul"
            , "lastname": "cezane"
            , "email": "sophiedupont1289@mail.com"
            , "dod": "2018-12-19T00:00:00.000Z"
            , "dob": "1964-04-12T00:00:00.000Z"
            , "places": [
                { 
                    "name": "Cimetière de Montparnasse"
                  , "address": "3 rue de Rivoli, 75014 Paris"
                  , "type": "ceremony"
                  , "date": "2018-12-19T11:45:00.000Z"
                }
              ]
            , "contacts": [
                { 
                    "name": "Sophie Dupont"
                  , "phone": "0601020304"
                  , "email": "familledupont@mail.com"
                }
              ] 
            , "meta": {
                  "author": "Marc Leblanc"
                , "agency": "Pompes Funèbres République"
              } 
          }'
  ```
