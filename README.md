### Startup
Go to http://localhost:8080/?plUserId=1612197&orderNo=00PL004&lang=sv&selectedTrackingNo=00PL16120004

### Issues
- Unable to remove translations. Setting a value to '' or null or false results in the default value being used.
- Component re renders more than once resulting in the functions inside the hook being called over and over. Main issue is the custom loading bar I have created flickering between the original icons and the new loading bar.