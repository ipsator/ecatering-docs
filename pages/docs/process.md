# Product & Process

## Vendors
eCatering vendors are mainstream restaurants located within a certain range of railway stations. Some vendors have outlets located within station campuses, while most are a few kilometers away. Many vendors are partnered directly with IRCTC, while many others are available via aggregators(Zoop, Railofy, etc.) partnered with IRCTC.

## Delivery Challenge
Because the customer to whom food is to be delivered is in a moving train, the service deals with challenges non-existent in case of traditional home delivery services.
- Train’s arrival time & thus delivery time is subject to change (early arrival, delays, etc.)
- Train’s arrival platform & thus delivery location is subject to change
- The coach positions may not be aligned with the railway platform’s coach labels
- The customer might have switched seats during the journey
- The customer may not be reachable on phone due to network connectivity issues

## Cut-offs
Every outlet has a cut-off time set which denotes the duration of time (in minutes) requested by the outlet for preparing and processing orders. The base cut-off time requested by restaurants can be between 15 to 105 minutes.

Cut-offs are then calculated based on the base cutoff as:
- Outlet cutoff value 🠦 x minutes
- Customer order booking cutoff 🠦 x minutes (x + 15 minutes in case of prepaid only outlets)
- Customer cancellation cutoff 🠦 x + 10 minutes
- Customer payment cutoff 🠦 x + 5 minutes

These values are used by the system for various decisions like:
- when a placed order is pushed to vendor partner
- whether an outlet should be listed to the customer
- whether a new order can be placed at the outlet
- whether an order can be cancelled by customer
- whether a customer can pay online for a COD order
