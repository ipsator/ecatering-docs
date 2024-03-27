# Delivery Partner APIs

2 type of api calls:
-------------------
- hydrogen api makes api call to aggregator apis
- delivery aggregator server makes a call to hydrogen api

API calls: 
----------

- from hydrogen to delivery aggregator api:

  * delivery aggregator will provide an auth token to make apis call with
  * every request will have auth token in the request header like this:
    Authorization: &lt;auth_token>

- from delivery aggregator server to hydrogen server
  * hydrogen server will also provide an api auth token to make api calls to it
  * every request will have auth token in the request header like this
    Authorization: &lt;auth_token>


Hydrogen to delivery aggregator api
-----------------------------------

- post order details on delivery aggregator server

**POST  https://&lt; delivery_aggregator_base_url>/order**

request body will be in this format:
    
```
{
  "orderId": "123324231",
  "customerDetails": {
    "customerId": 12,
    "customerName": "Abhinav Gupta",
    "email": "abhinav@ipsator.com",
    "mobile": "93426439274"
  },
  "outletDetails": {
    "id": 235,
    "name": "name of the outlet"
  },
  "bookingDate": "02-23-2018 06:07 IST",    // MM-DD-YYYY HH:mm Z"
  "deliveryDate": "02-23-2018 06:07 IST",    // MM-DD-YYYY HH:mm Z"
  "priceDetails": {
    "totalAmount": 5739,
    "amountPayable": 3243
  },
  "status": "ORDER_PLACED",
  "deliveryDetails": {
    "pnr": 323,
    "trainNo": "12532",
    "trainName": "Random Kranti Superfast",
    "station": "Lucknow Jn",
    "stationCode": "LKO",
    "berth": "23",
    "coach": "B1",
    "ETA": "02-23-2018 06:07 IST",    // MM-DD-YYYY HH:mm Z"
    "ETD": "02-23-2018 06:09 IST"    // MM-DD-YYYY HH:mm Z"
  },
  "orderItems": [
    {
      "itemId": 2232,
      "itemName": "something something",
      "description": "dhff d",
      "quantity": 1,
      "sellingPrice": 434,
      "option": "Regular"              // Again only for pizza
    }
  ],
  "paymentType": "CASH_ON_DELIVERY"
}
```

response will have following fields:

```
{
  "orderId": "123324231",
  "orderTrackingId": "234231",
  "status": "ORDER_CONFIRMED"
}
```

- get order from aggregator server

**GET  https://&lt; delivery_aggregator_base_url>/order/&lt;orderId>**

```
{
  "orderId": "123324231",
  "orderTrackingId": "234231",
  "status": "ORDER_OUT_FOR_DELIVEY"
}
```

- update order status on delivery aggregator server (For pushing cancelled status)

**POST https://&lt; delivery_aggregator_base_url>/order/&lt;order_id>/status**
    
request body will be like this:
```
{
    "status": "ORDER_CANCELLED"
}
```
    
response will be like this:
```
{
    "success": true
}
```

Delivery aggregator to Hydrogen api
-----------------------------------

- update order status on hydrogen server

**GET https://www.ecatering.irctc.co.in/api/v1/order/&lt;order_id>/track**      // NOT FINAL
    
    response will be:
```
{
    "ETA": "02-23-2018 06:07 IST"    // MM-DD-YYYY HH:mm Z"
}
```

- update order status on hydrogen server

**POST https://www.ecatering.irctc.co.in/api/v1/order/&lt;order_id>/status**      // NOT FINAL
    
request body will be like this:
```
{
    "status": "ORDER_DELIVERED"
}
```
    
response will be like this:
```
{
    "status": "success"
}
```    
the value of status can only be one of following:
- ORDER_PLACED
- ORDER_CONFIRMED
- ORDER_PREPARED
- ORDER_OUT_FOR_DELIVERY
- ORDER_DELIVERED
- ORDER_CANCELLED

In case of API error, delivery aggregator server should return HTTP status code 500 with the following response:

```
{
    "status": "error",
    "message: "some error message"
}
```
