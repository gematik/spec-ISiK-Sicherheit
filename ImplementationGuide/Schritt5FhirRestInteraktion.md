# Schritt 5: FHIR Restful Interaktion

Das erhaltene Acces Token kann durch den Client am FHIR-Endpunkt des bestätigungsrelevanten System eingelöst werden. Hierzu ist bei jeder Restful-Interaktion ein Authorization-Header mitzusenden. Das Token ist per [RFC6750 -  The OAuth 2.0 Authorization Framework: Bearer Token Usage](https://datatracker.ietf.org/doc/html/rfc6750) zu kodieren.

Bestätigungsrelevant sind die Anforderungen nach [SMART App Launch - 2.0.11 - Access FHIR API](http://build.fhir.org/ig/HL7/smart-app-launch/app-launch.html#step-6-fhir-api). Unter anderem MÜSSEN folgende Validierungschritte seitens des bestätigungsrelevanten Systems durchgeführt werden:

- Validierung ob das Token abgelaufen ist oder zurückgezogen wurde
- Validierung ob der "aud"-Parameter des Tokens übereinstimmt mit dem angefragten Endpunkt
- Validierung ob alle angefragten Restful-Interaktionen abgedeckt sind durch die Scopes innerhalb des Tokens