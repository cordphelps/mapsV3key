# Handling Google Maps V3 API keys for an html5 app built using PhoneGap Build.

[PhoneGap Build](https://build.phonegap.com/faq) is a cloud-based service that builds mobile apps from [html5](http://www.html5rocks.com/en/)-based resources. The combination is compelling; no IDE requirements, a javascript core with tangy [community support](http://stackoverflow.com/), and no additional tools to download and maintain on the workstation. Combined with [github](https://github.com/) as a code repository, your laptop can go swimming and your app project lives on.

This example app illustrates a method of configuring PhoneGap Build to manage a [google maps V3](https://developers.google.com/maps/) API key in an html5 iOS app.  


## Secret Sauce

- index.html loads the google maps V3 SDK without specifying a key

```xml
<script src="https://maps.googleapis.com/maps/api/js?v=3&sensor=true" >
</script>
```

- the PhoneGap Build config.xml injects the google maps v3 key for iOS into the info.plist resource file

```xml
<gap:config-file platform="ios" parent="GMSServices" overwrite="true">
    <array>
        <dict>
            <key>provideAPIKey</key>
            <string>YOUR-KEY-HERE</string>
        </dict>
    </array>
</gap:config-file>
```

So, my conjecture is that an iOS service, "GMSServices", underneath the app, is handing the API key to google on request. I have not been able to trace this transaction.....

## Screen shot

[![screen shot](https://github.com/cordphelps/mapsV3key/raw/master/screenshot.PNG)](https://github.com/cordphelps/mapsV3key)


## Installation

- get Apple Developer credentials and register your iOS device for development, establish a PhoneGap Build account, and pbB-install your credentials. 

- from the Google API console, activate the Google Maps SDK for iOS, then get a google maps v3 API key for iOS making sure that the iOS bundle identifier matches the one specified for your Apple (and pgB) credentials. Note that the geolocation service API is not necessary.

- install [QRReader](https://itunes.apple.com/us/app/qr-reader-for-iphone/id368494609?mt=8) on your iOS device.

- fork this repository (which represents an iOS app built with html5 with a pgB shim)

- edit config.xml, replace "YOUR-KEY-HERE" with your newly created google maps v3 API key and replace "YOUR-BUNDLE-ID-HERE" with your bundle ID.

- create a zip file containing the modified config.xml, index.html, css/*, and js/*

- upload the zip file to PhoneGap Build, create the demo app

- install the demo app by scanning the pgB created QR code using the QRReader app.


## Testing / Investigation Strategies

#### workstation-based testing

- use firefox to load http://localhost:8000/index.html from the project directory on your workstation. Observe network traffic with the [firebug plugin](http://getfirebug.com/) for firefox. By default, you are not using an API key when you load the SDK via http. OSX hint: start a local webserver from the project directory with

```bash
python -m SimpleHTTPServer 8000
```

- modify index.html (workstation testing only) to insert your google maps for iOS API key. comment out:

```xml
<script src="https://maps.googleapis.com/maps/api/js?v=3&sensor=true" >
</script>
```

then, remove comments, and insert your key:

```xml
<script src="https://maps.googleapis.com/maps/api/js?v=3&key=INSERT-YOUR-KEY-HERE&sensor=true" >
</script>
```

- re-load http://localhost:8000/index.html. You should get an error popup from google as the key you specified is for an iOS app. 

- enjoy experimenting with different keys. Investigate network traffic with FireBug (for browser observations) and [SquidMan](http://squidman.net/) and [tcpdump]() (for traffic originating from iOS). 


#### open issues

The google API console is not very friendly and it is unclear how quickly the data is updated.


## TODO:

Extend this cookbook to cover android keys.



## License
[MIT](http://www.opensource.org/licenses/MIT)

Software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.


## Author
Cord Phelps // [github](http://cordphelps.github.io)



