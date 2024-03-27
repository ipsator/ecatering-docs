# Order Flow

## PNR Input
As the service is only available to valid train tickets, PNR is mandatory to create orders.

Customers may enter their PNR booked from any ticketing service or proceed with a PNR booked via your service.

Orders can be created for PNRs with W/L & RAC seats. PNR status is checked before dispatching orders to vendors. In case a PNR is cancelled, the order gets cancelled automatically & refunds for any amounts paid is immediately initiated.

## Station List
Customers are displayed a list of stations on their journey that are serviceable by eCatering. A station has to be chosen from the list to proceed.
> API endpoint: /pnr/station/details?pnr=8730136580

The API returns a list of stations along with train & seat info for the given PNR. The station list is filtered out based on current journey progress (stations whose arrival time has crossed are not listed) & eCatering service hours.

![](/images/station-page.jpg)

### Edge cases / Key points

- PNR string not a 10-digit number → API error with relevant message. Input must be validated to avoid this.
- No PNR info due to unknown reasons, expired (flushed) PNR → API error with relevant message. Another PNR must be tried. Usually, retrying with the same PNR later results in the same error.
- PNR service is unavailable approximately between 23:30 to 00:30 daily due to limitations in underlying APIs → API error with relevant message. Customers must wait for this maintenance window to pass & retry.
- No upcoming serviceable stations in this journey → Empty station list. Usually, another PNR must be used. However, there can be some rare cases where a late train’s delay information might get updated later causing some stations to become serviceable for the same PNR. So, in case of late trains, at times retrying might help too.

## Restaurant List
Customers are displayed a list of restaurants actively serving at the chosen station when their train arrives there.
> API endpoint: /station/outlets?stationCode=KGP time=17:18&date=2021-03-06

The API returns a list of outlets (internal term for restaurants) sorted by their rating.
- Booking cut-off times of the outlets are taken into account apart from other factors like eCatering service hours, opening / closing times of outlets, etc.
- There is no upper or lower limits for the number of outlets available at a station
- The API response is strongly tied to the parameters passed in the request & the time of the request i.e. listed outlets may change for the same parameters when requested at a later time 
- Ratings are based on feedback sent for previous orders & recorded at outlet-level
- Due to cutoffs playing a major role in order creation flow, customers must be alerted about the same to avoid order failure later. eCatering apps use the term ‘order by’ to convey this.
- In context of the technical implementation, vendors are independent entities having one or more outlets. They may be of type VENDOR or AGGREGATOR.
- Although outlets belonging to aggregators (vendorType AGGREGATOR in vendor field of API response) are listed inline with other outlets (vendorType VENDOR) in the API response, aggregator outlets must be grouped together under the aggregator’s name while displaying to customers

![](/images/outlet-page.jpg)

### Edge cases / Key points
- No outlets serving at the requested station & arrival time → Empty outlet list. Customers must choose another station & try. However, sometimes outlets might be disabled temporarily and may get enabled later, outlets may change their base cutoff, etc. causing the list to become non-empty when queried at a later time.
- Past date/time used in the request → Empty outlet list. Only future time must be sent.
- New restaurant / no ratings yet → Default value of 3.3 for rating value used for sorting, 0 for count. Apt message may be displayed to customers.
- Cuisines not set by the vendor → Empty list. You should handle this accordingly.
- Same outlet may serve multiple stations in their vicinity. You must keep a record of the station chosen by the customer during hops.

## Menu Display

![](/images/menu-page.jpg)

## Feedback

Customers can submit feedback for their created orders, starting **from ETA + 15 mins & up to ETA + 5 days**.

Following information is collected:
- Customer received all items, partial items or nothing (mandatory)
- Star rating for the order (mandatory for delivered & partially delivered, omitted for non-delivery)
- Multiple choice feedback options for various parameters of order experience (optional for delivered & partially delivered, omitted for non-delivery)
- Comment (optional)

Based on the star-rating, options selected & comment, a feedback may be treated as a complaint ticket. The API response informs whether the feedback is to be considered a complaint or not.

Feedback options for various parameters of order experience are:

- For 4-star & above:
![](/images/good-rating.jpg)

- For 3-star & below:
![](/images/bad-rating.jpg)


