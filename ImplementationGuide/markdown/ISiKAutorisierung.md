# Übersicht ISiK Autorisierung

Am Baustein "ISiK-Autorisierung" von ISiK-Sicherheit teilnehmende Systeme können eine oder mehrere der folgenden Rollen einnehmen:
* ISiK-Ressourcenserver: IT-Systeme stellen in dieser Rolle geschützte Ressourcen über ISiK-konforme FHIR-ReST-Endpunkte bereit oder nehmen geschützte Ressourcen über ISiK-konforme FHIR-ReST-Endpunkte entgegen.
* ISiK-Client: IT-Systeme in dieser Rolle fragen geschützte Ressourcen von ISiK-Ressourcenservern ab oder senden geschützte Ressourcen an ISiK-Ressourcenserver.  
* Autorisierungsserver: IT-Systeme in dieser Rolle stellen für authentifizierte Nutzer Sicherheitstoken aus, die einen Zugang zu auf ISiK-Ressourcenservern verwalteten FHIR Ressourcen autorisieren.
* ISiK-App-Launcher: IT-Systeme in dieser Rolle können einen ISiK-Client aus ihrem eigenen Sicherheitskontext initialisieren oder in einen neu aufgesetzten Kontext stellen. Der Kontext kann z. B. der aktuell ausgewählte Patienten oder Behandlungsfall sein.

Ein IT-System im Krankenhaus kann mehrere der genannten Rollen einnehmen. Beispielsweise kann ein KIS selber Ressourcen bereitstellen ("ISiK-Ressourcenserver"), Ressourcen von spezialisierten Subsystemen abrufen ("ISiK-Client"), die Autorisierung von Nutzern durchführen ("Autorisierungsserver") und weitere Anwendungen aus dem aktuellen Nutzer- und/oder Patientenkontext heraus starten ("ISiK-App-Launcher"). 

## SMART App Launch

Ziel von ISiK-Autorisierung ist, bestehende Best Practices zur Autorisierung von Zugriffen auf FHIR-Server aufzugreifen und - unter Berücksichtigung der Weiterentwicklung der Telematikinfrastruktur - für die Gegebenheiten deutscher Krankenhäuser zu profilieren. 

ISiK-Autorisierung basiert auf dem [HL7 Standard _Smart App Launch - v2.0.0_](https://hl7.org/fhir/smart-app-launch/STU2/index.html). Für ISiK Stufe 3 werden jedoch keine normativen Festlegungen getroffen, die andere Profile auf dem _OAuth2_-Standard wie z. B. _IHE IUA_ grundsätzlich ausschließen. Dies soll es Krankenhäusern erlauben, für die Absicherung gegen externe Zugriffe (z. B. ausgehend von einem Patientenportal) und für die Autorisierung interner Zugriffe (z. B. zur Implementierung der OH KIS) unterschiedliche Standardprodukte einzusetzen, die jeweils auf die spezifischen Anforderungen zugeschnitten sind.

Ziel des _Smart App Launch_ ist es, ein Zugriffstoken von einem OAuth2-kompatiblen Autorisierungsserver zu erhalten, mittels dessen ein ISiK-Client eine FHIR Restful API-Interaktion gegen einen ISiK-Ressourcenserver durchgeführen kann. Dies erfolgt unter Berücksichtigung der vorab festgelegten Zugriffsrechte der Benutzer, die sich z. B. aus deren Rollen oder anderen Nutzerattributen ableiten können. Um ein Zugriffstoken zu erhalten und mit diesem als Autorisierungsnachweis auf einen Ressourcenserver zuzugreifen, sieht der _Smart App Launch_ die in der nachfolgenden Tabelle aufgeführten Schritte vor. Die Kreuze geben an, welche der oben beschriebenen logischen Bausteine an dem jeweiligen Schritt beteiligt sind.

| # | Schritt                                        | ISiK-Ressourceserver | ISiK-Client| Autorisierungsserver | ISiK-App-Launcher |
|:-:|:-----------------------------------------------|:--------------------:|:----------:|:--------------------:|:-----------------:|
| 1 | Aufsetzen Vertrauensbeziehungen                |                      |      X     |            X         |                   |
| 2 | Starten des Clients aus/in einen Launch-Kontext|                      |      X     |           (X)        |         X         |
| 3 | Abruf der SMART-Konfiguration                  |            X         |      X     |            X         |                   |
| 4 | Autorisierungsanfrage des Clients              |                      |      X     |            X         |                   |
| 5 | Prüfung Anfrage und Ausgabe Autorisierungscode |                      |      X     |            X         |                   |
| 6 | Austausch des Codes gegen ein Zugriffstoken    |                      |      X     |            X         |                   |
| 7 | Autorisierte ReST-Interaktion                  |            X         |      X     |           (X)        |                   |
|(8)| Neu-Ausstellen eines Zugriffstokens            |                      |      X     |            X         |                   |

Eine Übersicht des zusammenhängenden Smart App Launch ist dem Abschnitt [SMART App Launch - 2.0.3 - SMART authorization & FHIR access: overview](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#smart-authorization--fhir-access-overview) zu entnehmen.

-------

## ISiK Autorisierung in ISiK Stufe-3

Wie aus der Tabelle zu ersehen, betrifft das Gros der SMART-on-FHIR-Spezifikation das Zusammenspiel der Systeme in den Rolle des ISiK-Clients und des Autorisierungsservers. Dieses wird zusätzlich stark geprägt durch den Kontext aus welchem bzw. in welchen der Client gestartet wird:

- [SMART App Launch - EHR Launch](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#step-2-launch-ehr):
Ein Client kann aus dem Kontext eines anderen Systems (das dann in der Rolle des ISiK-App-Launchers agiert) direkt innerhalb einer bestehenden User Session gestartet werden, beispielsweise indem die eingeloggte Benutzer den Client startet und durch das System eine neue Browserinstanz geöffnet wird oder das System einen _iframe_ darstellt.

- [SMART App Launch - Standalone Launch](https://hl7.org/fhir/smart-app-launch/STU2/app-launch.html#launch-app-standalone-launch):
Clients, welche außerhalb eines anderen Systems gestartet werden (z.B. Mobile Apps welche Daten von einem ISiK-Ressourcenserver abfragen möchten), müssen ihren Kontext explizit über eine Nutzerinteraktion gegen eine als ISiK-App-Launcher fungierende Komponente aufbauen. Dieses kann beispielsweise ein Widget zur Auswahl des zu aktivierenden Patientenkontextes sein (siehe z. B. "patient context picker" in der [Open Source Keycloak-Erweiterung _Alvearie_](https://github.com/Alvearie/keycloak-extensions-for-fhir)).

Beide Launch-Sequenzen bedingen eine weitreichende Integration von Autorisierungsserver und App-Laucher, die gemeinsam den Launch-Kontext des ISiK-Clients bestimmmen und verwalten. Für den _Standalone Launch_ muss zusätzlich ein interaktiver "context picker" integriert oder angebunden werden. Diese enge Bindung steht im Widerspruch zu den [Vorgaben für ISiK-Sicherheit in ISiK Stufe 3](Motivation.md) und erschwert die Nutzung von marktgängigen Standard-Produkten wie z. B. _Keycloak_, _Auth0_ und/oder _NGINX_ zum Aufbau eines IAM-Subsystems. Daher werden in ISiK Stufe 3 im Modul ISiK-Sicherheit zunächst nur Interaktionen gegen Systeme in der Rolle eines ISiK-Ressourcenservers definiert. Die Vorgaben umfassen dabei insbesondere die SMART-Konfiguration (Schritt 3) sowie die Syntax und Semantik der über das Zugriffstoken vermittelten Autorisierungen (Schritt 7). 

-------

# Zugriffsrechte und Compartments

ISiK-Autorisierung schreibt Mechanismen für den Austausch und die Kodierung von Autorisierungen fest. Die Autorisierungen selbst instanziieren im Krankenhaus vergebene Berechtigungen für einen Zugriff einer Person auf eine geschützte Ressource (z.B. "Der zugreifende Nutzer darf Observation-Ressourcen des Patienten mit der ID 123 suchen und abrufen."). Autorisierungen werden in dem ISiK-Sicherheit zugrunde liegenden Bild einer IT-Sicherheitsinfrastruktur durch einen Autorisierungsserver im Ergebnis der Prüfung festgelegter Berechtigungen vergeben. Diese Berechtigungen wiederum leiten sich aus Sicherheitspolitiken des Krankenhauses, Rollendefinitionen, durch Patienten gegebene Einwilligungen und weiteren Vorgaben ab. Im ISiK zugrundeliegenden Bild erfolgt die Verwaltung von Berechtigungen über einen _Policy Administration Point_.  

ISiK-Autorisierung sieht für die Kodierung und Durchsetzung von Autorisierungen drei miteinander kombinierbare Konzepte aus FHIR bzw. SMART-on-FHIR vor:
* (Launch) Kontexte
* Compartments 
* an FHIR-Ressourcen gebundene Zugriffsrechte

## Kontexte

Jeder Zugriff auf eine geschützte Ressource erfolgt im Kontext eines Patienten, eines Behandlungsfalls oder einer anderen Ressource. Der Kontext wird vom aufrufenden Client "mitgebracht" und stellt den Bezugspunkt für alle anderen Berechtigungsinformationen dar. Aus Sicht des Client stellt dieser Kontext den "aktuellen Patienten", den "aktuellen Fall", etc. dar.

Beispiel: Der Nutzer hat in/aus der ISiK-Clientanwendung den Patient "123" geöffnet und möchte nun Daten zu diesem Patienten verarbeiten, zu deren Abruf eine Autorisierung erforderlich ist. Der Zugriffskontext der Autorisierung ist der Patient "123". Alle anwendbaren Compartments und Zugriffsrechte (s.u.) beziehen sich auf den Patienten "123". 

ISiK-Autorisierung macht in der ISiK Stufe 3 keine Vorgabe, wie ein ISiK-Client in einen bestimmten Kontext gestellt wird (SMART-on-FHIR sieht hierfür z. B. die oben skizzierten Mechanismen eines _EHR Launch_ bzw. eines _Standalone Launch_ vor). Es wird jedoch verlangt, dass eine Kontextinformation als Teil des Zugriffstokens (s.u.) beim Aufruf einer ReST-Schnittstelle eines ISiK-Ressourcenservers übergeben wird und dass der Ressourcenserver alle im Zugriffstoken kodierten Autorisierungsinformationen in diesem Kontext anwendet.

## Compartments

Autorisierungen können in FHIR an eine 'Fokus'-Ressource gebunden werden, z. B. eine Patient-Ressource ("Zugriff auf Daten zum Patienten 123") oder eine Encounter-Ressource ("Zugriff auf Daten zum Behandlungsfall 456"). Um die 'Fokus'-Ressource herum gruppieren sich weitere Ressourcen, die mit dieser in einer Beziehung stehen, z. B. im Fall der Patient-Ressource die dem Patienten zugeordneten Beobachtungen, Diagosen/Probleme, Termine, Behandlungspläne, etc. In FHIR werden diese Gruppierungen über Ressourcen vom Ressourcentyp [_CompartmentDefinition_](http://hl7.org/fhir/compartmentdefinition.html) festgelegt. Diese definiert die Elemente einer Ressource, die die Bindung zu der 'Fokus'-Ressource herstellen. 

Beispiel: Für den Ressourcentyp [_Condition_](http://hl7.org/fhir/condition.html) legt die [_CompartmentDefinition_ der _Patient_-Ressource](http://hl7.org/fhir/compartmentdefinition-patient.html) die Elemente 'Condition.patient' und 'Condition.participant-actor' als verbindende Elemente fest. Eine Autorisierung für den Zugriff auf Patientendaten im Kontext des Patienten "123" umfasst damit grundsätzlich nur _Condition_-Ressourcen, deren 'subject'- oder 'participant-actor'-Element auf den Patienten "123" verweist.  

ISiK-Autorisierung in der ISiK Stufe 3 verlangt von FHIR-Ressourcenservern, dass diese zumindest Autorisierungen mit Bezug zu allem in FHIR definierten _Compartment_-Definitionen verarbeiten können.

## Zugriffsrechte auf Ressourcen

In dem von SMART-on-FHIR profilierten _OAuth2_-Standard legen sog. _Scopes_ die spezifischen Aktionen und/oder Daten fest, auf die eine Client-Anwendung in Vertretung eines Benutzers zugreifen bzw. diese manipulieren kann. Z.B. kann ein _Scope_ den Zugriff auf _Observation_-Ressourcen autorisieren oder die Möglichkeit gewähren, neue _Condition_-Ressourcen in einem System anzulegen.

_Scopes_ werden vom API-Anbieter - im Fall von ISiK dem ISiK-Ressourcenserver - definiert und von dem zugreifenden ISiK-Client während des Autorisierungsprozesses über den Autorisierungsserver angefordert. Sofern die Auswertung der anwendbaren Berechtigungsregeln durch den Autorisierungsserver die angeforderten _Scopes_ bestätigt, wird dem ISiK-Client ein Zugriffstoken (_Access Token_)ausgestellt, das die anwendbaren _Scopes_ enthält. Der ISiK-Client kann anschließend das Zugriffstoken verwenden, um im Rahmen der durch die _Scopes_ bestätigten Autorisierung auf die geschützten Ressourcen eines ISiK-Ressourcenservers zuzugreifen.

ISiK-Autorisierung in der ISiK Stufe 3 verlangt von FHIR-Ressourcenservern, dass diese die in SMART-on-FHIR definierte Syntax für die Bestätigung von _Scopes_ verarbeiten können und die mit den _Scopes_ verbundenen Autorisierungen zur Absicherung ihrer ReST-Schnittstellen anwenden können.

## Zusammenspiel von Kontexten, Compartments und Zugriffsrechten auf Ressourcen

_Compartments_ grenzen ab, auf welche mit einer 'Fokus'-Ressource gruppierten Ressourcen ein ISiK-Client überhaupt zugreifen kann. Welche Ressource konkret den 'Fokus' darstellt, wird über den Kontext bestimmt. Über _Scopes_ bestätigte Zugriffsrechte grenzen ein, auf welchen der mit der 'Fokus'-Ressource gruppierten Ressourcen der ISiK-Client welche Operationen ausführen darf. 

Beispiel: Der aus Sicht des ISiK-Clients aktuelle Patient ist der Patient "123". Dieser ist hiermit auch der Kontext der Autorisierung. Die Kombination aus der _CompartmentDefinition_ für die _Patient_-Ressource und dem Zugriffsrecht 'patient/Observation.r' legt fest, dass der ISiK-Client nur lesend und nur auf _Observation_-Ressourcen zugreifen darf, die über 'Observation.subject' oder 'Observation.performer' dem Patienten "123" zugeordnet sind.


## Zugriffstoken: Beispiel

Zugriffstoken werden als Base64-kodierte _JSON Web Token (JWT)_ ausgetauscht. 

Beispiel:

```
eyJhbGciOiAiSFMyNTYiLCAidHlwIjogIkpXVCJ9.eyJwYXRpZW50IjogIjg3YTMzOWQwLThjYWUtNDE4ZS04OWM3LTg2NTFlNmFhYjNjNiIsICJ0b2tlbl90eXBlIjogImJlYXJlciIsICJzY29wZSI6ICJwYXRpZW50L09ic2VydmF0aW9uLnJzIiwgImNsaWVudF9pZCI6ICJraHpnX3BvcnRhbCIsICJpYXQiOiAxNjU1MjYwMDAwLCAiZXhwIjogMTY1NTI2MDYwMH0.MrzWx4Ob7xuZLs9vG8A70sva21VzheQ0c3q0o8KtW-4
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
  "iat": 1655260000,
  "exp": 1655260600
}
```

Das Zugriffstoken in dem Beispiel gewährt Lese- und Such-Zugriffe auf die Patient-Ressource und Observation-Ressourcen des Patienten mit der _id_ '87a339d0-8cae-418e-89c7-8651e6aab3c6'. Das Token wurde am 14. April 2023 10:00 Uhr ausgestellt und ist ab diesem Zeitpunkt 10 Minuten lang gültig.