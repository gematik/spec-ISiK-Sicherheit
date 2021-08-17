# Übersicht SMART App Launch

Die nachfolgende Spezifikation basiert auf dem [HL7 Standard "Smart App Launch - v1.1"](http://build.fhir.org/ig/HL7/smart-app-launch/index.html). Ziel der Spezifikation ist bestehende Best Practices zur Authorisierung- und Authentifikation für FHIR-Server wiederzuverwenden. Es werden keine Anpassungen innerhalb dieser Spezifikation veröffentlicht, welche dazu führen würden, dass eine Implementierung nicht mehr konform zum ursprünglichen Standard ist.

Der vorliegende ImplementationGuide dient zur Erläuterung des Ablaufs eines Smart App Launchs, sowie Anmerkungen, welche Teile der Spezifikation bestätigungsrelevant sind.

Ziel des Smart App Launch ist es, ein Zugangstoken von einem OAuth 2.0-kompatiblen Authorisierungsserver zu erhalten mittels dessen eine FHIR Restful API Interaktion durchgeführt werden kann. Um ein Zugangstoken zu erhalten sind folgende sechs Schritte notwendig:

1. Registrierung einer SMART App mit dem bestätigungsrelevanten System
1. App bittet um Autorisierung
1. Bestätigungsrelevanten System evaluiert die Autorisierungsanfrage, Authentifizierung der Endnutzer
1. Austausch des Autorisierungscodes für ein Zugangstoken
1. FHIR Restful Interaktion abgesichert durch Zugangstoken
1. Austellung eines "Refresh"-Zugangstoken

Die weiteren Unterkapitel geben eine Einführung in die jeweiligen Abschnitte des Smart App Launch. Bestätigungsrelevante Inhalte werden als solche markiert mit entsprechenden Verweisen auf die Smart App Launch Spezifikation.

# Kategorisierung Clients

Die SMART on FHIR Spezifikation enthält teilweise optionale Anforderungen abhänig davon ob ein Client als "confidential app" oder "public app" klassifiziert wird. Diese Differenzierung erfolgt auf Basis von Kriterien nach [RFC6749 - The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749#section-2.1). Bestätigungsrelevante Systeme MÜSSEN einen SMART App Launch für sowohl "Confidential Apps", als auch "Public Apps" unterstützen. Weitere Details werden durch den Abschnitt ["1.0.2.2 Support for “public” and “confidential” apps"](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#support-for-public-and-confidential-apps) definiert.

# EHR Launch / Standalone Launch

Eine weitere Differenzierung der Funktionalität eines Smart App Launch erfolgt durch die Einteilung in eine "EHR launch sequence" und "Standalone launch sequence"

- EHR Launch:
Ein Client kann aus dem Kontext des bestätigungsrelevanten System direkt innerhalb einer bestehenden User Session gestartet werden. Beispielsweise indem der eingeloggte Benutzer den Client startet und durch das System eine neue Browser Instanz geöffnet wird oder das System einen iframe darstellt.

- Standalone Launch:
Clients welche außerhalb des bestätigungsrelevanten System gestartet werden (z.B. Mobile Apps welche Daten vom bestätigungsrelevanten System abfragen möchten). Es besteht kein bestehender gemeinsamer Kontext.

# SMART “launch sequence”