# ST_API
Arduino IDE Library for interacting with SmartThings API

The ST_API library provides the following functionality:
1. Execute a Rule
2. Execute a Scene
3. Get device online state
4. Get device status
5. Send commands to a device:
    1. Turn on or off
    2. Set dim level
    3. Turn on & set dim level
    4. Set color temperature 
    5. Set hue and saturation
    6. Refresh (for supported devices)
    7. Send arbitrary json command

To install the library:
1. Download the ST_API.zip file from my GitHub
2. In the Arduino IDE, select Sketches Menu->Manage Libraries ->Import Library from zip file and select the downloaded library
3. Restart the IDE

The library should work with any microcontroller that supports SSL clients and has sufficient memory. The example sketches have been successfully used with both ESP32 and MKR WiFi 1010 and have complied with several other Arduino microcontrollers

There are several example sketches included. To use the sketches, enter your location ID, PAT and WiFI credentials on the Secret.h tab. Add the necessary ST IDs (rules, scene and device IDs) in the ST_ID.h tab (SmartThings® API Browser+ is an excellent tool for finding IDs). There are sketches set up for both the ESP32 and MKR WiFi 1010. The only difference between the sketches is the #defines at the top of each sketch which selects the libraries and code for necessary for for the microcontroller for compilation.

Device command  json used for the 'arbitrary json command' should be validate, minified and the quotes escape prior to using in the sketches. jsonformatter.org  works well for checking and formatting json.

You can use unescaped minified json using raw C++ strings like this:
yourJSONvariable {R”(your-unescaped-minified-json-command)”};

Device json commands can have imbedded placeholders variables that can be changed during  program execution using snprintf. See ST_RawJSON_Command sketch for an example.

There are four levels for logging information to the serial monitor. Log level 1 shows both the request json sent to ST and the response json received from ST. The response json is helpful in determining key needed to extract data out of the response using the Capvalue() function fwhich can then be used by the sketch. Log level 2 and 3 logs various levels of the response header information. Log level 0 logs only the response code received from ST.

Limitations:
The SmartThing API takes 2-3 seconds per request (most the time is waiting for SmartThing API to send the response). Checking the device online state before making a device request (which is disabled by default) doubles the time.

Requesting device status from an offline device will succeed with stale device status/information. There is no indication that the device is offline, hence the need to check the online state to insure latest device status/information. Separate requests are needed to determine the online state and then send the device status request

This library uses the ST http client/server API for interactions. The  microcontroller must send  an appropriate request to the ST platform in order to get a response from ST.

