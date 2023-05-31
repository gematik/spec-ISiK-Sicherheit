
# ISiK Sicherheit und SMART on FHIR

---
### Informativ
---

Die normativen Vorgaben zu ISiK-Sicherheit in der ISiK Stufe 3 fokussieren auf ISiK-Ressourcenserver als bestätigungsrelevante Systeme und bilden damit lediglich eine Untermenge des HL7-Standards _SMART on FHIR_ ab. Für Hersteller von Autorisierungsservern, _SMART Client-Apps_, KIS sowie Patienten- und Zuweiserportalen kann es attraktiv sein, ihre Lösungen über die in ISiK Stufe 3 normativen API-Aufrufe gegenüber ISiK-Ressourcenservern hinaus am SMART-on-FHIR-Standard auszurichten. Auch Krankenhäuser können von den grundlegenden Ideen hinter _SMART on FHIR_ profitieren, z. B. indem funktionale Module von Drittherstellern über einen _Smart App Launch_ einfacher in bestehende Systemlandschaften integriert werden können.

ISiK-Sicherheit in der Stufe 3 unterstützt dieses, indem die Integration der bestätigungsrelevanten Systeme und Schnittstellen in das gesamte SMART-on-FHIR Ökosystem als informative Ergänzung zu ISiK Stufe 3 beschrieben wird. In den zu dieser Seite untergeordneten Seiten werden die kompletten Interaktionen zwischen EHR (KIS, Portal), _SMART Apps_, Autorisierungsserver und Ressourcenservern auf Basis der aktuellen Fassung des SMART-on-FHIR-Standards beschrieben. Diese Beschreibungen sind informativ, da sie aktuell nicht Gegenstand der ISiK-Konformitätsfeststellungen sind und da sie - zumindest in Teilen - auch über den Standard hinausgehende Empfehlungen beinhalten. Um den Lesern dieser Spezifikation die Unterscheidung zwischen normativen und informativen Bestandteilen von ISiK-Sicherheit zu erleichtern, sind alle Seiten mit informativen Inhalten deutlich mit dem Label "Informativ" im Seitenkopf gekennzeichnet.

## _SMART App Launch_
Die auf den nachgeordneten Seiten beschriebene Implementierungsleitfaden dient zur Erläuterung des Ablaufs eines _Smart App Launch_ (siehe [ISik Sicherheit: Autorisierung](ISiKAutorisierung.md)). Ziel des _Smart App Launch_ ist es, ein Zugangstoken von einem OAuth2-kompatiblen Autorisierungsserver zu erhalten, mittels dessen eine FHIR RESTful API-Interaktion durchgeführt werden kann. Dies erfolgt unter Berücksichtigung der Zugriffsrechte der Benutzer in dem den _SMART App Launch_ auslösenden System (KIS, Portal), das im Folgenden analog zur SMART-on-FHIR-Spezifikation als 'EHR' (_Electronic Health Record_) bezeichnet wird. 

Um ein Zugangstoken für den Zugriff auf einen Ressourcenserver zu erhalten, sind folgende sechs Schritte notwendig, die auf den Unterseiten zu dieser Seite jeweils im Detail beschrieben sind:

* Registrierung eines _SMART Clients_ mit dem EHR
* Der _SMART Client_ fragt den Autorisierungsserver des EHR um Autorisierung an
* Der EHR evaluiert die Autorisierungsanfrage und initiiert ggf, die Authentifizierung des (menschlichen) Nutzers
* Austausch des vom EHR an den Client ausgegebenen Autorisierungscodes gegen ein Zugangstoken.
* Ausführung einer durch das Zugangstoken abgesicherten FHIR Restful Interaktion am Ressourcenserver
* Ausstellung eines "Refresh"-Zugangstoken.

Eine Übersicht des zusammenhängenden _SMART App Launch_ ist dem Abschnitt [SMART App Launch - 2.0.3 - SMART authorization & FHIR access: overview](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#smart-authorization--fhir-access-overview) der SMART-on-FHIR-Spezifikation zu entnehmen.

