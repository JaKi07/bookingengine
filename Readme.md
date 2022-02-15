# Completed Django test project and comments are specified under the individual task section provided below.

## Django & Django REST framework test project.
The test project won't be used in any real environment.


## Summary:

Extend Django Project with Django REST Framework for a simple prebuild booking app that have:
- Listing object with two types - **booking_engine.models** :
    - Apartment(single_unit) have booking information (price) directly connected to it.
    - Hotels(multi-unit) have booking information (price) connected to each of their HotelRoomTypes.
- filtering through Listings and returning JSON response with available units based on search criterias.
- should handle large dataset of Listings.

1. There is a pre-build structure for Hotels/Apartments (could be changed or extended). Database is prefilled with information - **db.sqlite3**.
    - superuser
        - username: **admin**
        - password: **admin**

2. We should be able to **block days** ( make reservations ) for each **Apartment** or **HotelRoom**.
    - **new** Model for blocked (reserved) days must be created

    **A new model for blocking days (reservation) is created. The BlockedDays model is provided in the `models.py` file. Includes relevant fields that are necessary for blocking days. A single `day` field is considered for blocking for simplicity.**

3. NEW **endpoint** where we will get available Apartments and Hotels based on:
	- **available days** (date range ex.: "from 2021-12-09 to 2021-12-12")
            - Apartment should not have any blocked day inside the range
            - Hotel should have at least 1 Hotel Room available from any of the HotelRoomTypes
     - **max_price** (100):
		- Apartment price must be lower than max_price.
		- Hotel should have at least 1 Hotel Room without any blocked days in the range with price lower than max_price.

    **A new endpoint is created to fetch all available apartments and hotels that satisfy all the conditions that are specified above.**

	- returned objects should be **sorted** from lowest to highest price.
		-  hotels should display the price of the **cheapest HotelRoomType** with **available HotelRoom**.

    **A output is sorted is sorting from lowest to highest price and displays the cheapest HotelRoomType with available HotelRoom in case of hotels.**

    **A JSON output response is displayed in the browser as per the response example format provided.**

## Techincal Implementation Details

**1. A new model for blocking days is created in the `models.py` file**

**2. A template (simple HTML) to receive inputs from the user - `start date`, `end date` and `maximum price` is created.**

**3. The paths/routes are updated in the `urls.py` file and linked to the Django methods in `views.py` file**
**4. `views.py` file holds the logic for displaying data and computing available accomodations according to the user inputs and the specification provided in the README file.**
    
    - It consists of two methods: `display_home` to take the input from the user, `display_data` to calculate and output the JSON response according to the inputs from the user and the specifications provided.

**5. Database file is updated and necessary migrations are also performed to test the specification.**

**6. Necessary updations are done in `settings.py` file (it also includes a debug toolbar :)**

## Request example:

http://localhost:8000/api/v1/units/?max_price=100&check_in=2021-12-09&check_out=2021-12-12


## Response example:

    {
        "items": [
            {
                "listing_type": "Apartment",
                "title": "Luxurious Studio",
                "country": "UK",
                "city": "London",
                "price": "40"

            },
            {
                "listing_type": "Hotel",
                "title": "Hotel Lux 3***",
                "country": "BG",
                "city": "Sofia",
                "price": "60" # This the price of the first Hotel Room Type with a Room without blocked days in the range

            },
            {
                "listing_type": "Apartment",
                "title": "Excellent 2 Bed Apartment Near Tower Bridge",
                "country": "UK",
                "city": "London",
                "price": "90"
            },
        ]
    }
