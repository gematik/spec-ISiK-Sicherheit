# Schritt 5: FHIR Restful Interaktion

Das erhaltene Access Token kann durch den Client am FHIR-Endpunkt des bestätigungsrelevanten System eingelöst werden. Hierzu ist bei jeder Restful-Interaktion ein Authorization-Header mitzusenden. Das Token ist per [RFC6750 -  The OAuth 2.0 Authorization Framework: Bearer Token Usage](https://datatracker.ietf.org/doc/html/rfc6750) zu kodieren.

Bestätigungsrelevant sind die Anforderungen nach [SMART App Launch - 2.0.11 - Access FHIR API](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#access-fhir-api). Unter anderem MÜSSEN folgende Validierungschritte seitens des bestätigungsrelevanten Systems durchgeführt werden:

- Validierung ob das Token abgelaufen ist oder zurückgezogen wurde
- Validierung ob der "aud"-Parameter des Tokens übereinstimmt mit dem angefragten Endpunkt
- Validierung ob alle angefragten Restful-Interaktionen abgedeckt sind durch die Scopes innerhalb des Tokens

## Break The Glass

In klinischen Notfällen kann es erforderlich sein, dass eine Benutzer*in auf die Daten des bestätigungsrelevanten Systems zugreifen muss, ohne entsprechende Rechte hierfür zu besitzen, um die Sicherheit der PatientInnen zu gewährleisten. Die vorliegende Spezifikation macht diesbezüglich keine Vorgaben und verweist auf die [FHIR Kernspezifikation - Break The Glass](https://www.hl7.org/fhir/security-labels.html#break-the-glass). Eine grundlegende Betrachtung der resultierenden Problemstellung findet sich in ["Healthcare Requirements for Emergency Access"](http://www.hl7.org/search/viewSearchResult.cfm?search_id=393442&search_result_url=%2Fdocumentcenter%2Fpublic%2Fwg%2Fsecure%2FHL7%20Emergency%20Access%2Edoc).

----

## Beispiel