<div class="content-section">

## Release notes

15.06.2018 v14 (demo)

* Smart-ID certificates will now be implementing 6K RSA keys, it was 4K RSA keys
* Made whole home screen tap/click to trigger check transaction (excluding top edge);
* Added "Existing accounts" modal after registration if user has more than one account;
* Added transaction timeout modal when less than 30 seconds left to complete transaction;
*Added better scroll indication and buttons visibility with longer text - when view is scrollable, text will start fading out below;
*Added Smart-ID Basic (NQ) warning view to banklink registration recommending to register a Smart-ID (Q) account instead;
*Added QSCD info to menu if account has QSCD certificate, displayed as “Smart-ID Qualified Electronic Signature”;
* Separated Settings and Application info into two menu sections;
* Improved error handling for older device operating systems that are not supported anymore and will require updating to a newer version;
* Added “Open system notification settings" option to menu and implemented notification channels on Android 8+:
* Added isCaptured check to warn about screen being captured/recorded (iOS);
* Added registering ID-card support to Edge;
* Implemented "Remember login details" functionality in self-service portal;
* General bug fixes, performance and usability improvements.

19.04.2018 v13

* Optimised layout for iPhone X;
* Added option to toggle "allow screenshot" functionality;
* "User info" is now also shown with unregistered account menu (only "Device" info is shown without account);
* App now shows transaction details along with verification code in the same view without having to toggle the verification code and details separately;
* Google Play now has also better differentiation between Smart-ID live and demo apps;
* Improved error handling for older app versions that are not supported anymore and will require updating to a newer version;
* Improved error handling for secure connections on iOS when issue is likely caused by poor Internet connection;
* General bug fixes, performance and usability improvements.

29.01.2018 v12

* Added home view error clearing;
* Added prettier for hybrid file formatting;
* Added timeout counter to RP request view;
* Added handling of RP request with type SIGNING;
* Added "Already have an account?" link to registration home view;
* Added timeout for entering PINs during identity token registration;
* Account status is now updated when app returns to foreground;
* App no longer shows "Tap code for details" disclaimer when RP has no displayText;
* Updated terms & conditions;
* Updated hybrid dependencies and Android/iOS (third party) libraries;
* Redesigned help menu:
  * Added self-service portal link to help menu (links also open in respective languages);
  * App and device info is added to e-mails sent to support;
  * Contacts are shown based on app language;
  * Removed 900 1807 number for Estonia;
* Improved different font sizes and layout breakpoints. Icons for warning & error messages now also resize with breakpoints;
* Improved check transaction error handling and logo animation logic;
* Improved key generation error handling;
* Improved general error handling:
  * Added "Session expired" error message;
  * Added error messages for various PRE errors;
  * Added "Account not found" error when push message arrived after account is deleted;
  * Added view for INVALID_PERSONAL_CODE error during manual and banklink registration;
  * Added “Poor internet connection” error with more detailed NSURLError codes instead of “Service temporarily unavailable” where applicable;
* Improved logging:
  * Added Lib errors ERROR_CODE_RESPONSE_TIMEOUT, ERROR_CODE_SERVER_INTERNAL_ERROR and App SSL exceptions to portal;
  * Added registration abandoned event logging when account is deleted in DOCUMENT_CREATED state;
* Input is scrolled into view when it is hidden after keyboard opens;
* Hide main webview while showing banklink and remove/pause loaders while not visible to reduce unnecessary rendering;
* Changed MID polling times. New values: initial delay = 15 sec, polling timeout = 3 sec;
* Fixed cancelling registration during key generation;
* Fixed user cancelling in SEB LT banklink views resulting in technical error;
* Fixed "Account deleted" view popping up before deletion process is completed;
* Fixed keyboard staying up when submitting form and navigating to following views (e.g. with keyboard enter button);
* Fixed resolving account state for account status view. Fixes animations and app not loading after deleting account in portal;
* Added Android root info to Menu (Android);
* Added scroll indicator (arrow) for banklink view (Android);
* Added timer to inactive transaction error views. App now closes automatically after time expires (Android);
* Implemented code obfuscation and optimisation with proguard. Removed Multidex (Android);
* Retry loading transaction when it initially fails due to ERROR_CODE_RESPONSE_TIMEOUT (Android);
* RP request view is now automatically hidden when new push arrives and active RP request is completed (Android);
* Notification (toast) is now shown in app when navigating to external URLs and device has no browser installed (Android);
* Potentially fix joda.time crash on some devices (Android);
* Fixed Crash on Android 4.x devices when pressing help icon in native Error view (Android);
* Removed automatic account deletion functionality - now user has to press button for account to be deleted. If it fails, error is shown;
* Fixed SSL error when opening Swebank banklink, also correct error message is now shown when Portal SSL pinning fails (Android);
* Fixed bug where back stops working when user manages to open two webviews (pressing buttons very fast) and when pressing back immediately (200ms) before banklink webview is loaded (Android);
* Fixed keyboard showing when pressing "Back" in banklink view while keyboard is opened (Android);
* Fixed "Registration terminated" showing up every time app is started after app was killed after creating PINs in ID-card registration (Android);
* Portal shared cookies are now added to banklink authentication request (iOS);
* Improved CSR transaction error handling when no result is returned from Lib (iOS);
* Fixed Safari 9 buttons not working issue in Terms and MID application view when user has scrolled to the end or back to the beginning;
* Fixed missing parent signing links for minors registration (iOS);
* Fixed menu not closing properly on iOS 11.2 when changing language (iOS);
* Fixed being able to start MID signing multiple times because UI problems (iOS);
* Fixed CSR transaction shown when returning to registration from help view (iOS);
* Fixed pdf icons not rendering correctly in iOS <=8.3. Works when dimensions are multiples of 8 (iOS);
* General bug fixes, performance and usability improvements.

* Added more PRE error messages;
* Added app minimum version check;
* Added method for app to log events;
* Added a "X-Request-ID" header to all CSI requests;
* Added BEGIN and END tags to certificates downloaded from self-service;
* Added more detailed error handling to ID-card authentication to self-service;
* Added notification about updating certificates on self-service ID-card authentication failure;
* Added FAQ link with instructions to the start of minors registration;
* Added FAQ link redirecting Latvians with new personal code to go to bank office for registration;
* Redesigned self-service login page with redirects to start/continue registration;
* Self-service now shows notification about old accounts for Latvians with new personal code;
* Self-service now logs user out if the Smart-ID account that was used to log in is deleted through self-service;
* Selecting cancel on SEB Lithuanian banklink now sends the app a more appropriate error message;
* Updated UI & backend third-party libraries;
* Re-enabled plugin based ID-card authentication to self-service;
* Removed some duplicate logging from banklink registration flow;
* Changed the Estonian support number fallback to only +(372) 715 1606;
* The session expiry dialog no longer shows during registration before authentication and after completion;
* Portal language can now be set with the lang URL parameter and signing links now direct to the correct language;
* Improved error handling if the is-logged-in request fails on page load and error messages for plugin missing and outdated;
* Slightly improved MID and ID-Card error responses and removed a deprecated method;
* Fixed app event logging API.
* Fixed terms & conditions button disappearing sometimes;
* Fixed occasional incorrect tracking ID on "Register button clicked" log event.
* Fixed a bug where user data was requested twice after logging into self-service;
* Fixed EULA with ID-card minor registration, links in terms & conditions are also now clickable;
* Fixed a bug when clicking on an e-mail address would sometimes prefill email with an incorrect message;
* General bug fixes, performance and usability improvements.

11.10.2017 v10

* New landing animation intro when starting registration starting from Android 5x and iOS 9x. Older devices show still image;
* Improved animations to make the registration process and usage more fluid. These include:
   * Improved loader rotation, loader animating to success and fade-in success animations;
   * Slide animations between views - from right to left when moving forward and opposite when moving backwards;
   * All modals, alert and error views fade in and slightly slide in from top and disappear by fading out and sliding up;
* Improved app universal responsive design - various breakpoints for different screen sizes have been added for resizing text, buttons, icons and PIN pad;
* During registration, users can now see an improved progress bar indicating consecutive registration steps;
* When registering a demo account, users can choose "Other countries" to generate a random personal code and use it to authenticate to demo portal;
* First 10 characters of the push token are now visible in the menu for troubleshooting issues with support if push messages/transactions do not arrive automatically to the device;
* Demo app also displays info if device has been rooted or jailbroken next to the device name in menu;
* Improved error handling when registration times out and for some rare server side issues e.g. SSL pinning;
* Improved error handling for issues related to data absence in population registries (PRE) needed for registration;
* General technical error messages now also display error code or "Case ID" to better identify issues in support;
* Notification messages are now cleared when transaction is opened manually in app (Android);
* Notification messages are now cleared from the tray when there are no pending transactions (iOS);
* Migrated from Google Cloud Messaging to Firebase Cloud Messaging;
* Added UTM parameters (app version) to smart-id.com FAQ requests. User agent info is also sent with requests to portal
* Updated hybrid dependencies and Android/iOS (third party) libraries;
* General bug fixes, performance and usability improvements.

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

29.07.2017 v9

* Redesigned Menu;
* Redesigned "Waiting approval" dialogues with ability to restart registration from menu if needed;
* Help view now has app and lib versions visible;
* Help view now has 2 support contact phone numbers displayed for all countries;
* Help view now has white background (equals to Q design) if there are no active accounts;
* Added timeout for multiple account transaction request views;
* Added “National ID number not supported” error view when registering with ID-card and new Latvian ID-code;
* Added “Invalid country code” error view when registering with bank link and bank returns INVALID_COUNTRY_CODE;
* Added small help disclaimer for multiple account transaction request views in case of receiving extra notification screens per transaction;
* Error views now direct to FAQ articles directly where applicable;
* New "How To Use Smart-ID" onboarding cards when completing registration;
* Improved minors registration flow, users under 18 can now register for a Smart-ID account;
* Moved Terms & Conditions view immediately after authentication method selection to avoid timeouts when reading too long;
* Readiness for electronic Qualified certificate issuance (EE, LT) which will be turned on later on July;
* Implemented transaction pending local state retry logic. When transaction submission fails due to network error, then operation can be retried without additional PIN entry;
* Fixed “Try again” in error view, when app is started without internet;
* Fixed ID-card registration code not refreshing when app returns to foreground.
* Fixed network error when receiving push message when power saver is turned on on some devices (Android);
* Push upon arrival plays tune (Android);
* Added option in menu to toggle notification sound on or off. Default is on (Android);
* Control code is now always shown on new transactions (iOS);
* Pressing transaction cancel now shows loader and error view if canceling fails (iOS);
* “Navigate back to e-service” dialog is now shown on after completing transaction iOS9+ (iOS);
* “Notifications disabled” dialog is now always shown after transaction when notifications are disabled (iOS);
* General bug fixes, performance and usability improvements.

* Added message to loader on signing application;
* Added basic validation to legal guardian info from PRE;
* Added tooltip for copy share content to clipboard button;
* Added link to installer.id.ee/plugins on the missing plugins view;
* Added multi doc format support and visible signature option for iSign;
* Added support for Latvia SEB bank link onboarding for corporate customers;
* Added new LV personal code support for NQ registration without age checking;
* Added server session timeout manually for every session created and timeout for ID-card signing;
* Added logging for register button clicks, missing plugins and bank-link handle-result session error;
* Added custom styling and visuals for iSign gateway used for minors registration parent signing flow;
* Updated copy, header and footer;
* Updated login view visuals including M-ID & ID-card logos in login view and minors application signing;
* Implemented new minors registration logic;
* Implemented iSign API with improved logging for minors issuance;
* Implemented RA-API and application saving for minor electronic issuance;
* Portal now returns INVALID_COUNTRY_CODE in case country code is not EE, LV or LT;
* Download cert buttons are now hidden behind a "..." button in self-service dashboard;
* Fixed account delete issues, fixed account delete loader;
* Fixed session invalidation when cancelling minors registration;
* Fixed RA-API error handling, refactored general registration error handling;
* Fixed iframe reload issue on language change, added modal for cancel conformation;
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