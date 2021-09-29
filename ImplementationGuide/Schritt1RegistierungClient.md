# Schritt 1: Registrierung einer SMART App mit dem bestätigungsrelevanten System

Bevor ein Client eine EHR Launch Sequence oder Standalone Launch Sequence ausführen kann MUSS der Client beim Authorisierungsserver des bestätigungsrelevanten Systems registriert werden. Per Abschnitt [1.0.3 Registering a SMART App with an EHR](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#registering-a-smart-app-with-an-ehr) ist er freigestellt wie diese Registrierung durchgeführt wird.

Der Authorisierungsserver MUSS die [Anforderungen an die Registrierung von Launch Urls und Redirect Uris](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#registering-a-smart-app-with-an-ehr) unterstützen. Es wird besonders auf die Anforderungen aus RFC8252 - Abschnitt [7.  Receiving the Authorization Response in a Native App](https://datatracker.ietf.org/doc/html/rfc8252#section-7) hingewiesen. Für Redirect Uris sind folgende Schemata zu untersützen:

- Private-Use URI Scheme Redirection
- Claimed "https" Scheme URI Redirection
- Loopback Interface Redirection

Durch die zuvor genannten Ausnahmen ergibt sich die Erforderniss, dass bei der Validierung der Redirect Urls (während Schritt 3 - "Bestätigungsrelevanten System evaluiert die Autorisierungsanfrage, Authentifizierung der Endnutzer") nicht davon ausgegangen werden kann, dass diese einen statischen Wert besitzen.

Der Authorisierungsserver vergibt auf Basis der Registrierung ein Client Id, welche zur eineindeutigen Identifizierung des Clients dient. Diese Client Id MUSS während der Authentifizierung des Clients bei einer Access Token Anfrage (siehe Schritt 4 - Austausch des Autorisierungscodes für ein Zugangstoken) verwendet werden.

Die Verwendung von [RFC7591 - OAuth 2.0 Dynamic Client Registration Protocol](https://datatracker.ietf.org/doc/html/rfc7591) wird ausdrücklich empfohlen, ist jedoch optional und nicht bestätigungsrelevant. Es ist zu beachten, dass derzeitig keine standartisierte Metadata Extension für die Registrierung der Launch Urls existiert. Diese kann beliebig gewählt werden.

## Beispiel

Folgender Request stellt beispielhaft dar welche Parameter bei einer Anfrage an einen Dynamic-Client-Registration-Endpunkt vorhanden sein können:

POST /register HTTP/1.1
Content-Type: application/json
Accept: application/json
Host: server.example.com

{
    "redirect_uris": ["https://example.org/redirect_uri/fhir/client/exampleId/"],
    "token_endpoint_auth_method": ["client_secret_basic", "client_secret_post", "private_key_jwt"],
    "grant_types": ["authorization_code", "refresh_token"],
    "response_types": ["code"],
    "client_name": "MyFhirClientName",
    "jwks_uri": "https://example.org/jwks/fhir/client/exampleId/jwks.json"
}

Response:

HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache

{
    "client_id": "TestClientId",
    "redirect_uris": ["https://example.org/redirect_uri/fhir/client/exampleId/"],
    "token_endpoint_auth_method": ["client_secret_basic", "client_secret_post", "private_key_jwt"],
    "grant_types": ["authorization_code", "refresh_token"],
    "response_types": ["code"],
    "client_name": "MyFhirClientName",
    "jwks_uri": "https://example.org/jwks/fhir/client/exampleId/jwks.json"
}