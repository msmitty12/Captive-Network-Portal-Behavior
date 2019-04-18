# Captive Network Portal Behavior

> Documentation of captive network portal behavior on different devices

## Background

Public Wi-Fi networks offering short-term or temporary internet access commonly begin new connections using a Captive Portal Mini-Browser. This also happens in fixed and cellular networks in the case of insufficient balance for prepaid tariff plans. Each platform has a native version that is “baked” into the OS. There are also commercial browsers, like Firefox and Chrome that have this built-in support to detect and navigate the Captive Portal. This mode uses a web-view pushed to the client and has limited capabilities for branding, advertising & other monetization tactics along with limited capabilities with special authorization processes and paid access.

The aim for this project is to define existing use cases for client devices and client manufacturers. 


## Contents

- [Existing Device Behavior](#existing)
  - [All](#all)
  - [Apple](#apple)
    - [iOS](#ios)
    - [iOS 11+](#ios11)
    - [iOS 6+](#ios6)
    - [MacOS](#macos)
  - [Android](#android)
    - [9(P)](#9P)
  - [Google](#google)
    - [Pixel,Pixel2](#pixel)
- [List of captive portal check URLs](#urls)



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


<a name="macos"></a>
#### MacOS

- Native Mini-Browser – AKA “Captive Network Assistant” (CNA) - Notifies user about the need to log in by pushing the OS-level mini browser.


<a name="google"></a>
### Google

<a name="google_android"></a>
#### Google Android

- The Android OS determines the existence of the captive portal by attempting to access a list of domains (See appendix for complete list). If the domains are accessible, it can assume that it is not constrained by a captive portal. Otherwise, it will trigger the notification.
- When clicked, users are being redirected to CPMB.
- PostAuth experience – Once a user has successfully authenticated, the mini-browser may be hidden automatically or manually by pressing some special button.

## Code of Conduct

Please note that this project is released with a [Contributor Code of Conduct](code-of-conduct.md). By participating in this project you agree to abide by its terms.

## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [TBD](https://tbd.com/) has waived all copyright and related or neighboring rights to this work.
