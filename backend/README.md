# Backend - Pray5

This is the backend directory for the Pray5 project.

## Database Schema Design

### `mosques`

- id: int (PRIMARY KEY)
- name: string
- address: string
- coordinates_x: float
- coordinates_y: float
- phone_number: string (Acts as a username to login)
- password: string (Hashed)

### `prayers`

- mosque_id: int (FOREIGN KEY to `mosques.id`)
- Fajr: time with timezone
- Dhuhr: time with timezone
- Asr: time with timezone
- Maghrib: time with timezone
- Isha: time with timezone

### `events`

- To be implemented.

## API Specification

### `GET /search`

- Query Parameters:
  - `q`: The search query
- Response:
  - `200 OK`: The search results
    data:

    ```json
    [
        {
                "id": 1,
                "name": "Mosque 1",
                "address": "Address 1",
                "coordinates_x": 0.0,
                "coordinates_y": 0.0,
                "phone_number": "1234567890"
            },
            {
                "id": 2,
                "name": "Mosque 2",
                "address": "Address 2",
                "coordinates_x": 0.0,
                "coordinates_y": 0.0,
                "phone_number": "0987654321"
        }
    ]
    ```

  - `400 Bad Request`: The query is invalid
  - `500 Internal Server Error`: The server failed to process the request

### `GET /near_me`

- Request Body:
  - `coordinates_x`: The x-coordinate of the user
  - `coordinates_y`: The y-coordinate of the user
- Response:
  - `200 OK`: The search results
      data:
  
      ```json
      [
          {
                  "id": 1,
                  "distance": 0.0, // Note that the distance is in kilometers and this attribute will be used to order the mosques on the frontend
                  "name": "Mosque 1",
                  "address": "Address 1",
                  "coordinates_x": 0.0,
                  "coordinates_y": 0.0,
                  "phone_number": "1234567890",
                  "prayers": {
                      "Fajr": "05:00:00+08:00",
                      "Dhuhr": "12:00:00+08:00",
                      "Asr": "15:00:00+08:00",
                      "Maghrib": "18:00:00+08:00",
                      "Isha": "20:00:00+08:00"
                  }
              },
              {
                  "id": 2,
                  "distance": 0.1,
                  "name": "Mosque 2",
                  "address": "Address 2",
                  "coordinates_x": 0.0,
                  "coordinates_y": 0.0,
                  "phone_number": "0987654321",
                  "prayers": {
                      "Fajr": "05:00:00+08:00",
                      "Dhuhr": "12:00:00+08:00",
                      "Asr": "15:00:00+08:00",
                      "Maghrib": "18:00:00+08:00",
                      "Isha": "20:00:00+08:00"
                  }
          }
      ]
      ```
  
  - `400 Bad Request`: The request body is invalid
  - `500 Internal Server Error`: The server failed to process the request

### `POST /login`

- Request Body:
  - `phone_number`: The phone number of the user
  - `password`: The password of the user
- Response:
  - `200 OK`: The login is successful
    data:

    ```json
    {
        // Specific user data 
        "id": 1,
        "name": "Mosque 1",
        "address": "Address 1",
        "coordinates_x": 0.0,
        "coordinates_y": 0.0,
        "phone_number": "1234567890",
        // To display the prayers for updates
        "prayers": {
            "Fajr": "05:00:00+08:00",
            "Dhuhr": "12:00:00+08:00",
            "Asr": "15:00:00+08:00",
            "Maghrib": "18:00:00+08:00",
            "Isha": "20:00:00+08:00"
        }
    }
    ```

  - `400 Bad Request`: The request body is invalid
  - `401 Unauthorized`: The login is unsuccessful
  - `500 Internal Server Error`: The server failed to process the request

### `POST /register_mosque`

- Request Body:
  - `name`: The name of the mosque
  - `address`: The address of the mosque
  - `coordinates_x`: The x-coordinate of the mosque
  - `coordinates_y`: The y-coordinate of the mosque
  - `phone_number`: The phone number of the mosque
  - `password`: The password of the mosque
  - `prayers`: The prayer timings of the mosque
    data:

    ```json
    {
        "Fajr": "05:00:00+08:00",
        "Dhuhr": "12:00:00+08:00",
        "Asr": "15:00:00+08:00",
        "Maghrib": "18:00:00+08:00",
        "Isha": "20:00:00+08:00"
    }
    ```

- Response:
  - `200 OK`: The registration is successful
      data:
  
      ```json
      {
          // Specific user data 
          "id": 1,
          "name": "Mosque 1",
          "address": "Address 1",
          "coordinates_x": 0.0,
          "coordinates_y": 0.0,
          "phone_number": "1234567890",
          // To display the prayers for updates
          "prayers": {
              "Fajr": "05:00:00+08:00",
              "Dhuhr": "12:00:00+08:00",
              "Asr": "15:00:00+08:00",
              "Maghrib": "18:00:00+08:00",
              "Isha": "20:00:00+08:00"
          }
      }
      ```
  
  - `400 Bad Request`: The request body is invalid
  - `500 Internal Server Error`: The server failed to process the request

### `PATCH /update_prayers`

- Note: Must be logged in.
- Request Body:
  - `prayers`: The (updated) prayer timings of the mosque
    data:

    ```json
    {
        "Fajr": "05:00:00+08:00",
        "Dhuhr": "12:00:00+08:00",
        "Asr": "15:00:00+08:00",
        "Maghrib": "18:00:00+08:00",
        "Isha": "20:00:00+08:00"
    }
    ```

- Response:
  - `200 OK`: The update is successful
  - `400 Bad Request`: The request body is invalid
  - `401 Unauthorized`: The login is unsuccessful/User is not authorized
  - `500 Internal Server Error`: The server failed to process the request

### `PATCH /update_mosque`

- Note: Must be logged in.
- Request Body:
  - `name`: The name of the mosque
  - `address`: The address of the mosque
  - `coordinates_x`: The x-coordinate of the mosque
  - `coordinates_y`: The y-coordinate of the mosque
  - `phone_number`: The phone number of the mosque
  - `password`: The password of the mosque

- Response:
  - `200 OK`: The update is successful
  - `400 Bad Request`: The request body is invalid
  - `401 Unauthorized`: The login is unsuccessful/User is not authorized
  - `500 Internal Server Error`: The server failed to process the request

### `GET /events_near_me`

- To be implemented
