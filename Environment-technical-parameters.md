<div id="TOC">

*   [LIVE parameters](#live-parameters)
*   [DEMO parameters](#demo-parameters)
*   [Test accounts for automated testing](#accounts)

</div>

# LIVE

## Live Parameters

|  Parameter | Value  |
|---|---|
|  base URL | https://rp-api.smart-id.com/v1/ |


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

|  EndResult | Country | national-identity-number | certificateLevel |
|---|---|---|---|
| OK | EE | 10101010005  | QUALIFIED |
| OK | LV | 010101-10006 | QUALIFIED |
| OK | LV | 020101-10000 | ADVANCED |
| OK | LT | 10101010005  | QUALIFIED |
| OK | LT | 10101020001 | ADVANCED |
| USER_REFUSED | EE | 10101010016 | QUALIFIED |
| USER_REFUSED | LV | 010101-1001 | QUALIFIED |
| USER_REFUSED | LV | 020101-10019 | ADVANCED |
| USER_REFUSED | LT | 10101010016 | QUALIFIED |
| USER_REFUSED | LT | 10101020012 | ADVANCED |
| TIMEOUT | EE | 10101010027 | QUALIFIED |

