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

## Smart-ID Basic (ADVANCED) level accounts

DEMO relyingPartyUUID has no access to Smart-ID Basic accounts. If you would like to test with Smart-ID Basic accounts please write to support@sk.ee .

*   When using "certificateLevel":"ADVANCED" with wrong relyingPartyUUID in requests then HTTP error _403 Forbidden_ is returned.
*   When user has only Smart-ID Basic account then HTTP error _404 Not Found_ is returned.



# Test accounts for automated testing

## Accounts

|  EndResult | Country | national-identity-number | certificateLevel | Document Number |
|---|---|---|---|---|
| OK | EE | 10101010005  | QUALIFIED | PNOEE-10101010005-Z1B2-Q |
| OK | LV | 010101-10006 | QUALIFIED | PNOLV-010101-10006-SGT7-Q |
| OK | LV | 020101-10000 | ADVANCED | PNOLV-020101-10000-96R2-NQ |
| OK | LT | 10101010005  | QUALIFIED | PNOLT-10101010005-Z52N-Q |
| OK | LT | 10101020001 | ADVANCED | PNOLT-10101020001-K87V-NQ |
| USER_REFUSED | EE | 10101010016 | QUALIFIED | PNOEE-10101010016-9RF6-Q |
| USER_REFUSED | LV | 010101-1001 | QUALIFIED | PNOLV-010101-10014-782V-Q |
| USER_REFUSED | LV | 020101-10019 | ADVANCED | PNOLV-020101-10019-X69J-NQ
| USER_REFUSED | LT | 10101010016 | QUALIFIED | PNOLT-10101010016-3JGR-Q |
| USER_REFUSED | LT | 10101020012 | ADVANCED | PNOLT-10101020012-FFQP-NQ |
| TIMEOUT | EE | 10101010027 | QUALIFIED | PNOEE-10101010027-TFPR-Q |

