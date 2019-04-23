# Captive Network Portal Behavior

> Documentation of captive network portal behavior on different devices

## Background

Public Wi-Fi networks offering short-term or temporary internet access commonly begin new connections using a Captive Portal Mini-Browser. This also happens in fixed and cellular networks in the case of insufficient balance for prepaid tariff plans. Each platform has a native version that is “baked” into the OS. There are also commercial browsers, like Firefox and Chrome that have this built-in support to detect and navigate the Captive Portal. This mode uses a web-view pushed to the client and has limited capabilities for branding, advertising & other monetization tactics along with limited capabilities with special authorization processes and paid access.

The aim for this project is to define existing use cases for client devices and client manufacturers. 


## Contents

- [Existing Device Behavior](#existing)
  - [All](#all)
    -[Mobile](#mobile)
      - [Apple](#apple)
        - [iOS](#ios)
        - [iOS 11+](#ios11)
        - [iOS 6+](#ios6)
      - [Android](#android)
        - [9(P)](#9P)
      - [Google](#google)
        - [Google Android](#google-android)
        - [Samsung Android](#samsung-android)
        - [Android 5](#android-5)
    -[Desktop](#desktop)
      - [Apple](#desktop-apple)
        - [MacOS](#macos)
      - [Microsoft](#microsoft)
        - [Windows 10](#windows10)
      - [Google](#desktop-google)
        - [ChromeOS](#chromeos)
      - [Linux](#linux)
        - [Ubuntu](#ubuntu)
        - [Linux (Firefox Browser Installed)](#linux-firefox)
        - [Linux (Chrome Browser Installed)](#linux-chrome)
      - [Amazon](#amazon)
        - [FireOS](#fireos)   
- [List of captive portal check URLs](#urls)
- [Code of Conduct](#conduct)
- [License](#license)


## Existing Device Behavior

<a name="all"></a>
### All

- There are no persistent cookies in CPMB: all the written cookies are destroyed after CPMB closes. [check on Android last changes]

- CPMB closes after authorization is completed (sometimes it needed additional actions from the user)

- The CPMB disappears and the device disconnects from the network when focus is changed to another app, such as SMS or email.

- Most of external services (file system, applications and etc.) are not accessible from SPMB [Add in this section some information about differences between devices behavior in PreAuth vs PostAuth along with some special limitations of CNA and CPMB: memory usage, local storage, JS support and etc.]

<a name="apple"></a>
### Apple

<a name="ios"></a>
#### iOS

- Standard flow for Captive Network authorization process starts from the Wi-Fi association process. It doesn’t matter what kind of Wi-Fi association protocol is used (Hotspot 2.0 or older one): in all cases just after the association is complete, the device making a request for IP-address (DHCP DISCOVER).

- After receiving of IP-address the device goes to check http://captive.apple.com/hotspot-detect.html (exact domain and URI could be different from this one: see appendix for complete list) via so-called CNA Helper. 

- In this request the device uses specific User-Agent: “CaptiveNetworkSupport-355.200.27 wispr” (the version mentioned could be different). Received answer is analyzing for Web-authorization existence and in case of detection, Wi-Fi network marks as captivated (appropriate switches appears in the network settings), and switching from cellular connection to Wi-Fi doesn’t appear otherwise the device switches to the Wi-Fi connection as major one.

- In case of association with known SSID of Captive Network when device is not active (locked state in a pocket, for example), there is no further requests produced by device before unlocking. After this device is unlocked, additional checking request appears and in case of Web-authorization confirms, CPMB is rising.

- When CPMB is risen it generate additional request to http://captive.apple.com/hotspot-detect.html (see appendix for complete list) but with different kind of User-Agent: “Mozilla/5.0 (iPhone; CPU iPhone OS 12_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/16A366” (the version mentioned could be different). During authorization process in CPMB, almost all network actions followe4d by additional checks of current state.

- When the CPMB detects that the captive domain is accessible, it changes the control button in the top right corner from “Cancel” to “Done” and switches the primary connection from cellular network to Wi-Fi. The CPMB only performs a check of the captive domain after a full URL navigation or by timer. A javascript-triggered AJAX call for example will not result in the CPMB performing an additional check of the captive domain. With a single page app that does not redirect to a separate post-authentication landing page, a full page navigation must occur for the CPMB to change the control button from “Cancel’ to “Done”. If this doesn’t happen, the user’s device will be authenticated and connected to Wi-Fi but the CPMB will only provide the “Cancel” option, which will disassociate the device from the SSID and take the user back the Wi-Fi network selection screen in the Settings menu.

- Sometimes, [Need to clarify conditions to change network type] after several connections to the Wi-Fi network without Captive Portal with the same SSID as used in Captive Network, iOS may switch off Captive Checker for this particular SSID and there will no Captive Browser rising


<a name="ios11"></a>
#### iOS 11+
- (VPN or some other software that blocks Captive Detector Installed)
- Normally, there is a local Push-notification raise instead of CPMB.


<a name="ios6"></a>
#### iOS 6+

- (Switched off the “Auto-connect” selector in the Wi-Fi SSID settings.)
-There is no CPMB rising. Manual redirection is needed.


<a name="google"></a>
### Google

<a name="google_android"></a>
#### Google Android (Pixel, Pixel2, Pixel3)

- The Android OS determines the existence of the captive portal by attempting to access a list of domains (See appendix for complete list). If the domains are accessible, it can assume that it is not constrained by a captive portal. Otherwise, it will trigger the notification.
- When clicked, users are being redirected to CPMB.
- PostAuth experience – Once a user has successfully authenticated, the mini-browser may be hidden automatically or manually by pressing some special button.


<a name="samsung_android"></a>
#### Samsung Android

- Active Captive Portal - Notifies user about the need to log in by pushing the OS-level mini browser.
-The Android OS determines the existence of the captive portal by attempting to access a list of domains (See appendix for complete list). If the domains are accessible, it can assume that it is not constrained by a captive portal. Otherwise, it will pops up Captive Portal or Full browser.
-Post-auth experience – Once a user has successfully authenticated, the mini-browser may be hidden automatically or manually by pressing some special button.
-It can be artificial ad block on CPMB in some Android devices.

<a name="android_5"></a>
### Android 5.0.2

- Google Chrome is opened instead of CPMB.


<a name="desktop"></a>
### Desktop

<a name="desktop-apple"></a>
#### Apple

<a name="macos"></a>
#### MacOS

- Native Mini-Browser – AKA “Captive Network Assistant” (CNA) - Notifies user about the need to log in by pushing the OS-level mini browser.


<a name="microsoft"></a>
#### Microsoft

<a name="windows10"></a>
##### Windows 10

- Notifies user about the need to log in by opening the user’s default browser and attempting to redirect the user to a default HTTP destination which should be intercepted by the network.


<a name="desktop-google"></a>
#### Google

<a name="chromeos"></a>
##### Chrome OS

- Connection manager for Chromium OS attempts to retrieve the web page [http://clients3.google.com/generate_204](ttp://clients3.google.com/generate_204). This well known URL is known to return an empty page with an HTTP status 204. If for any reason the web page is not returned, or an HTTP response other than 204 is received, then shill marks the service as being in the portal state.
- Other captive portals, sometimes run by cellular carriers, provide absolutely no IP connectivity other than to their own servers, but they use a standard DNS server and do not intercept HTTP requests. When a Chrome Book connects to this type of network, the HTTP requests fail because the TCP connection to clients3.google.com can never be established. The portal code tries multiple times for up to 10 seconds to connect to clients3.google.com. If it cannot connect it marks the service as being in a captive portal. This determination is somewhat unreliable because very high latency connections, lossy connections and other network issues can also result in failure to connect to clients3.google.com. All of these are indicative of a network that is not fully functional, but they do not necessarily indicate that the machine is stuck in a captive portal.

<a name="linux"></a>
#### Linux

<a name="ubuntu"></a>
##### Ubuntu

- Doesn’t support OS level captive portal login in available LTS (long term support) releases so far. Now there is a discussion in Ubuntu WiKi networking section where it is proposed to provide OS level support in coming releases. GNOME (GNU Network Object Model Environment) had introduced automatic login prompt for Wi-Fi captive portals (Wi-Fi access points which required web based login, such as those found in public places) quite a while ago. However this functionality is still unavailable in Ubuntu GNOME, The same is part of wish-list in upcoming Ubuntu LTS releases.

<a name="linux-firefox"></a>
#### Linux (Firefox Browser Installed)

- Firefox introduces automatic detection of Captive portals and notifies user about the need to log in. Additionally, after Firefox detects a Captive portal, it replaces certificate error pages with a message encouraging user to log in.
- Firefox determines the existence of a captive portal constraint by attempting to download the file success.txt from http://detectportal.firefox.com/success.txt (there is only one word in that file, the word "success". If it can successfully retrieve that file, it can assume that it is not constrained by a Captive portal. Otherwise, it will trigger an in-browser notification.

<a name="linux-chrome"></a>
#### Linux (Chrome Browser Installed)

-Chrome introduces automatic detection of Captive portals and notifies user about the need to log in. Additionally, after Chrome detects a Captive portal, it replaces certificate error pages with a message encouraging user to log in.

<a name="amazon"></a>
#### Amazon

<a name="fireos"></a>
##### FireOS

- Fire OS (based on Android) uses http://spectrum.s3.amazonaws.com/kindle-wifi/wifistub.html for connectivity checks and if the URI is not reachable, a notification appears indicating a captive portal login is required.
- Tapping the notifications triggers the the captive portal mini-browser to open, the device attempts to reach http://spectrum.s3.amazonaws.com/kindle-wifi/wifiredirect.html and the user is greeted with “Unsecured Connection. This connection is not secure. When using an unsecured connection, your personal information may be visible to others.” After clicking continue, the captive portal loads.
- The Unsecured Connection warning appears to be triggered by the attempt to hit the “wifiredirect” page over HTTP, not because of a TLS certificate mismatch.


<a name="captive-portal-urls"></a>
## List of captive portal check URLs

### Apple iOS
- www.apple.com
- www.appleiphonecell.com
- captive.apple.com
- www.airport.us
- www.ibook.info
- www.itools.info
- www.thinkdifferent.us
- apple.com


### Apple MacOS
- captive.apple.com


### Google Android
- clients3.google.com
- clients4.google.com
- android.clients.google.com
- connectivitycheck.android.com
- connectivitycheck.gstatic.com
- www.gstatic.com
- www.google.com
- www.androidbak.net


### Samsung Android
- http://connectivitycheck.android.com/generate_204
- http://connectivitycheck.gstatic.com/generate_204
- d2uzsrnmmf6tds.cloudfront.net


### HTC Android
- clients3.google.com

### Windows
- msftncsi.com

<a name="conduct"></a>
## Code of Conduct

Please note that this project is released with a [Contributor Code of Conduct](code-of-conduct.md). By participating in this project you agree to abide by its terms.

<a name="license"></a>
## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [TBD](https://tbd.com/) has waived all copyright and related or neighboring rights to this work.
