# About ZelleSDK

The Turnkey Service for Zelle®: Mobile SDK (Mobile SDK) offers a turnkey solution for enabling 
an enhanced mobile user experience. The current web based hosted Zelle® UI offers the flexibility 
to be enabled in both online and mobile banking applications, but it does not enable access to 
the mobile device capabilities (e.g., contact list, camera, photo gallery, and sharing capabilities) 
that are expected with a native mobile app experience.

The Mobile SDK (native iOS and Android), integrated with the hosted Zelle® UI and 
the parent mobile banking application, will enable features that require access to 
the device capabilities. Those features are:

- Zelle® Ready Contacts
- QR Codes

In the current state, the Zelle® UI relies on the parent mobile application to access device capabilities. 
With the Mobile SDK, the SDK is responsible for accessing device capabilities.

## Mobile SDK Capabilities

### Access to Contacts

SDK provides access to contacts on a device and handles the user’s permission for 
native app to access contacts to add a single contact as a recipient or to pull a list of contacts 
who are enrolled in Zelle® and display those contacts in a recipient list as “Zelle Ready Contacts.”

### Access to Camera

SDK provides access to the camera on a device and handles the user’s permission for 
native app to access camera for scanning the QR code of a user’s token for sending or requesting a payment.

### Access to Photo Gallery

SDK provides access to the photo gallery on a device and handles the user’s permission for 
native app to access photos of the QR code of a user’s token for sending or requesting a payment.

### Access to Share Sheet (SMS, Email, Etc.)

SDK provides the ability to share or print QR codes via the share tray on a device.


 
 
