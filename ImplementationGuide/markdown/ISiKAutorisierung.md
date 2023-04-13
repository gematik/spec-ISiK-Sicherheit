# Übersicht ISiK Autorisierung

Am Baustein "ISiK-Autorisierung" von ISiK-Sicherheit teilnehmende Systeme können eine oder mehrere der folgenden Rollen einnehmen:
* ISiK-Ressourcenserver: IT-Systeme stellen in dieser Rolle geschützte Ressourcen über ISiK-konforme FHIR-ReST-Endpunkte bereit oder nehmen geschützte Ressourcen über ISiK-konforme FHIR-ReST-Endpunkte entgegen.
* ISiK-Client: IT-Systeme in dieser Rolle fragen geschützte Ressourcen von ISiK-Ressourcenservern ab oder senden geschützte Ressourcen an ISiK-Ressourcenserver.  
* Autorisierungsserver: IT-Systeme in dieser Rolle stellen für authentifizierte Nutzer Sicherheitstoken aus, die einen Zugang zu auf ISiK-Ressourcenservern verwalteten FHIR Ressourcen autorisieren.
* ISiK-App-Launcher: IT-Systeme in dieser Rolle können einen ISiK-Client aus ihrem eigenen Sicherheitskontext initialisieren oder in einen neu aufgesetzten Kontext stellen. Der Kontext kann z. B. eine Benutzer-Session oder einen ausgewählten Patienten darstellen.

Ein IT-System im Krankenhaus kann mehrere der genannten Rollen einnehmen. Beispielsweise kann ein KIS selber Ressourcen bereitstellen ("ISiK-Ressourcenserver"), Ressourcen von spezialisierten Subsystemen abrufen ("ISiK-Client"), die Autorisierung von Nutzern durchführen ("Autorisierungsserver") und weitere Anwendungen aus dem aktuellen Nutzer- und/oder Patientenkontext heraus starten ("ISiK-App-Launcher"). 

## SMART App Launch

ISiK-Autorisierung basiert auf dem [HL7 Standard "Smart App Launch - v2.0.0"](https://hl7.org/fhir/smart-app-launch/STU2/index.html). Ziel ist, für ISiK-Autorisierung die bestehenden Best Practices zur Autorisierung von Zugriffen auf FHIR-Server wiederzuverwenden. Gleichzeitig werden für ISiK Stufe 3 keine normativen Festlegungen getroffen, die andere Profile auf dem OAuth2-Standard wie z. B. IHE IUA grundsätzlich ausschließen. Dies soll es Krankenhäusern erlauben, für die Absicherung gegen externe Zugriffe (z. B. ausgehend von einem Patientenportal) und für die Autorisierung interner Zugriffe (z. B. zur Implementierung der OH KIS) unterschiedliche Standardprodukte einzusetzen, die jeweils auf die spezifischen Anforderungen zugeschnitten sind.

Ziel des Smart App Launch ist es, ein Zugangstoken von einem OAuth 2.0-kompatiblen Autorisierungsserver zu erhalten, mittels dessen ein ISiK-Client eine FHIR Restful API Interaktion gegen einen ISiK-Ressourcenserver durchgeführen kann. Dies erfolgt unter Berücksichtigung der vorab festgelegten Zugriffsrechte der Benutzer, die sich z. B. aus deren Rollen oder anderen Nutzerattributen ableiten können. Um ein Zugangstoken zu erhalten und mit diesem als Autorisierungsnachweis auf einen Ressourcenserver zuzugreifen, sieht der Smart App Launch die in der nachfolgenden Tabelle aufgeführten Schritte vor. Die Kreuze geben an, welche der oben beschriebenen logischen Bausteine an dem jeweiligen Schritt beteiligt sind.

| # | Schritt                                        | ISiK-Ressourceserver | ISiK-Client| Autorisierungsserver | ISiK-App-Launcher |
|:-:|:-----------------------------------------------|:--------------------:|:----------:|:--------------------:|:-----------------:|
| 1 | Aufsetzen Vertrauensbeziehungen                |                      |      X     |            X         |                   |
| 2 | Starten des Clients aus/in einen Launch-Kontext|                      |      X     |           (X)        |         X         |
| 3 | Abruf der SMART-Konfiguration                  |            X         |      X     |            X         |                   |
| 4 | Autorisierungsanfrage des Clients              |                      |      X     |            X         |                   |
| 5 | Prüfung Anfrage und Ausgabe Autorisierungscode |                      |      X     |            X         |                   |
| 6 | Austausch des Codes gegen ein Zugangstoken     |                      |      X     |            X         |                   |
| 7 | Autorisierte ReST-Interaktion                  |            X         |      X     |           (X)        |                   |
|(8)| Neu-Ausstellen eines Zugangstokens             |                      |      X     |            X         |                   |

Eine Übersicht des zusammenhängenden Smart App Launch ist dem Abschnitt [SMART App Launch - 2.0.3 - SMART authorization & FHIR access: overview](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#smart-authorization--fhir-access-overview) zu entnehmen.

-------

## ISiK Autorisierung in ISiK Stufe-3

Wie aus der Tabelle zu ersehen, betrifft das Gros der SMART-on-FHIR-Spezifikation das Zusammenspiel der Systeme in den Rolle des ISiK-Clients und des Autorisierungsservers. Dieses wird zusätzlich stark geprägt durch den Kontext aus welchem bzw. in welchen der Client gestartet wird:

- [SMART App Launch - EHR Launch](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#step-2-launch-ehr):
Ein Client kann aus dem Kontext eines anderen Systems (das dann in der Rolle des ISiK-App-Launchers agiert) direkt innerhalb einer bestehenden User Session gestartet werden, beispielsweise indem die eingeloggte Benutzer den Client startet und durch das System eine neue Browserinstanz geöffnet wird oder das System einen iframe darstellt.

- [SMART App Launch - Standalone Launch](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#launch-app-standalone-launch):
Clients, welche außerhalb eines anderen Systems gestartet werden (z.B. Mobile Apps welche Daten von einem ISiK-Ressourcenserver abfragen möchten), müssen ihren Kontext explizit über eine Nutzerinteraktion gegen eine als ISiK-App-Launcher fungierende Komponente aufbauen. Dieses kann beispielsweise ein Widget zur Auswahl des zu aktivierenden Patientenkontextes sein (siehe z. B. "patient context picker" in der [Open Source Keycloak Erweiterung 'Alvearie'](https://github.com/Alvearie/keycloak-extensions-for-fhir)).

Beide Launch-Sequenzen bedingen eine weitreichende Integration von Autorisierungsserver und App-Laucher, die gemeinsam den Launch-Kontext des ISiK-Clients bestimmmen und verwalten. Für den Standalone Launch muss zusätzlich ein interaktiver "context picker" integriert oder angebunden werden. Diese enge Bindung steht im Widerspruch zu den [Vorgaben für ISiK-Sicherheit in ISiK Stufe 3](Motivation.html) und erschwert die Nutzung von marktgängigen Standard-Produkten wie z. B. Keycloak, Auth0 und/oder NGINX zum Aufbau eines IAM-Subsystems. Daher werden in ISiK Stufe 3 im Modul ISiK-Sicherheit zunächst nur Interaktionen gegen Systeme in der Rolle eines ISiK-Ressourcenservers definiert. Die Vorgaben umfassen dabei insbesondere die SMART-Konfiguration (Schritt 3) sowie die Syntax und Semantik der über das Zugangstoken vermittelten Autorisierungen (Schritt 7). 

-------

# Zugriffsrechte und Compartments

ISiK-Autorisierung schreibt Mechanismen für de Austausch und die Kodierung von Autorisierungen fest. Die Autorisierungen selbst instanziieren im Krankenhaus vergebene Berechtigungen für einen Zugriff einer Person auf eine geschützte Ressource (z.B. "Der zugreifende Nutzer darf Observation-Ressourcen des Patienten mit der ID 123 suchen und abrufen."). Autorisierungen werden in dem ISiK-Sicherheit zugrunde liegenden Bild einer IT-Sicherheitsinfrastruktur durch einen Autorisierungsserver im Ergebnis der Prüfung festgelegter Berechtigungen vergeben. Diese Berechtigungen wiederum leiten sich aus Sicherheitspolitiken des Krankenhauses, Rollendefinitionen, durch Patienten gegebene Einwilligungen und weiteren Vorgaben ab. Im ISiK zugrundeliegenden Bild erfolgt die Verwaltung von Berechtigungen über einen Policy Administration Point.  

ISiK-Autorisierung sieht für die Kodierung von Autorisierungen zwei miteinander kombinierbare Konzepte aus dem SMART-on-FHIR-Standard vor:
* Compartments (Launch Kontexte)
* an FHIR-Ressourcen gebundene Zugriffsrechte

## Compartments

Autorisierungen sollen oft an eine 'Fokus'-Ressource gebunden werden, z. B. eine Patient-Ressource ("Zugriff auf Daten zum Patienten 123") oder eine Encounter-Ressource ("Zugriff auf Daten zum Behandlungsfall 456"). Um die 'Fokus'-Ressource herum gruppieren sich weitere Ressourcen, die mit dieser in einer Beziehung stehen, z. B. im Fall der Patient-Ressource die dem Patienten zugeordneten Beobachtungen, Diagosen/Probleme, Termine, Behandlungspläne, etc. In FHIR werden diese Gruppierungen über die Ressource CompartmentDefinition festgelegt. Diese definiert die Elemente einer Ressource, die die Bindung zu der 'Fokus'-Ressource herstellen. 

Beispiel: Für die Ressource Condition legt die CompartmentDefinition der Patient-Ressource die Elemente Condition.patient und Condition.participant-actor als verbindende Elemente fest. Eine Autorisierung für den Zugriff auf Daten des Patienten 123 umfasst damit grundsätzlich nur Condition-Ressourcen, deren subject- oder participant-actor-Element auf den Patienten 123 verweist.  

ISiK Autorisierung in der ISiK Stufe 3 verlangt von FHIR-Ressourcenservern, dass diese zumindest Autorisierungen mit Bezug zu Compartment-Definitionen der Ressourcen Patient und Encounter verarbeiten können.

## Zugriffsrechte auf Ressourcen


