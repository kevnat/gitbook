# Handling EMV Tag Data

### Description

For solutions supporting Bolt and therefore, EMV, certain data elements should be presented on cardholder receipts.

Tokens provided via Bolt will be securely stored within CardSecure with specific EMV data, and this data will be sent to the processor when the token is used in an authorization request. Additional data is usually returned from the processor and CardConnect will pass this through in a JSON array within the authorization response.

The maximum length for the “emvTagData” string is 2000 characters.

#### Guidance

The data returned should be presented on a reciept if applicable, and recorded with the transaction details for future reference. 

### Examples

{% code-tabs %}
{% code-tabs-item title="Example Authorization Response containing “emvTagData”" %}
```text
{
    "amount": "1.00",
    "resptext": "Approval",
    "commcard": " F ",
    "cvvresp": " ",
    "batchid": "116",
    "avsresp": "U",
    "respcode": "00",
    "merchid": "496000111880",
    "token": "9471234123456295",
    "authcode": "078823",
    "respproc": “RPCT",
    "emvTagData": "{\"TVR\":\"8080108000\",\"ARC\":\"5A33\",\"PIN\":\"None\",\"Signature\":\"true\"\"TSI\":\"E800\",\"Application Preferred Name\":\"DEBITO DE VISA\",\"Mode\":\"Issuer\",\"TSI\":\"6800\",\"Application Preferred Name\":\"DEBITO DE VISA\",\"AID\":\"A0000000980840\",\"IAD\":\"06010A03A00000\",\"Entry method\":\"Chip Read\",\"Application Label\":\"US DEBIT\"}",
    "retref": "122078257733",
    "respstat": "A",
    "account": "9471234123456295"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Example Inquire Response containing “emvTagData”" %}
```text
{
    "amount": "1.00",
    "resptext": "Approval",
    "setlstat": "Queued for Capture",
    "capturedate": "20180502160214",
    "respcode": "00",
    "batchid": "116",
    "merchid": "496000111880",
    "token": "9471234123456295",
    "respproc": "FNOR",
    "emvTagData": "{\"TVR\":\"8080108000\",\"ARC\":\"5A33\",\"PIN\":\"None\",\"Signature\":\"true\",\"Mode\":\"Issuer\",\"TSI\":\"6800\",\"Application Preferred Name\":\"DEBITO DE VISA\",\"AID\":\"A0000000980840\",\"IAD\":\"06010A03A00000\",\"Application Label\":\"US DEBIT\"}",
    "authdate": "20180502",
    "lastfour": "6295",
    "name": "",
    "currency": "USD",
    "retref": "122078257733",
    "respstat": "A",
    "account": "9471234123456295"
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### EMV Tag Data Table

| **Name** | **Tag** | **Details** | **Source** | **Format** | **Max Len** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TVR \(Terminal Verification Results\) | 95 | Status of the different functions as seen from the terminal | Terminal | binary | 5 |
| ARC \(Authorization Response Code\) | 8A | Indicates the transaction disposition of the transaction received from the issuer for online authorisations. | Issuer/Terminal | an 2 | 2 |
| PIN \(CVM Results\) | 9F34 | Indicates the results of the last CVM performed. If PIN was entered, then "Verified by PIN" | Terminal | String | 15 |
| Signature \(CVM Results\) | 9F34 | Indicates the results of the last CVM performed. If "true" then CVM supports signature and signature line may be applicable. However, card brands have moved away from requiring signature. | Terminal | Boolean | 5 |
| Mode |  | Identifies the mode used to authorize \(or decline\) the transaction.        Always "Issuer" | CardConnect | String | 6 |
| TSI \(Transaction Status Information\) | 9B | Indicates the functions performed in a transaction | Terminal | b | 2 |
| Application Preferred Name | 9F12 | Preferred mnemonic associated with the AID. If unavailable, use Application Label.  | Card | ans | 1-16 |
| AID \(Application Identifier, Terminal\) | 9F06 | Identifies the application as described in ISO/IEC 7816-5 | Terminal | binary 40-128 | 5-16 |
| IAD \(Issuer Application Data\) | 9F10 | Contains proprietary application data for transmission to the issuer in an online transaction. | Card | binary | 0-32 |
| Entry Method |  | Indicator identifying how the card information was obtained. | Terminal | String | 11 |
| Application Label | 50 | Mnemonic associated with the AID according to ISO/IEC 7816-5. If unavailable, use Application Preferred Name.  | Card | ans with the special character limited to space | 1-16 |



