# Schritt 3: Bestätigungsrelevantes System evaluiert die Autorisierungsanfrage, Authentifizierung der Endnutzer

Um die Entscheidung zu treffen, ob eine Autorisierungsanfrage eines Clients zu akzeptieren oder abzulehnen ist, KANN der Autorisierungsserver eine Authentifizierung der Benutzer:in verlangen.
Sowohl die Smart App Launch Spezifikation als auch der vorliegende Implementierungsleitfaden legen keine Vorgaben diesbezüglich fest. Es ist darauf zu achten, dass bei einer fehlgeschlagenen Authentifizierung dem Endnutzer ein eindeutiger Fehlerhinweis angezeigt wird. Ein Redirect zum Client mit einem entsprechenden Fehlercode ist optional.

Im Falle einer erfolgreichen Authentifizierung MUSS der Autorisierungsserver die Parameter, welche unter [SMART App Launch - 2.0.9 - Obtain authorization code](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#step-4-authorization-code) dokumentiert sind, an den Client zurückliefern. Die Hinweise zur Gültigkeitsdauer des Autorisierungscodes MÜSSEN eingehalten werden.

Als Ergebnis dieses Schritts erhält der Client einen einmalig gültigen Autorisierungscode, welcher im weiteren Verlauf gegen ein Autorisierungstoken getauscht werden kann.

----

## Beispiel

Response:
```
Location: 
https://example.org/redirect_uri/fhir/client/exampleId?
code=123abc&
state=df01f5f8-5bf2-45ea-ab7a-706361da0515
```
