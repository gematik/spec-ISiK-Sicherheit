# Schritt 1: Registrierung einer SMART App mit dem bestätigungsrelevanten System

Bevor ein Client eine EHR Launch Sequence oder Standalone Launch Sequence ausführen kann MUSS der Client beim Authorisierungsserver des bestätigungsrelevanten Systems registriert werden. Per Abschnitt [1.0.3 Registering a SMART App with an EHR](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#registering-a-smart-app-with-an-ehr) ist er freigestellt wie diese Registrierung durchgeführt wird.

Der Authorisierungsserver MUSS die [Anforderungen an die Registrierung von Launch Urls und Redirect Uris](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#registering-a-smart-app-with-an-ehr) unterstützen. Es wird besonders auf die Anforderungen aus RFC8252 - Abschnitt [7.  Receiving the Authorization Response in a Native App](https://datatracker.ietf.org/doc/html/rfc8252#section-7) hingewiesen. Für Redirect Uris sind folgende Schemata zu untersützen:

- Private-Use URI Scheme Redirection
- Claimed "https" Scheme URI Redirection
- Loopback Interface Redirection
