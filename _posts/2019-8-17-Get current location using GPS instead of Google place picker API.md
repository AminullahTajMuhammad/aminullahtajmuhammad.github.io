## Get current location using GPS instead of Google place picker API
![](https://miro.medium.com/max/1000/0*kvRD2syTAHvCMK4B)

According to the policy of google the place picker is depreciated in January and the new version of place picker is available but it’s very difficult and confusing.

So I have to find a new approach to get a full current location without using google place picker API. After some research, I got nothing special and then I experimented with my own logic.

This article is part of ***Today I Learn*** Category. Follow this article step by step and implement it on your projects and get full current address of your mobile phone.

So let’s started

So, for getting full address, first, we get latitude and longitude of the current location. because latitude and longitude help to find out the location. So below code shows how we get latitude and longitude of the current location.

````Kotlin 
private var longitude = 0.0
private var latitude = 0.0

private fun getLatLong() {
    val locationManager = getSystemService(Context.LOCATION_SERVICE) as LocationManager
    val listener = MyLocationListner()
    locationManager.requestLocationUpdates(LocationManager.GPS_PROVIDER, 0, 0f, listener)

}

private inner class MyLocationListner : LocationListener {
    override fun onLocationChanged(location: Location?) {
        if (location != null) {
            longitude = location.longitude
            latitude = location.latitude
        }
    }
    override fun onStatusChanged(provider: String, status: Int, extras: Bundle) { }
    override fun onProviderEnabled(provider: String) { }
    override fun onProviderDisabled(provider: String) { }
}
````
After that now calculate the full address with the help of latitude and longitude. So below code returns full address from latitude and longitude.

````Kotlin
private fun getFullAddressFromLatLong(latitude: Double, longitude: Double): String? {
        val geocoder = Geocoder(this, Locale.getDefault())
        fullAdress = ""

        try {

            var addresses: List<Address>? = null
            addresses = geocoder.getFromLocation(latitude, longitude, 1)

            if (addresses != null && addresses.size > 0)
                fullAdress = addresses[0].getAddressLine(0)

        } catch (e: IOException) {
            e.printStackTrace()
        }

        return 
} 
````

So after that, we get a full address from latitude and longitude. Now the main thing is android permissions. Location permissions are important without permission this code doesn’t work. So, add below permissions in your project manifest file.

````kotlin
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
````
