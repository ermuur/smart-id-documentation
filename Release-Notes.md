<div class="content-section">

## Release notes

11.10.2017

* New landing animation intro when starting registration starting from Android 5x and iOS 9x. Older devices show still image;
* Improved animations to make the registration process and usage more fluid. These include:
  Improved loader rotation, loader animating to success and fade-in success animations;
Slide animations between views - from right to left when moving forward and opposite when moving backwards;
All modals, alert and error views fade in and slightly slide in from top and disappear by fading out and sliding up;
Improved app universal responsive design - various breakpoints for different screen sizes have been added for resizing text, buttons, icons and PIN pad;
During registration, users can now see an improved progress bar indicating consecutive registration steps;
When registering a demo account, users can choose "Other countries" to generate a random personal code and use it to authenticate to demo portal;
First 10 characters of the push token are now visible in the menu for troubleshooting issues with support if push messages/transactions do not arrive automatically to the device;
Demo app also displays info if device has been rooted or jailbroken next to the device name in menu;
Improved error handling when registration times out and for some rare server side issues e.g. SSL pinning;
Improved error handling for issues related to data absence in population registries (PRE) needed for registration;
General technical error messages now also display error code or "Case ID" to better identify issues in support;
Notification messages are now cleared when transaction is opened manually in app (Android);
Notification messages are now cleared from the tray when there are no pending transactions (iOS);
Migrated from Google Cloud Messaging to Firebase Cloud Messaging;
Added UTM parameters (app version) to smart-id.com FAQ requests. User agent info is also sent with requests to portal
Updated hybrid dependencies and Android/iOS (third party) libraries;
General bug fixes, performance and usability improvements.

* Implemented and improved plugin based ID-card authentication in addition to TLS, both authentication methods supported on a single instance;
* Improved plugin detection and plugin min version check at the start of registration;
* Added a mobile browser check and warning view at the start and back-to-app animation at the end of ID-card registration;
* Added plugin distribution from config links for supported browsers;
* Terms & Conditions can now be downloaded from the self-service portal;
* New Latvian personal codes are now always allowed with NQ registration;
* User is now notified in advance when registration or self-service session is about to expire;
* User can now continue registration from self-service dashboard if there are no accounts or account registration is in progress;
* Updated instructions & FAQ links at the start of registration and added smart-id.com link to footer;
* Update hwcrypto, improved portal error logging for hwcrypto errors and added log requests for app and portal logging;
* Improved authentication and application signing error handling during ID-card registration;
* Improved error handling for issues related to data absence in population registries (PRE) needed for registration;
* General technical error messages now also display error code or "Case ID" to better identify issues in support;
* Demo app users can now log in to self-service by selecting "Estonia" when having checked "Other countries" in app during manual registration;
* General bug fixes, performance and usability improvements.

28.02.2017

*   More reliable PIN dialogue notification for Android
*   Personal code not shown on home screen
*   Fixed the bug which blocked registering account with multiple first names via banklink
*   Bug fixes, performance and usability improvements


02.01.2017

*   Smart-ID has a new look - white background for Smart-ID and blue for Smart-ID Basic
*   Full support for 5 languages implemented - in addition to English and Russian, we support all Baltic languages (Estonian, Latvian and Lithuanian)
*   Improved user experience - account registration with ID-card is made much easier for you!
*   Keeping in mind your safety and security reasons, easy PIN-codes (e.g 1234, 0000) are not allowed

20.12.2016

*   Due to popular demand, custom PIN selection is now set as default
*   ID-card registration optimisation: contact data is now entered in self-service portal
*   Audio notifications (for transactions) in iOS platform
*   Many visual tweaks and fixes

30.11.2016

*   ID-card based registration for Estonia, Latvia, Lithuania
*   Redesigned home screen, issuance method and country selection
*   Redesigned self-service portal
*   General UX improvements

02.11.2016

*   New visual
*   Added uservoice feedback system

25.10.2016

*   Added Latvian, Lithuanian and Russian languages
*   Added Google analytics
*   Improved push messages speed on iOS platform;
*   Dropped JSON/RPC integration API support.
*   Several changes in self service portal, including mobile view.

16.09.2016

*   Multilevel PIN locking. After three incorrect PIN entries, the authentication or signing function is locked for 3 hours. Upon another three incorrect PIN entries, the function is locked for 24 hours. After 24 hours the user has a possibility to enter the PIN codes three more times - if all entries are still invalid then the account is locked and certificates are revoked. To use Smart-ID the user must go through the registration process again.
*   Major changes as regards the issuance process (CSR signing). These are mostly internal changes and don't affect the user flow.
*   Some UX improvements
*   The first version of self service portal available at [https://sid.demo.sk.ee/portal/](https://sid.demo.sk.ee/portal) . The current version supports only login with Smart-ID, contains a possibility to get an overview of user accounts and to close an account

19.08.2016

*   Multi-device support (user has several active accounts on different devices). When user has multiple active accounts and a new transaction occurs, a confirmation screen is shown
*   Changed bank selection during banklink authentication. User has to choose first the bank and afterwards the country of the residence.
*   Implemented language changing. Currently allows to change between English and Estonian. By default, device system language is used.
*   Added bank authentication support for Swedbank customers in Lithuania
*   Added asking user phone number to the registration process. During registration user should enter his e-mail or phone number.
*   Improved push notification sending to iOS devices
*   Some UX improvements
*   RP JSON/RPC API improvements
*   Several internal improvements

01.07.2016

*   Added bank authentication support for Swedbank customers in Latvia

29.06.2016

*   Added manual registration possibility (in addition to Swedbank EE and SEB EE bank authentication)
*   Improved PIN-pad view
*   Added loader and Smart-ID logo to the bank authentication view header
*   Updated PIN creation flow in registration:
    *   Added PIN intro view
    *   Updated titles and descriptions
    *   Added "PIN1/2 saved" animation
    *   When using "Remember PIN”, then PIN needs to be confirmed only one time
*   Updated error message texts
*   Improved device name displaying (iOS)
*   “Wrong PIN message” view shows now attempts left (iOS)
*   Improved “Check transaction” button behaviour (iOS)

15.06.2016
* First public beta release


</div>