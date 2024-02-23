# Conformance: SMART Capabilities

Bestätigungsrelevante Systeme in der Rolle eines ISiK-Ressourcenservers MÜSSEN eine _SMART Capabilities_ JSON-Datei als '.well-known'-Dokument (vgl. [RFC5785](https://datatracker.ietf.org/doc/html/rfc5785)) anbieten. Clients können auf diese Art und Weise u.a. abfragen, welche Kontexte und _Scopes_ seitens des ISiK-Ressourcenservers unterstützt werden.

ISiK-Ressourcenserver MÜSSEN dieses JSON-Dokument unter der URL bereitstellen, der durch Anhängen von ```/.well-known/smart-configuration``` an ihre Basis-URL gebildet wird. Die Kodierung der _SMART Capabilities_ MUSS den Vorgaben aus [SMART App Launch - 8.2 - FHIR Authorization Endpoint and Capabilities Discovery using a Well-Known Uniform Resource Identifiers (URIs)](https://hl7.org/fhir/smart-app-launch/STU2/conformance.html#using-well-known) entsprechen. 

## Vorgaben für ISiK-Connect - SMART Capabilities

Bestimmte SMART-Funktionalitäten werden in SMART Capability Sets zusammengefasst. Die Kodierung dieser Capability Sets MUSS den Vorgaben aus [SMART App Launch - 8.2 - FHIR Authorization Endpoint and Capabilities Discovery using a Well-Known Uniform Resource Identifiers (URIs)](https://hl7.org/fhir/smart-app-launch/STU2/conformance.html#using-well-known) entsprechen. Eine Beschreibung der Capability Sets ist ebenfalls der Kernspezifikation zu entnehmen. Folgende Capability Sets MÜSSEN auf Basis der vorliegenden Spezifikation verpflichtend unterstützt werden.

### Launch Modes

* ```launch-ehr```
* ```launch-standalone```

### Authorization Methods

* ```authorize-post```

### Client Types

* ```client-public```
* ```client-confidential-symmetric```
* ```client-confidential-asymmetric```

### Single Sign-on

* ```sso-openid-connect```

### Launch Context

* ```context-ehr-patient```
* ```context-ehr-encounter```
* ```context-standalone-patient```
* ```context-standalone-encounter```

### Permissions

* ```permission-offline```
* ```permission-patient```
* ```permission-user```
* ```permission-v2```

Darüber hinaus MÜSSEN folgende Angaben enthalten sein:

* ```authorization_endpoint```: Es MUSS die URL des Autorisierungs-Endpunkts des _OAuth2_-Autorisierungsservers angegeben sein, über die ein ISiK-Client eine Autorisierungsanfrage stellen kann.
* ```grant_types_supported```: Die _OAuth2 Grant Types_ ```authorization_code``` (Authorization Code Flow mit PKCE) UND ```client_credentials``` (Client Credentials) MÜSSEN unterstützt werden.  
  * Der _Grant Type_ ```refresh_token``` SOLL unterstützt werden. Wenn dieser _Grant Type_ angegeben ist, MUSS eine Erneuerung des _Access Token_ über ein _Refresh Token_ möglich sein.
* ```token_endpoint```: Es MUSS die URL des Token-Endpunkts des _OAuth2_-Autorisierungsservers angegeben sein, über die ein Zugriffstoken zur Bestätigung einer Autorisierung zum Zugriff auf geschützte Ressourcen des ISiK-Ressourcenservers abgerufen werden kann.
* ```code_challenge_methods_supported```: Es MÜSSEN die vom Autorisierungsserver unterstützten PKCE-Code-Challenge-Methoden angegeben sein. Die Methode ```S256``` MUSS unterstützt werden. Die Methode ```plain``` DARF NICHT unterstützt werden.
* ```scopes_supported```: siehe [Scopes und Kontexte](ConformanceScopesKontexte.md). Der ISiK-Ressourcenserver MUSS mindestens alle aufgeführten _Scopes_ unterstützen. Weitere _Scopes_ KÖNNEN unterstützt werden.

## Beispiel für eine Anfrage

``` 
GET /.well-known/smart-configuration HTTP/1.1
Host: fhirapi.cdr.example.com
```

## Beispiel für eine SMART Capabilities Datei

``` JSON
{
  "authorization_endpoint": "https://auth0.krankenhaus.de/auth/authorize",
  "token_endpoint": "https://auth0.krankenhaus.de/auth/token",
  "grant_types_supported": [    "authorization_code", "client_credentials"  ],
  "scopes_supported": [ "patient/Patient.rs", "patient/Observation.rs", "patient/Condition.rs" ],
  "response_types_supported": ["code"],
  "introspection_endpoint": "https://auth0.krankenhaus.de/user/introspect",
  "revocation_endpoint": "https://auth0.krankenhaus.de/user/revoke",
  "code_challenge_methods_supported": ["S256"],
  "capabilities": [ "permission-v2" ]
}
```
