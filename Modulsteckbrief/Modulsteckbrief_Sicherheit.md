# Modul-Name Sicherheit

## Kurzbeschreibung
Das Modul Sicherheit umfasst zwei verschiedene Use Cases die eine Authentifizierung und Autorisierung von verschiedenen Akteueren innerhalb des Krankenhauses ermöglichen, sodass gesichert Daten von einem IsiK-FHIR-Endpunkt gelesen bzw. dort gespeichert werden können.

1. Benutzer einer Drittanbieter-Software (Patient, Leistungserbringer etc.) möchte Zugang zum einem IsiK-FHIR-Endpunkt erhalten (Lese- und/oder Schreibrechte).

2. Drittanbieter-Software kommuniziert mit einem IsiK-FHIR-Endpunkt (Machine-Zu-Machine-Kommunikation)

Problemstellung: Um einen sicheren Zugang zu den Daten eines IsiK-FHIR-Endpunktes zu gewährleisten müssen entsprechende Maßnahmen rund um die Authentifizierung und Autorisierung von Benutzern sichergestellt werden. Insbesondere in Hinblick auf die Öffnung des KIS für den Zugriff durch Subsysteme sollte eine Standardisierung (Empfehlung von existierenden Profilen) angestrebt werden, sodass entsprechende Clients einheitliche Schnittstellen vorfinden können. Zudem wird darauf hingewiesen, dass für eine Schnittstelle mit Schreibrechten eine entsprechende Authentifizierung zwingend notwendig ist, sodass die Herkunft der Daten stets nachvollzogen werden kann.

Zusätzlich könnte das Modul umfassen:
- ~~FHIR-Endpoint-Discovery~~
- Client Registration
- Break The Glass

## Bezug KHZG
Kein direkter Bezug zum KHZG

## Stakeholder

* Hersteller und Betriebsverantwortliche der bestätigungsrelevanten Systeme in den Krankenhäusern
* Endandwender der bestätigungsrelevanten Systeme in den Krankenhäusern

## Relevante Spezifikationen
* [SMART App Launch Framework](http://hl7.org/fhir/smart-app-launch/index.html)
* [SMART Backend Services: Authorization Guide](https://hl7.org/fhir/uv/bulkdata/authorization/index.html)
* [IHE IUA](https://profiles.ihe.net/ITI/IUA/index.html)
* [RFC7591](https://tools.ietf.org/html/rfc7591)
* [Validated Healthcare Directory Implementation Guide](http://hl7.org/fhir/uv/vhdir/2018Jan/index.html)

## Datenobjekte/FHIR-Ressourcen

### existierende Datenobjekte

* Erweiterung der Suchmöglichkeiten um eine Compartment-Search-API (notwendig zur Definition des Scopes für die Anfrage in einem Patienten- oder Fallkontext)

### neue Datenobjekte
~~* Endpoint~~ (nur notwendig für FHIR-Endpoint-Discovery)


## Potentielle Probleme/Risiken
* Bestehende Autorisierung- und Authentifizierungslösungen sowie haus-individuelle Konzepte zu Benutzerrollen und -rechten sollen weiter nutzbar sein; Vorgaben zur Interoperabilität sollen zunächst auf den Austausch und die Umsetzung von _Access Token_ und der Durchsetzung von Autorisierungen bei Aufrufen von ISiK-FHIR-Endpunkten fokussieren.
* Für die Authentifizierung von PatientInnen sollen perspektivisch die sektoralen Identitätsdienste der gesetzlichen Krankenkassen nach [§ 291 Absatz 8 SGB V](https://www.gesetze-im-internet.de/sgb_5/__291.html) unterstützt werden. Die in ISiK getroffenen Festlegungen zur Autorisierung müssen entsprechend geeignet sein, aus einer Autorisierungsanfrage heraus eine Authentifizierung anzustoßen.

### Anwendungsfälle Sicherheit

* Patientenportale / Zuweiserportale 
    * (1) Anfrage und Bestätigung von Terminen können über ein Patientenportal erfolgen
    * (2) Patientenportale könnten im Auftrag der PatientIn die Daten in ein externen System übertragen

* ISiP
    * (3) Anfrage von ambulanten Behandlungsdaten aus dem stationären Kontext heraus
    * (4) Pflegeüberleitungsbogen übertragen (PULL anstatt PUSH)

* Interne Kommunikation absichern
    * (5) Innerhalb des stationären Kontexts sollten die FHIR-Endpunkte abgesichert sein

* Machine-To-Machine-Kommunikation
    * (6) Abgleich von Behandlungsdaten ohne zentrales Repository (z.B. zwischen LIS / KIS)

* Rollen- / Rechtekonzepte

Hinweis: Das Modul strebt nicht an, Vorgaben für die Systeme zu formulieren, wie bestimmte Scopes gegen im Krankenhaus definierte Rollen- und Rechtekonzepte zu interpretieren sind. Die bestätigungsrelevanten Systeme müssen entsprechend nur die standardisierten Scopes anwenden können. Die genaue Umsetzung hiervon ist - wie bisher - systemintern.

Durch unterschiedliche Scopes können verschiedene Use Cases abgesichert werden:

Patient-Level-Scopes:
- Use Case (1)
- Use Case (2)

User-Level-Scopes:
- Use Case (3)
- Use Case (4)
- Use Case (5)

System-Level-Scopes:
- Use Case (3)
- Use Case (4)
- Use Case (6)

### Testing

Das Testing-Backend muss um einen Autorisierungsserver erweitert werden, der ISiK-konforme _Access Token_ ausstellt. Hersteller sollen alternativ in der Testkonfiguration angeben können, dass sie einen eigene Autorisierungsserver nutzen. Die _Endpunkte müssen zur Laufzeit des Tests über die _SmartConfiguration_ des zu testenden ISiK-Ressourcenservers abfragbar sein.
  
