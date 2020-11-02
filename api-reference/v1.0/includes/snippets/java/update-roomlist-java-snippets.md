---
description: "Automatically generated file. DO NOT MODIFY"
---

```java

IGraphServiceClient graphClient = GraphServiceClient.builder().authenticationProvider( authProvider ).buildClient();

Roomlist place = new Roomlist();
place.displayName = "Building 1";
place.phone = "555-555-0100";
PhysicalAddress address = new PhysicalAddress();
address.street = "4567 Main Street";
address.city = "Buffalo";
address.state = "NY";
address.postalCode = "98052";
address.countryOrRegion = "USA";
place.address = address;
OutlookGeoCoordinates geoCoordinates = new OutlookGeoCoordinates();
geoCoordinates.altitude = nulld;
geoCoordinates.latitude = 47.0d;
geoCoordinates.longitude = -122.0d;
geoCoordinates.accuracy = nulld;
geoCoordinates.altitudeAccuracy = nulld;
place.geoCoordinates = geoCoordinates;

graphClient.places("Building1RroomList@contoso.onmicrosoft.com")
	.buildRequest()
	.patch(place);

```