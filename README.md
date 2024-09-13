

## ONRC InfoCert/InfoRBR integration API

------------------------------------------------------------------------------------------

#### Create a new request

<details>
 <summary><code>POST</code> <code><b>{uri}/api/partners/submitRequest</b></code> <code>(creates a new request)</code></summary>

##### Endpoint

> | Key      | Value               | description                                                           |
> |-----------|-------------------------|-----------------------------------------------------------------------|
> | uri      | String  | Provided by OpenCode (STAGING / PROD)  |

##### Headers

> | Key      | Value               | description                                                           |
> |-----------|-------------------------|-----------------------------------------------------------------------|
> | Authorization      | Basic Auth   | Provided by OpenCode  |
> | X-OCD-Partner      | String   | Provided by OpenCode  |

##### Body

> | name      |  type     | data type               | description                                                           |
> |-----------|-----------|-------------------------|-----------------------------------------------------------------------|
> | cui      |  required | String   | Company identifier (without "RO")  |
> | documentType      |  required | String   | From value list (see bottom) |
> | documentScope      |  required | String   | From value list (see bottom) |
> | priority      |  required | String   | Low / High  |
> | partnerRef      |  required | String   | Partner's **unique internal ID** of request  |

###### Example
```bash
curl -L 'https://$uri/api/partners/submitRequest' \
-u '$user:$password' \
-H 'X-OCD-Partner: $partnerId' \
-H 'Content-Type: application/json' \
-d '{
    "cui":  "32332105",
    "documentType":  "InfoCERT - Certificat constatator de bază",
    "documentScope":  "Agenția Națională pentru Ocuparea Forței de Muncă",
    "priority":  "Low",
    "partnerRef":  "12345"
}'
```

##### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`        | object (JSON)                             |
> | `400`         | `text/html;charset=utf-8` | None   |
> | `401`         | `text/html;charset=utf-8`         | None                                   |


##### Response Body

> | name      |   data type               | description                   |
> |-----------|-----------|-------------------------|
> | requestId      |   String   | Internal request ID  |

###### Example
```json
{
"requestId":  "jrurF1FhZ7nuyYAdy6Xm"
}
```

</details>

------------------------------------------------------------------------------------------

#### Cancel request

<details>
 <summary><code>POST</code> <code><b>{uri}/api/partners/cancelRequest</b></code> <code>(cancels a request - only from Created or RetrySendToONRC)</code></summary>

##### Endpoint

> | Key      | Value               | description                                                           |
> |-----------|-------------------------|-----------------------------------------------------------------------|
> | uri      | String  | Provided by OpenCode (STAGING / PROD)  |

##### Headers

> | Key      | Value               | description                                                           |
> |-----------|-------------------------|-----------------------------------------------------------------------|
> | Authorization      | Basic Auth   | Provided by OpenCode  |
> | X-OCD-Partner      | String   | Provided by OpenCode  |

##### Body

> | name      |  type     | data type               | description                                                           |
> |-----------|-----------|-------------------------|-----------------------------------------------------------------------|
> | requestId      |  required | String   | Internal request ID  |

###### Example
```bash
curl -L 'https://$uri/api/partners/cancelRequest' \
-u '$user:$password' \
-H 'X-OCD-Partner: $partnerId' \
-H 'Content-Type: application/json' \
-d '{
    "requestId": "jrurF1FhZ7nuyYAdy6Xm"
}'
```

##### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`        | object (JSON)    |
> | `400`         | `text/html;charset=utf-8` | None   |
> | `401`         | `text/html;charset=utf-8`         | None  |
> | `404`         | `text/html;charset=utf-8`         | None  |
> | `409`         | `text/html;charset=utf-8`         | None  |


##### Response Body

> | name      |   data type               | description                   |
> |-----------|-----------|-------------------------|
> | requestId      |   String   | Internal request ID  |
> | requestStatus      |   String   | Request Status - Cancelled  |

###### Example
```json
{
"requestId":  "jrurF1FhZ7nuyYAdy6Xm",
"requestStatus": "Cancelled"
}
```

</details>

------------------------------------------------------------------------------------------

#### Webhook request status

<details>
 <summary><code>POST</code> <code><b>{uri}</b></code> <code>(webhook update request status)</code></summary>

##### Endpoint

> | Key      | Value               | description                                                           |
> |-----------|-------------------------|-----------------------------------------------------------------------|
> | uri      | String  | Provided by Partner  |


##### Headers

> | Key      | Value               | description                                                           |
> |----------|---------------------|-----------------------------------------------------------------------|
> | Authorization      | Basic Auth or None   | Provided by Partner  |

##### Body

> | name      |  present on request status     | data type               | description                                                           |
> |-----------|-----------|-------------------------|-----------------------------------------------------------------------|
> | requestId      | all |   String   | Internal request ID  |
> | partnerRef      | all |   String   | Partner's unique internal ID of request  |
> | requestStatus      | all |   String   | Request Status  |
> | onrcPortalNo | SentToONRC | String | ONRC Portal Number (ID) |
> | docUri      | DoneONRC,Finalised|   String   | Direct download URI for generated document (present only if generated)  |
> | onrcInvoiceUri | Finalised* | String | Direct download URI for ONRC invoice (only for partners with self-invoice) |

###### Examples
```json
{
"requestId": "jrurF1FhZ7nuyYAdy6Xm",
"partnerRef":  "12345",
"requestStatus":  "SentToONRC",
"onrcPortalNo": "856012"
}
```
```json
{
"requestId": "jrurF1FhZ7nuyYAdy6Xm",
"partnerRef":  "12345",
"requestStatus":  "DoneONRC",
"docUri":  "https://firebasestorage.googleapis.com/v0/b/certificatconstatator-dev.appspot.com/o/2022_7_25_certificat273627-10S0Q.pdf?alt=media&token=ee42cf9c-c185-4291-9537-8bb518533218"
}
```
```json
{
"requestId": "jrurF1FhZ7nuyYAdy6Xm",
"partnerRef":  "d5f3af8e",
"requestStatus":  "Finalised",
"docUri":  "https://firebasestorage.googleapis.com/v0/b/certificatconstatator-dev.appspot.com/o/2022_7_25_certificat273627-10S0Q.pdf?alt=media&token=ee42cf9c-c185-4291-9537-8bb518533218",
"onrcInvoiceUri":  "https://firebasestorage.googleapis.com/v0/b/certificatconstatator-dev.appspot.com/o/_data1_portal_ccfil_certificate_2023_1_1_factura465298-MZX87.pdf?alt=media&token=a72fd4a8-b62d-43ab-a2de-5d34d7a2a353"
}
```

##### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`        | Any    |


</details>

------------------------------------------------------------------------------------------

#### Query request status *

<details>
 <summary><code>POST</code> <code><b>{uri}/api/partners/queryRequestStatus</b></code> <code>(query request status - as an async **alternative** to webhook implementation)</code></summary>

##### Endpoint

> | Key      | Value               | description                                                           |
> |-----------|-------------------------|-----------------------------------------------------------------------|
> | uri      | String  | Provided by OpenCode (STAGING / PROD)  |


##### Headers

> | Key      | Value               | description                                                           |
> |----------|---------------------|-----------------------------------------------------------------------|
> | Authorization      | Basic Auth   | Provided by OpenCode  |
> | X-OCD-Partner      | String   | Provided by OpenCode  |

##### Body

> | name      |  type     | data type               | description                                                           |
> |-----------|-----------|-------------------------|-----------------------------------------------------------------------|
> | requestId      |  required | String   | Internal request ID  |

###### Example
```bash
curl -L 'https://$uri/api/partners/queryRequestStatus' \
-u '$user:$password' \
-H 'X-OCD-Partner: $partnerId' \
-H 'Content-Type: application/json' \
-d '{
    "requestId": "jrurF1FhZ7nuyYAdy6Xm"
}'
```

##### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`        | object (JSON)    |
> | `400`         | `text/html;charset=utf-8` | None   |
> | `401`         | `text/html;charset=utf-8`         | None  |
> | `404`         | `text/html;charset=utf-8`         | None  |


##### Response Body

> | name        |   data type  | description                                       |
> |-------------|--------------|---------------------------------------------------|
> | requestId      |   String   | Internal request ID  |
> | partnerRef      |   String   | Partner's **unique internal ID** of request  |
> | requestStatus      |   String   | Request Status  |
> | onrcPortalNo | String | ONRC Portal Number |
> | docUri      |   String   | Direct download URI for generated document (present only if generated)  |
> | onrcInvoiceUri | String | Direct download URI for ONRC invoice (only for partners with self-invoice) |

###### Example
```json
{
"requestId": "jrurF1FhZ7nuyYAdy6Xm",
"partnerRef":  "12345",
"requestStatus":  "Finalised",
"onrcPortalNo": "856012",
"docUri":  "https://firebasestorage.googleapis.com/v0/b/certificatconstatator-dev.appspot.com/o/2022_7_25_certificat273627-10S0Q.pdf?alt=media&token=ee42cf9c-c185-4291-9537-8bb518533218",
"onrcInvoiceUri":  "https://firebasestorage.googleapis.com/v0/b/certificatconstatator-dev.appspot.com/o/_data1_portal_ccfil_certificate_2023_1_1_factura465298-MZX87.pdf?alt=media&token=a72fd4a8-b62d-43ab-a2de-5d34d7a2a353"
}
```

</details>

------------------------------------------------------------------------------------------

#### Query ONRC status

<details>
 <summary><code>POST</code> <code><b>{uri}/api/partners/queryOnrcStatus</b></code> <code>(query ONRC status)</code></summary>

##### Endpoint

> | Key      | Value               | description                                                           |
> |-----------|-------------------------|-----------------------------------------------------------------------|
> | uri      | String  | Provided by OpenCode (STAGING / PROD)  |


##### Headers

> | Key      | Value               | description                                                           |
> |----------|---------------------|-----------------------------------------------------------------------|
> | Authorization      | Basic Auth   | Provided by OpenCode  |
> | X-OCD-Partner      | String   | Provided by OpenCode  |


###### Example
```bash
curl -L 'https://$uri/api/partners/queryOnrcStatus' \
-u '$user:$password' \
-H 'X-OCD-Partner: $partnerId' 
```

##### Responses

> | http code     | content-type                      | response                                                            |
> |---------------|-----------------------------------|---------------------------------------------------------------------|
> | `200`         | `application/json`        | object (JSON)    |
> | `401`         | `text/html;charset=utf-8`         | None  |


##### Response Body

> | name        |   data type  | description                                       |
> |-------------|--------------|---------------------------------------------------|
> | isInfoCertActive      |   String   | "true" / "false" string values  |

###### Example
```json
{
"isInfoCertActive": "true"
}
```

</details>

------------------------------------------------------------------------------------------
#### Value Lists
<details>
 <summary>documentType</summary>
 
 ```javascript
 "INFOCERT_CONSTATATOR_PJ"
 "INFOCERT_CONSTATATOR_PF (in curand)"
 "INFOCERT_RAPORT_ISTORIC (in curand)"
 "INFORBR_SITUATIE_LA_ZI_PJ"
 "INFORBR_RAPORT_ISTORIC_PJ"
 "INFORBR_SITUATIE_LA_ZI_PF (in curand)"
 "INFORBR_RAPORT_ISTORIC_PF (in curand)"
 ```
</details>

<details>
 <summary>documentScope</summary>
 
 <blockquote>
 
<details>
	<summary>documentType = <code>"INFOCERT_CONSTATATOR_PJ"</code></summary>
	<blockquote>
	<code>*orice valoare text*</code>
</details>
<details>
	<summary>documentType = <code>"INFOCERT_CONSTATATOR_PF"</code></summary>
	<blockquote>
	<code>*orice valoare text*</code>
</details>
<details>
	<summary>documentType = <code>"INFORBR_RAPORT_ISTORIC_PJ"</code></summary>
	<blockquote>
	<code>"Informare"</code>
</details>
<details>
	<summary>documentType = <code>"INFORBR_SITUATIE_LA_ZI_PJ"</code></summary>
	<blockquote>
	<code>"Informare"</code>
</details>
<details>
	<summary>documentType = <code>"INFORBR_RAPORT_ISTORIC_PJ"</code></summary>
	<blockquote>
	<code>"Informare"</code>
</details>
<details>
	<summary>documentType = <code>"INFORBR_SITUATIE_LA_ZI_PF"</code></summary>
	<blockquote>
	<code>"Informare"</code>
</details>
<details>
	<summary>documentType = <code>"INFORBR_RAPORT_ISTORIC_PF"</code></summary>
	<blockquote>
	<code>"Informare"</code>
</details>
</details>

<details>
 <summary>requestStatus</summary>
 
> | Option   |  Description                                                           |
> |----------|----------------------------------------------------------------|
> | Created      | Request received and loaded to backend systems  |
> | Cancelled | Request cancelled by partner |
> | SendingToONRC      | In progress - API create ONRC request |
> | RetrySendToONRC | Postponed - API create ONRC request |
> | SentToONRC | Request is sent to ONRC and waiting for document |
> | DownloadONRC | In progress - check ONRC for document generation |
> | RetryDownloadONRC | Postponed - check ONRC for document generation |
> | DoneONRC | Document is generated and available |
> | InvoiceGeneratedONRC | ONRC invoice is generated and available |
> | Finalised | Request is finalised |

</details>
