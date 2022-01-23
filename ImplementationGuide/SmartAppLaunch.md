# Übersicht SMART Client Launch

Die nachfolgende Spezifikation basiert auf dem [HL7 Standard "Smart App Launch - v2.0.0"](https://hl7.org/fhir/smart-app-launch/STU2/index.html). Ziel der Spezifikation ist, bestehende Best Practices zur Autorisierung- und Authentifikation für FHIR-Server wiederzuverwenden. Es werden keine Anpassungen innerhalb dieser Spezifikation veröffentlicht, welche dazu führen würden, dass eine Implementierung nicht mehr konform zum ursprünglichen Standard ist.

Der vorliegende Implementierungsleitfaden dient zur Erläuterung des Ablaufs eines Smart App Launchs, sowie Anmerkungen, welche Teile der Spezifikation bestätigungsrelevant sind.

Ziel des Smart App Launch ist es, ein Zugangstoken von einem OAuth 2.0-kompatiblen Autorisierungsserver zu erhalten mittels dessen eine FHIR Restful API Interaktion durchgeführt werden kann. Dies erfolgt unter Berücksichtigung der Zugriffsrechte der Benutzer*in im bestätigungsrelevanten System. Um ein Zugangstoken zu erhalten sind folgende sechs Schritte notwendig:

1. Registrierung eines SMART Clients mit dem bestätigungsrelevanten System
3. Client bittet um Autorisierung
4. Bestätigungsrelevanten System evaluiert die Autorisierungsanfrage, Authentifizierung der Endnutzer
5. Austausch des Autorisierungscodes für ein Zugangstoken
6. FHIR Restful Interaktion abgesichert durch Zugangstoken
7. Ausstellung eines "Refresh"-Zugangstoken

Eine Übersicht des zusammenhängenden Smart App Launch ist Abschnitt [SMART App Launch - 2.0.3 - SMART authorization & FHIR access: overview](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#smart-authorization--fhir-access-overview) zu entnehmen.

Die weiteren Unterkapitel geben eine Einführung in die jeweiligen Abschnitte des Smart App Launch. Bestätigungsrelevante Inhalte werden als solche markiert mit entsprechenden Verweisen auf die Smart App Launch Spezifikation.

-------

# EHR Launch / Standalone Launch

Eine weitere Differenzierung der Funktionalität eines Smart App Launch erfolgt durch die Einteilung aus welchem Kontext der Client gestartet wird:

- [SMART App Launch - EHR Launch](http://build.fhir.org/ig/HL7/smart-app-launch/app-launch.html#step-2-launch-ehr):
Ein Client kann aus dem Kontext des bestätigungsrelevanten System direkt innerhalb einer bestehenden User Session gestartet werden. Beispielsweise indem die eingeloggte Benutzer*in den Client startet und durch das System eine neue Browser Instanz geöffnet wird oder das System einen iframe darstellt.

- [SMART App Launch - Standalone Launch](http://build.fhir.org/ig/HL7/smart-app-launch/app-launch.html#launch-app-standalone-launch):
Clients welche außerhalb des bestätigungsrelevanten System gestartet werden (z.B. Mobile Apps welche Daten vom bestätigungsrelevanten System abfragen möchten). Es existiert kein gemeinsamer Kontext zwischen bestätigungsrelevanten System und Client.

Bestätigungsrelevante Systeme MÜSSEN einen EHR Launch und einen Standalone Launch unterstützen.

Beschreibung Smart launch sequence, siehe [SMART App Launch - 2.0.7 - Launch App: EHR Launch](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#step-2-launch-ehr).

{{render:smartlaunchsequence}}

-------

<br><br>

Beschreibung Smart authorization sequence, siehe [SMART App Launch - 2.0.9 - Obtain authorization code](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#obtain-authorization-code).

{{render:smartauthorizationsequence}}

-------

<br><br>

Beschreibung Smart retrieval and refresh sequence, siehe [SMART App Launch - 2.0.11 - Access FHIR API](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#access-fhir-api) und [SMART App Launch - 2.0.12 - Access FHIR API](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#refresh-access-token).


{{render:smartretrievalandrefreshsequence}}

-------

# Kategorisierung Clients

Die SMART on FHIR Spezifikation enthält teilweise optionale Anforderungen abhänig davon ob ein Client als "confidential app" oder "public app" klassifiziert wird. Diese Differenzierung erfolgt auf Basis von Kriterien nach [RFC6749 - The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749#section-2.1). Bestätigungsrelevante Systeme MÜSSEN einen SMART App Launch für sowohl "Confidential Apps", als auch "Public Apps" unterstützen. Weitere Details werden durch den Abschnitt [SMART App Launch - 2.0.2.2 - Support for “public” and “confidential” apps](http://build.fhir.org/ig/HL7/smart-app-launch/app-launch.html#support-for-public-and-confidential-apps) definiert.
