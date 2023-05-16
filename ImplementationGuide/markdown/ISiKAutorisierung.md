# Übersicht ISiK-Sicherheit

---
### Normativ
---

Am Baustein Autorisierung von ISiK-Sicherheit teilnehmende Systeme können eine oder mehrere der folgenden Rollen einnehmen:
* ISiK-Ressourcenserver: IT-Systeme stellen in dieser Rolle geschützte Ressourcen über ISiK-konforme FHIR-ReST-Endpunkte bereit oder nehmen geschützte Ressourcen über ISiK-konforme FHIR-ReST-Endpunkte entgegen.
* ISiK-Client: IT-Systeme in dieser Rolle fragen geschützte Ressourcen von ISiK-Ressourcenservern ab oder senden geschützte Ressourcen an ISiK-Ressourcenserver.  
* Autorisierungsserver: IT-Systeme in dieser Rolle stellen für authentifizierte Nutzer Sicherheitstoken aus, die einen Zugang zu auf ISiK-Ressourcenservern verwalteten FHIR Ressourcen autorisieren.
* ISiK-App-Launcher: IT-Systeme in dieser Rolle können einen ISiK-Client aus ihrem eigenen Sicherheitskontext initialisieren oder in einen neu aufgesetzten Kontext stellen. Der Kontext kann z. B. der aktuell ausgewählte Patient oder Behandlungsfall sein.

Ein IT-System im Krankenhaus kann mehrere der genannten Rollen einnehmen. Beispielsweise kann ein KIS selbst Ressourcen bereitstellen ("ISiK-Ressourcenserver"), Ressourcen von spezialisierten Subsystemen abrufen ("ISiK-Client"), die Autorisierung von Nutzern durchführen ("Autorisierungsserver") und weitere Anwendungen aus dem aktuellen Nutzer- und/oder Patientenkontext heraus starten ("ISiK-App-Launcher"). Durch eine solche Integration verschiedener Rollen kann ein KIS im Kontext der vorliegenden Vorgaben zu "ISiK Sicherheit" einen _Electronic Health Record_ im Sinne von _SMART on FHIR_ darstellen und damit als Plattform für wiederverwendbare, austauschbare und dynamisch startbare Anwendungen (_SMART Apps_) fungieren. 

## SMART App Launch

Ziel von ISiK-Sicherheit Teil: Autorisierung (im folgenden ISiK-Sicherheit) ist, bestehende Best Practices zur Autorisierung von Zugriffen auf FHIR-Server aufzugreifen und - unter Berücksichtigung der Weiterentwicklung der Telematikinfrastruktur - für die Gegebenheiten deutscher Krankenhäuser zu profilieren. 

ISiK-Sicherheit soll durch eine wie oben skizzierte Integration verschiedener Systemrollen eine Umsetzung des als Teil des SMART-on-FHIR-API definierten [HL7 Implementierungsleitfadens _Smart App Launch - v2.0.0_](https://hl7.org/fhir/smart-app-launch/STU2/index.html) ermöglichen. Für ISiK Stufe 3 werden jedoch keine normativen Festlegungen getroffen, die andere Profile auf dem _OAuth2_-Standard wie z. B. _IHE IUA_ grundsätzlich ausschließen. Dies soll es Krankenhäusern erlauben, für die Absicherung gegen externe Zugriffe (z. B. ausgehend von einem Patientenportal) und für die Autorisierung interner Zugriffe (z. B. zur Implementierung der OH KIS) unterschiedliche Standardprodukte einzusetzen, die jeweils auf die spezifischen Anforderungen zugeschnitten sind.

Ziel des _Smart App Launch_ ist es, ein Zugriffstoken von einem OAuth2-kompatiblen Autorisierungsserver zu erhalten, mittels dessen ein ISiK-Client eine FHIR RESTful API-Interaktion gegen einen ISiK-Ressourcenserver durchführen kann. Dies erfolgt unter Berücksichtigung der vorab festgelegten Zugriffsrechte der Benutzer, die sich z. B. aus deren Rollen oder anderen Nutzerattributen ableiten können. Um ein Zugriffstoken zu erhalten und mit diesem als Autorisierungsnachweis auf einen Ressourcenserver zuzugreifen, sieht der _Smart App Launch_ die in der nachfolgenden Tabelle aufgeführten Schritte vor. Die Kreuze geben an, welche der oben beschriebenen logischen Bausteine an dem jeweiligen Schritt beteiligt sind.

| # | Schritt                                        | ISiK-Ressourceserver | ISiK-Client| Autorisierungsserver | ISiK-App-Launcher |
|:-:|:-----------------------------------------------|:--------------------:|:----------:|:--------------------:|:-----------------:|
| 1 | Aufsetzen Vertrauensbeziehungen                |     x                 |      X     |      X         |                   |
| 2 | Starten des Clients aus/in einen Launch-Kontext|                      |      X     |           (X)        |         X         |
| 3 | Abruf der SMART-Konfiguration                  |            X         |      X     |           |                   |
| 4a | Autorisierungsanfrage des Clients              |                      |      X     |            X         |                   |
| 4b | Prüfung Anfrage und Ausgabe Autorisierungscode |                      |      X     |            X         |                   |
| 5 | Austausch des Codes gegen ein Zugriffstoken    |                      |      X     |            X         |                   |
| 6 | Autorisierte ReST-Interaktion                  |            X         |      X     |           (X)        |                   |
|(7)| Neu-Ausstellen eines Zugriffstokens            |                      |      X     |            X         |                   |

Eine Übersicht des zusammenhängenden _SMART App Launch_ ist dem Abschnitt [SMART App Launch - 2.0.3 - SMART authorization & FHIR access: overview](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#smart-authorization--fhir-access-overview) zu entnehmen.

-------

## ISiK-Sicherheit Teil: Autorisierung in ISiK Stufe 3

Wie aus der Tabelle zu ersehen, betrifft das Gros der SMART-on-FHIR-Spezifikation das Zusammenspiel der Systeme in den Rollen des ISiK-Clients und des Autorisierungsservers. Dieses wird zusätzlich stark geprägt durch den Kontext aus welchem bzw. in welchen der Client - d. h. die wiederverwendbare, austauschbare, modulare Anwendung - dynamisch gestartet wird:

- [SMART App Launch - EHR Launch](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#step-2-launch-ehr):
Ein Client kann aus dem Kontext eines anderen Systems (das dann in der Rolle des ISiK-App-Launchers agiert) direkt innerhalb einer bestehenden User Session gestartet werden, beispielsweise indem die eingeloggte Benutzer den Client startet und durch das System eine neue Browserinstanz geöffnet wird oder das System einen _iframe_ darstellt.

- [SMART App Launch - Standalone Launch](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#launch-app-standalone-launch):
Clients, welche außerhalb eines anderen Systems gestartet werden (z.B. Mobile Apps welche Daten von einem ISiK-Ressourcenserver abfragen möchten), müssen ihren Kontext explizit über eine Nutzerinteraktion gegen eine als ISiK-App-Launcher fungierende Komponente aufbauen. Dieses kann beispielsweise ein Widget zur Auswahl des zu aktivierenden Patientenkontextes sein (siehe z. B. "patient context picker" in der [Open Source Keycloak-Erweiterung _Alvearie_](https://github.com/Alvearie/keycloak-extensions-for-fhir)).

Beide Launch-Sequenzen bedingen eine weitreichende Integration von Autorisierungsserver und App-Laucher, die gemeinsam den Launch-Kontext des ISiK-Clients bestimmmen und verwalten. Diese enge Bindung steht im Widerspruch zu den formulierten [Vorgaben für ISiK-Sicherheit in ISiK Stufe 3](Motivation.md) und erschwert die Nutzung von marktgängigen Produkten oder open Source bibliotheken zum Aufbau eines IAM-Subsystems. Daher werden in ISiK Stufe 3 im Modul ISiK-Sicherheit zunächst nur Interaktionen gegen Systeme in der Rolle eines ISiK-Ressourcenservers definiert. Die Vorgaben umfassen dabei insbesondere die SMART-Konfiguration (Schritt 3) sowie die Syntax und Semantik der über das Zugriffstoken vermittelten Autorisierungen (Schritt 6). 

-------

# Zugriffsrechte und Compartments

ISiK-Sicherheit schreibt Mechanismen für den Austausch und die Kodierung von Autorisierungen fest. Die Autorisierungen selbst instanziieren im Krankenhaus vergebene Berechtigungen für einen Zugriff einer Person auf eine geschützte Ressource (z.B. "Der zugreifende Nutzer darf Observation-Ressourcen des Patienten mit der ID 123 suchen und abrufen."). Autorisierungen werden in dem ISiK-Sicherheit zugrunde liegenden Bild einer IT-Sicherheitsinfrastruktur durch einen Autorisierungsserver im Ergebnis der Prüfung festgelegter Berechtigungen vergeben. Diese Berechtigungen wiederum leiten sich aus Sicherheitspolitiken des Krankenhauses, Rollendefinitionen, durch Patienten gegebene Einwilligungen und weiteren Vorgaben ab. Im ISiK zugrundeliegenden Bild erfolgt die Verwaltung von Berechtigungen über einen _Policy Administration Point_.  

ISiK-Sicherheit nutzt für die Kodierung und Durchsetzung von Autorisierungen drei Konzepte aus FHIR bzw. SMART-on-FHIR:
* Kontext
* Compartment 
* Scope

 Scope und Compartements sind Teil eines Kontexts und können zueinander in Bezug stehen.

## Kontexte

Jeder Zugriff auf eine geschützte Ressource erfolgt im Kontext eines Patienten, eines Behandlungsfalls oder einer anderen Ressource. Der Kontext wird vom aufrufenden Client "mitgebracht" und stellt den Bezugspunkt für alle anderen Berechtigungsinformationen dar. Aus Sicht des Client stellt dieser Kontext den "aktuellen Patienten", den "aktuellen Fall", etc. dar.

Beispiel: Der Nutzer hat in/aus der ISiK-Clientanwendung den Patient "123" geöffnet und möchte nun Daten zu diesem Patienten verarbeiten, zu deren Abruf eine Autorisierung erforderlich ist. Der Zugriffskontext der Autorisierung ist der Patient "123". Alle anwendbaren Zugriffsrechte (s.u.) beziehen sich auf den Patienten "123". Das _Patient_-Compartment beschreibt, wie der Ressourcenserver validieren kann, dass eine Ressource (z. B. eine _Observation_) im Kontext des Patient "123" steht.

ISiK-Sicherheit macht in der ISiK Stufe 3 keine Vorgabe, wie ein ISiK-Client in einen bestimmten Kontext gestellt wird (SMART-on-FHIR sieht hierfür z. B. die oben skizzierten Mechanismen eines _EHR Launch_ bzw. eines _Standalone Launch_ vor, bei dem ein Kontext als _Launch Context_ an eine andere Anwendung übergeben/vererbt werden und kann dabei weiter eingeschränkt werden kann). Es wird jedoch verlangt, dass eine Kontextinformation als Teil des Zugriffstokens (s.u.) beim Aufruf eineS RESTful API eines ISiK-Ressourcenservers übergeben wird und dass der Ressourcenserver alle im Zugriffstoken kodierten Autorisierungsinformationen in diesem Kontext anwendet.

## Compartments

Autorisierungen können in FHIR an eine 'Fokus'-Ressource gebunden werden, z. B. eine Patient-Ressource ("Zugriff auf Daten zum Patienten 123"). Um die 'Fokus'-Ressource herum gruppieren sich weitere Ressourcen, die mit dieser in einer Beziehung stehen, z. B. im Fall der Patient-Ressource die dem Patienten zugeordneten Beobachtungen, Diagosen/Probleme, Termine, Behandlungspläne, etc. In FHIR werden diese Gruppierungen über Ressourcen vom Ressourcentyp [_CompartmentDefinition_](http://hl7.org/fhir/compartmentdefinition.html) festgelegt. Diese definiert die Elemente einer Ressource, die die Bindung zu der 'Fokus'-Ressource herstellen. 

Beispiel: Für den Ressourcentyp [_Condition_](http://hl7.org/fhir/condition.html) legt die [_CompartmentDefinition_ der _Patient_-Ressource](http://hl7.org/fhir/compartmentdefinition-patient.html) die Elemente 'Condition.patient' und 'Condition.participant-actor' als verbindende Elemente fest. Eine Autorisierung für den Zugriff auf Patientendaten im Kontext des Patienten "123" umfasst damit grundsätzlich nur _Condition_-Ressourcen, deren 'subject'- oder 'participant-actor'-Element auf den Patienten "123" verweist.  

ISiK-Sicherheit in der ISiK Stufe 3 verlangt von FHIR-Ressourcenservern, dass diese zumindest Autorisierungen mit Bezug zu der in FHIR definierten _Compartment_-Definition für patientenbezogene Daten verarbeiten können.

## Zugriffsrechte auf Ressourcen

In dem von SMART-on-FHIR profilierten _OAuth2_-Standard legen sog. _Scopes_ die spezifischen Aktionen und/oder Daten fest, auf die eine Client-Anwendung in Vertretung eines Benutzers zugreifen bzw. diese manipulieren kann. Z.B. kann ein _Scope_ die Benutzerberechtigung zum Zugriff auf _Observation_-Ressourcen an einen ISiK-Client delegieren und diesem damit die Möglichkeit geben, _Observation_-Ressourcen zu einem Patienten von einem ISiK-Ressourcenserver abzurufen.

_Scopes_ werden vom API-Anbieter - im Fall von ISiK dem ISiK-Ressourcenserver - definiert und von dem zugreifenden ISiK-Client während des Autorisierungsprozesses über den Autorisierungsserver angefordert. Sofern die Auswertung der anwendbaren Berechtigungsregeln durch den Autorisierungsserver die angeforderten _Scopes_ bestätigt, wird dem ISiK-Client ein Zugriffstoken (_Access Token_) ausgestellt, das die anwendbaren _Scopes_ enthält. Der ISiK-Client kann anschließend das Zugriffstoken verwenden, um im Rahmen der durch die _Scopes_ bestätigten Rechtedelegation für den berechtigten Nutzer auf die geschützten Ressourcen eines ISiK-Ressourcenservers zuzugreifen. Hierbei muss der _ISiK-Ressourceserver_ die Umsetzung der Scopes gewähten bzw. umsetzen können.

ISiK-Sicherheit in der ISiK Stufe 3 verlangt von FHIR-Ressourcenservern (ggf. im Zusammenspiel mit vorgelagerten _API Gateways_ oder _Reverse Proxies_), dass diese die in SMART-on-FHIR definierte Syntax für die Bestätigung von _Scopes_ verarbeiten können und die mit den _Scopes_ ausgedrücken Berechtigungsdelegationen zur Absicherung ihrer ReST-Schnittstellen anwenden können.

## Zusammenspiel von Kontexten, Compartments und Zugriffsrechten auf Ressourcen

_Compartments_ grenzen ab, auf welche mit einer 'Fokus'-Ressource gruppierten Ressourcen ein ISiK-Client überhaupt zugreifen kann. Welche Ressource konkret den 'Fokus' darstellt, wird über den Kontext bestimmt. Über _Scopes_ bestätigte Zugriffsrechte grenzen ein, auf welchen der mit der 'Fokus'-Ressource gruppierten Ressourcen der ISiK-Client welche Operationen ausführen darf. 

Beispiel: Der aus Sicht des ISiK-Clients aktuelle Patient ist der Patient "123". Dieser ist hiermit auch der Kontext der Autorisierung. Die Kombination aus der _CompartmentDefinition_ für die _Patient_-Ressource und dem Zugriffsrecht 'patient/Observation.r' legt fest, dass der ISiK-Client nur lesend und nur auf _Observation_-Ressourcen zugreifen darf, die über 'Observation.subject' oder 'Observation.performer' dem Patienten "123" zugeordnet sind.


## Zugriffstoken: Beispiel

Zugriffstoken werden als Base64-kodierte _JSON Web Token (JWT)_ ausgetauscht. 

Beispiel:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRpZW50IjoiODdhMzM5ZDAtOGNhZS00MThlLTg5YzctODY1MWU2YWFiM2M2IiwidG9rZW5fdHlwZSI6ImJlYXJlciIsInNjb3BlIjoicGF0aWVudC9PYnNlcnZhdGlvbi5ycyBwYXRpZW50L1BhdGllbnQucnMiLCJjbGllbnRfaWQiOiJraHpnX3BvcnRhbCIsImlhdCI6MTY4MTQ1OTIwMCwiZXhwIjoxNjgxNDU5ODAwfQ.NyA2LO9u17mZRXz4yP6uUvibuhpjVo5uLslXo2U4DOA
```
Nach der Dekodierung werden die im Token gekapselten Autorisierungsinformationen erkennbar:

```
{
  "alg": "HS256",
  "typ": "JWT"
}
{
  "patient": "87a339d0-8cae-418e-89c7-8651e6aab3c6",
  "token_type": "bearer",
  "scope": "patient/Observation.rs patient/Patient.rs",
  "client_id": "khzg_portal",
  "iat": 1681459200,
  "exp": 1681459800
}
```

Das Zugriffstoken in dem Beispiel gewährt Lese- und Such-Zugriffe auf die Patient-Ressource und Observation-Ressourcen des Patienten mit der _id_ '87a339d0-8cae-418e-89c7-8651e6aab3c6'. Das Token wurde am 14. April 2023 10:00 Uhr (Mitteleuropäische Sommerzeit) ausgestellt und ist ab diesem Zeitpunkt 10 Minuten lang gültig.