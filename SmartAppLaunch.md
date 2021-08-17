# Übersicht SMART App Launch

Die nachfolgende Spezifikation basiert auf dem [HL7 Standard "Smart App Launch - v1.1"](http://build.fhir.org/ig/HL7/smart-app-launch/index.html). Ziel der Spezifikation ist bestehende Best Practices zur Authorisierung- und Authentifikation für FHIR-Server wiederzuverwenden. Es werden keine Anpassungen innerhalb dieser Spezifikation veröffentlicht, welche dazu führen würden, dass eine Implementierung nicht mehr konform zum ursprünglichen Standard ist.

Der vorliegende ImplementationGuide dient zur Erläuterung des Ablaufs eines Smart App Launchs, sowie Anmerkungen welche Teile der Spezifikation bestätigungsrelevant sind.

Ziel des Smart App Launch ist es ein Zugangstoken von einem OAuth 2.0-kompatiblen Authorisierungsserver zu erhalten mittels dessen eine FHIR Restful API Interaktion durchgeführt werden kann. Um ein Zugangstoken zu erhalten sind folgende sechs Schritte notwendig:

1. Registierung einer SMART APP mit dem bestätigungsrelevanten System
1. App bittet um Autorisierung
1. Bestätigungsrelevanten System evaluiert die Autorisierungsanfrage, Authentifizierung der Endnutzer
1. Austausch des Autorisierungscodes für ein Zugangstoken
1. FHIR Restful Interaktion abgesichert durch Zugangstoken
1. Austellung eines "Refresh"-Zugangstoken

Die weiteren Unterkapitel geben eine Einführung in die jeweiligen Abschnitte des Smart App Launch. Bestätigungsrelevante Inhalte werden als solche markiert mit entsprechenden Verweisen auf die Hauptspezifikation.