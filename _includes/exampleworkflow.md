# Example Workflow

> Example Workflow

This section will provide a very basic example workflow for booking a hotel room for one adult and one child that encompasses creating a basket, searching for availability, adding items and customer information, commiting then checking the reservation and, finally,  cancelling it. For each step, see to the right of the page for ptototype calls using cURL or simple Javascript calls as well as example responses. Each of these calls (not counting the API keys and specific parameters that you will need to plug in) are the minimum requirements for a successful response.

Choose a Point of Sale
Choose a currency
Search availability
Create basket
Add booking item
Add customer info
Add Guest info
Commit Basket
Get Status
Review reservation
Cancel Reservation

## Get Point of Sale

```shell
curl -X GET 
--header 'Accept: application/json' 
--header 'apiKey: APIKEY132456789EWOK' 
'https://galaxy.test.citybreak.com/api/pointofsales'

```

```javascript
var r = fetch("https://galaxy.test.citybreak.com/api/pointofsales",
{
  headers: {
    "ApiKey:" "APIKEY132456789EWOK",
    "Accept": "application/json",
	 "Accept-Language": "en-US"
  }  
});
```

> Response:

```json
[
  {
    "Id": 1234561,
    "Name": "Online"
  },
  {
    "Id": 1234562,
    "Name": "online viktors hotell 3"
  },
  {
    "Id": 1234565,
    "Name": "callcenter test"
  },
  {
    "Id": 1234569,
    "Name": "Veras Call Center"
  },
  {
    "Id": 1234570,
    "Name": "Test salespoint Distribution API"
  }
]
```
The first step is to obtain a valid <a href="https://visit.github.io/galaxy-docs/#point-of-sales">Point of Sale</a> Identifier. This is needed for many of the following operations and defines the products available for search as well as many of the settings important for creating a booking, such as rate codes availability periods, etc. We will go with the "Test salespoint Distribution API" Point of Sale - **1234570**

### HTTP Request

`GET https://galaxy.citybreak.com/api/pointofsales`



## Choose A Currency 

```shell
curl -X GET 
--header 'Accept: application/json' 
--header 'apiKey: APIKEY132456789EWOK' 
'https://galaxy.test.citybreak.com/api/pointofsales/currencies/1234570'
```

```javascript
var r = fetch("https://galaxy.test.citybreak.com/api/pointofsales/currencies/1234570",
{
  headers: {
    "ApiKey:" "APIKEY132456789EWOK",
    "Accept": "application/json",
   "Accept-Language": "en-US"
  }  
});
```

> Example of response:

```json
[
  "DKK",
  "SEK",
  "NOK",
  "EUR"
]
```

Once we have a Point of Sale we need to choose a <a href="https://visit.github.io/galaxy-docs/#currencies">Currency</a>. Rates for hotel room products in the <a href="https://visit.github.io/galaxy-docs/#currencies">Availability Search</a> are shown in whatever currency you choose but certain hotels do not have products for sale in all currencies so your results may be filtered depending on the currency you choose. We will choose Swedish Kronor - **SEK**.

### HTTP Request

`GET https://galaxy.citybreak.com/api/pointofsales/currencies/1234570`





## Create A Basket

```shell
curl -X POST 
--header 'Accept: application/json' 
--header 'apiKey: APIKEY132456789EWOK' 
'https://galaxy.test.citybreak.com/api/basket/create/1234570/SEK'
```

```javascript
var r = fetch("https://galaxy.test.citybreak.com/api/basket/create/1234570/SEK",
{
  method:"POST"
  headers: {
    "ApiKey:" "APIKEY132456789EWOK",
    "Accept": "application/json",
  }  
});
```

> Example of response:

```json
{
  "BasketId": 87654321,
  "Success": true
}
```

For all booking operations you will need a <a href="https://visit.github.io/galaxy-docs/#create-basket">Shopping Basket</a>. This entity will be the reference for all information related to a booking, right up until the time the customer decides to commit to the booking and has provided all the necessary information. It is therefore very important to hold a reference to the BasketId, in this case - **87654321**

### HTTP Request

`GET https://galaxy.citybreak.com/api/content/attribute/1234570/SEK`





## Search Availability

```shell
curl -X POST 
--header 'Content-Type: application/json' 
--header 'Accept: application/json' 
--header 'Accept-Language: en-us' 
--header 'apiKey: APIKEY132456789EWOK' -d '{
   "PointOfSalesId": 1234570,
   "Arrival": "2017-10-14T12:27:58.851Z",
   "Departure": "2017-10-15T12:27:58.851Z",
   "Currency": "SEK",
   "PageSize": 20,
   "Page": 0,
   "PersonConfigurations": [
     {
       "Adults": 1,
       "ChildrenAges": [
         1
       ] 
     }
   ]
 }' 'https://galaxy.test.citybreak.com/api/availability/accommodation'
```

```javascript
var r = fetch("https://galaxy.test.citybreak.com/api/availability/accommodation",
{
	method: "POST",
	headers: {
	    "ApiKey:" "APIKEY132456789EWOK",
	    "Accept": "application/json",
		 "Accept-Language": "en-US"
	},
	body: JSON.Stringify({
	   "PointOfSalesId": 1234570,
	   "Arrival": "2017-10-14T12:27:58.851Z",
	   "Departure": "2017-10-15T12:27:58.851Z",
	   "Currency": "SEK",
	   "PageSize": 20,
	   "Page": 0,
	   "PersonConfigurations": [
	     {
	       "Adults": 1,
	       "ChildrenAges": [
	         1
	       ] 
	     }
	   ]
	})  
});
```

> Example of response:

```json
{
  "AccommodationSearch": {
    "PointOfSalesId": 1234570,
    "Arrival": "2017-10-14T00:00:00Z",
    "Departure": "2017-10-15T00:00:00Z",
    "Currency": "SEK",
    "PageSize": 20,
    "Page": 0,
    "Sort": {
      "Order": 0,
      "Field": "Price"
    },
    "PersonConfigurations": [
      {
        "Adults": 1,
        "ChildrenAges": [
          1
        ]
      }
    ],
    "ContentFilter": null,
    "OutputFilter": null
  },
  "Accommodations": [
    {
      "Id": 1136433,
      "Name": "Edelbrock Hotell 3",
      "Content": {
        "PriceFrom": 0,
        "Images": [
          {
            "Uri": "//images.citybreak.com/image.aspx?ImageId=4014876",
            "IsMain": true,
            "Name": null,
            "Copyright": null,
            "Description": null
          },
          {
            "Uri": "//images.citybreak.com/image.aspx?ImageId=4014877",
            "IsMain": false,
            "Name": null,
            "Copyright": null,
            "Description": null
          }
        ],
        "Information": [
          {
            "Id": 99,
            "Name": "Name",
            "Value": "Edelbrock Hotell 3"
          },
          {
            "Id": 101,
            "Name": "Introduction",
            "Value": "Edelbrock Hotell"
          },
          {
            "Id": 102,
            "Name": "Description",
            "Value": "Edelbrock Hotell!"
          },
          {
            "Id": 100038,
            "Name": "Elevator",
            "Value": "False"
          },
          {
            "Id": 100163,
            "Name": "Reception",
            "Value": "False"
          }
        ],
        "Categories": null,
        "Geos": [],
        "Pois": [],
        "Position": null
      },
      "Placements": [
        {
          "Price": {
            "Price": 450,
            "TotalPersons": 2,
            "Currency": "SEK",
            "DateStart": "2017-10-14T00:00:00Z",
            "DateEnd": "2017-10-15T00:00:00Z"
          },
          "Placements": [
            {
              "Name": "Dubbelrum med xbädd ÖSD",
              "Content": {
                "PriceFrom": null,
                "Images": [],
                "Information": [
                  {
                    "Id": 99,
                    "Name": "Name",
                    "Value": "Dubbelrum med xbädd ÖSD"
                  }
                ],
                "Categories": null,
                "Geos": null,
                "Pois": null,
                "Position": null
              },
              "IncludedSubProducts": [],
              "MaxPopopulation": 3,
              "MinPopulation": 1,
              "ExtraBeds": 1,
              "PersonConfiguration": {
                "Adults": 1,
                "ChildrenAges": [
                  1
                ]
              },
              "PricePeriods": [
                {
                  "DateStart": "2017-10-14T00:00:00",
                  "DateEnd": "2017-10-15T00:00:00",
                  "AdultPrice": 300,
                  "ChildPrice": 150,
                  "TotalPrice": 450,
                  "Currency": "SEK"
                }
              ],
              "BookingConditions": {
                "Name": null,
                "Terms": null
              }
            }
          ],
          "BookingKey": "18-A"
        },
        {
          "Price": {
            "Price": 500,
            "TotalPersons": 2,
            "Currency": "SEK",
            "DateStart": "2017-10-14T00:00:00Z",
            "DateEnd": "2017-10-15T00:00:00Z"
          },
          "Placements": [
            {
              "Name": "Dubbelrum ÖSD",
              "Content": {
                "PriceFrom": null,
                "Images": [],
                "Information": [
                  {
                    "Id": 99,
                    "Name": "Name",
                    "Value": "Dubbelrum ÖSD"
                  }
                ],
                "Categories": null,
                "Geos": null,
                "Pois": null,
                "Position": null
              },
              "IncludedSubProducts": [
                {
                  "Name": "Trädgårdstomte",
                  "Content": {
                    "PriceFrom": null,
                    "Images": [],
                    "Information": [
                      {
                        "Id": 99,
                        "Name": "Name",
                        "Value": "Trädgårdstomte"
                      }
                    ],
                    "Categories": null,
                    "Geos": null,
                    "Pois": null,
                    "Position": null
                  },
                  "Price": 150,
                  "Amount": 1,
                  "Currency": "SEK",
                  "PriceIncluded": false,
                  "IsExtraBed": false,
                  "PayOnSite": false
                }
              ],
              "MaxPopopulation": 3,
              "MinPopulation": 1,
              "ExtraBeds": 1,
              "PersonConfiguration": {
                "Adults": 1,
                "ChildrenAges": [
                  1
                ]
              },
              "PricePeriods": [
                {
                  "DateStart": "2017-10-14T00:00:00",
                  "DateEnd": "2017-10-15T00:00:00",
                  "AdultPrice": 250,
                  "ChildPrice": 100,
                  "TotalPrice": 350,
                  "Currency": "SEK"
                }
              ],
              "BookingConditions": {
                "Name": null,
                "Terms": null
              }
            }
          ],
          "BookingKey": "19-A"
        }
      ],
      "TotalResults": 2
    }
  ],
  "SearchId": "899fe054-3bb4-4ff8-b577-ba716b0b3317",
  "ExpirationDate": "2017-09-15T09:15:46.0619347Z",
  "TotalResults": 1
}
```

At a minimum, an <a href="https://visit.github.io/galaxy-docs/#accommodation-15">Availability Search</a> requires the **PointOfSalesId** we obtained earlier (1234570) and the **Currency** ("SEK") as well as the **Arrival** and **Departure** dates we want to search for, the **PersonConfigurations** (i.e. the number of guests both Adult and Child), and the "PageSize" of the result you want. If there are 50 results and you want to have 20 products at a time, you can check the total number products in the first response at **Page** 0 and repeat the request incrementing the **Page** as necessary. 

The information above is one of the most complex and probably the most important parts of the Galaxy API. It returns a list of "Accommodations", which in this case for Citybreak are Hotel Properties (or Bed & Breakfasts, Apartments, etc), within which is a list of "Placements" which are Hotel Rooms (or Beds or Apartments, etc. depending on the property. These may also come with compulsory Add-ons (also known as sub-products) like breakfast included or champagne on arrival. 

Each Accomodation object will also have content information about the property, that can also be found in an <a href="https://visit.github.io/galaxy-docs/#accommodation">Accommodation Search</a>, and how many room types (Placements) are available. 

The placement objects represent the products that will actually be booked. As such they have a lot of the important information a customer might want to see, they contain a "Price" that represents total spend as well as a "PricePeriods" list that breaks the cost dow for example. Other useful information might be included in the content info or the room configuration/occupancy allowances, etc. The most important information for the booking process however is the **BookingKey**. There is one per "Placement" and is used to <a href="https://visit.github.io/galaxy-docs/#add-booking-item">add a product to a Basket</a>.

Also required is the **SearchId** that represents your search. The object represented by this unique id will hold references to all the products that were returned in the Accomodation Search. Also keep note of the **ExpirationDate** of the search as the reference will be invalid after this timestamp is passed.

For this example we will use the double room at Edelbrock Hotell 3 with a handy Garden Gnome included:

"BookingKey" = 19-A
"SearchId" = 899fe054-3bb4-4ff8-b577-ba716b0b3317

### HTTP Request

`POST https://galaxy.citybreak.com/availability/accommodation`




## Add A Booking Item

```shell
curl -X PUT 
--header 'Accept: application/json' 
--header 'apiKey: APIKEY132456789EWOK' 
'https://galaxy.test.citybreak.com/api/basket/add/accommodation/87654321/899fe054-3bb4-4ff8-b577-ba716b0b3317/19-A'
```

```javascript
var r = fetch("https://galaxy.test.citybreak.com/api/basket/add/accommodation/87654321/899fe054-3bb4-4ff8-b577-ba716b0b3317/19-A",
{
  method:"PUT"
  headers: {
    "ApiKey:" "APIKEY132456789EWOK",
    "Accept": "application/json",
  }  
});
```

> Example of response:

```json
true
```

Taking the **BasketId**: 87654321 of the basket we created earlier, the **SearchId**: 899fe054-3bb4-4ff8-b577-ba716b0b3317 from the availability search and the **BookingCode**: 19-A of the product we selected, we can now <a href="https://visit.github.io/galaxy-docs/#add-booking-item">add a product to our Basket</a>.

`PUT https://galaxy.test.citybreak.com/api/basket/add/accommodation/87654321/899fe054-3bb4-4ff8-b577-ba716b0b3317/19-A"`




## Add Customer Info


```shell
curl -X POST 
--header 'Content-Type: application/json' 
--header 'apiKey: APIKEY132456789EWOK' -d '{
   "NameFirst": "Test",
   "NameLast": "User",
   "Salutation": "Ms",
   "CustomerType": "Private",
   "Culture": "en-US",
   "Address": { 
     "StreetAddress1": "Test Road 123",
     "PostalCode": "41111",
     "City": "Gothenburg",
     "CountryCode": "SE"
   },
   "Email": "testuser%40visit.com",
   "PhoneMobile": { 
     "CountryCode": "46",
     "AreaCode": "07",
     "Number": "2222222"
   }
 }' 'https://galaxy.test.citybreak.com/api/basket/customer/87654321'
```

```javascript
var r = fetch("https://galaxy.test.citybreak.com/api/basket/customer/87654321",
{
  method:"POST"
  headers: {
    "ApiKey:" "APIKEY132456789EWOK",
    "Accept": "application/json",
  }  
  body: JSON.Stringify({
    "NameFirst": "Test",
    "NameLast": "User",
    "Salutation": "Ms",
    "CustomerType": "Private",
    "Culture": "en-US",
    "Address": {
      "StreetAddress1": "Test Road 123",
      "PostalCode": "41111",
      "City": "Gothenburg",
      "CountryCode": "SE"
    },
    "Email": "testuser@visit.com",
    "PhoneMobile": {
      "CountryCode": "46",
      "AreaCode": "07",
      "Number": "2222222"
    }
  }
}
});
```

> Example of response:

```json
no content
```

To commit a Basket we will need to <a href="https://visit.github.io/galaxy-docs/#add-booking-item">provide customer information</a> using our **BasketId**: 87654321. In the example, we have created Ms. Test User and will POST her data which will attach it to the basket. You will only receive a status code of 204 to indicate success. The basket now has the bare minimum required to commit it but we'll add one more thing first.

### HTTP Request

`GET https://galaxy.citybreak.com/api/basket/customer/87654321"`
