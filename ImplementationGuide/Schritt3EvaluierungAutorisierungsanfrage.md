# Schritt 3: Bestätigungsrelevanten System evaluiert die Autorisierungsanfrage, Authentifizierung der Endnutzer

Um die Entscheidung zu treffen ob eine Authorisierungsanfrage eines Clients zu akzeptieren oder abzulehnen ist, KANN der Autorisierungsserver eine Authentifizierung des Benutzers verlangen.
Sowohl die Smart App Launch Spezifikation als auch der vorliegende Implementierungsleifaden legen keine Vorgaben diesbezüglich fest. Es ist darauf zu achten, dass bei einer fehlgeschlagenen Authentifizierung dem Endnutzer ein eindeutiger Fehlerhinweis angezeigt wird. Ein Redirect zum Client mit einem entsprechenden Fehlercode ist optional.

Im Falle einer erfolgreichen Authentifizierung MUSS der Autorisierungsserver die Parameter, welche unter [1.0.6.1.2 Step-2: EHR evaluates authorization request, asking for end-user input](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#step-2-ehr-evaluates-authorization-request-asking-for-end-user-input) dokumentiert sind an den Client zurückliefern. Die Hinweise zur Gültigkeitsdauer des Authorisierungscodes MÜSSEN implementiert werden.

Als Ergebnis dieses Schritts erhält der Client einen einmalig gültigen Authorisierungscode, welcher im weitere Verlauf gegen ein Authorisierungstoken getauscht werden kann.

----

## Beispiel

Response:

Location: https://example.org/redirect_uri/fhir/client/exampleId?code=123abc&state=df01f5f8-5bf2-45ea-ab7a-706361da0515
