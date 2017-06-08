<div id="TOC">

*   [LIVE parameters](#live-parameters)
*   [DEMO parameters](#demo-parameters)
*   [Test accounts for automated testing](#accounts)

</div>

# LIVE

## Live Parameters

|  Parameter | Value  |
|---|---|
|  base URL | https://rp-api.smart-id.com/ |


* Add Smart-ID to your e-service! [Order now!](https://sk.ee/en/services/smart-id/)  
* [Contact](https://github.com/SK-EID/smart-id-documentation/wiki/Contact)



# DEMO

## DEMO Parameters

|  Parameter | Value  |
|---|---|
|  base URL | https://sid.demo.sk.ee/smart-id-rp/v1/ |
|  relyingPartyUUID | 00000000-0000-0000-0000-000000000000 |
|  relyingPartyName | DEMO |

Smart-ID login buttons. Free to use! [Download](https://github.com/SK-EID/smart-id-documentation/raw/master/files/Smart-ID_login.zip)  
NB! DEMO relyingPartyUUID has no access to Smart-ID Basic accounts.

*   When using "certificateLevel":"ADVANCED" in requests then HTTP error _403 Forbidden_ is returned.
*   When user has only Smart-ID Basic account then HTTP error _404 Not Found_ is returned.



# Test accounts for automated testing

## Accounts

|  EndResult | Country | national-identity-number |
|---|---|---|
| OK | EE | 10101010005 |
| USER_REFUSED | EE | 10101010016 |
| TIMEOUT | EE | 10101010027 |

