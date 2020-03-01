# Documentation Geolocation Example does not function as expected. #
## This is a review of the issues and code changes to address them. ##

* Packages:

> Node v10.15.0
> NPM v6.13.0
>  "dependencies": {
>  "expo": "~36.0.0",
>  "react": "~16.9.0",
>  "react-dom": "~16.9.0",
>  "react-native": "https://github.com/expo/react-native/archive/sdk-36.0.0.tar.gz",
>  "react-native-web": "~0.11.7",
>  "vue-native-core": "0.1.4",
>  "vue-native-helper": "0.1.4"

1. **Change package imports to match Expo API**
   - Addressed with PR: 
   - Docs import 'Constants' which do not appear to be required for code example

2. **Condition: location access denied**
   - Error: "Can't find variable: 'errorMessage'
   - Fix: Prefix errorMesssage with 'this' in getLocation method
   - Fix: Add <text>errorMessage</text> field to template (otherwise no output)

3. **Condition: When user denies permission for location access**
  - Error: "possible unhandled promise rejection"
  - Cause: Code does not trap denial and falls through to getCurrentPositionAsync
  - Fix: add conditional to getCurrentPositionAsync
   ```
    } else if (status.granted) {
    'code'
  ```
4. **Condition: User allows location access**
  - Error: no location display
  - Fix: There are two different keys that can be used to check permission status
  - Status.granted or Status.status
  - Fix: Change status test so that true/false status can be used to execute code

5. **Condition: No location display after status.status = true**
  - Error: No error message
  - Fix: Actual data returned from getCurrentPositionAsync is a Coords object
  - Fix: Change access position data object
    ```
     this.location = location1.coords;
     // add <text>{{ location }}</text> to template
     OR
     this.lng = location.coords.longitude
     // requires adding lng to data
    ```
6. **Condition: No location display after user grants permission**
  - Error: status.granted false
  - Fix: User may have global setting of location: never
  - Fix: Add example for testing global setting and add redirect
  - Status: Pending

7. **Revised code example**
   [Vue-native-geolocation](https://github.com/ravenOSS/Vue-native-geolocation)

8. **General Note**
  - Expo app and/or Expo server frequently require restart for state reset 
  - Presently getting DatePicker and TimePicker warnings for new merged packages.
  - Fix: Haven't found one yet.


*Please review and comment. I can attempt PR if accepted*