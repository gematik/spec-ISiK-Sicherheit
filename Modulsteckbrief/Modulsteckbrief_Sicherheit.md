# Modul-Name Sicherheit

## Kurzbeschreibung
Das Modul Sicherheit umfasst zwei verschiedene Use Cases die eine Authentifizierung und Authorisierung von verschiedenen Akteueren innerhalb des Krankenhauses ermöglichen, sodass gesichert Daten von einem IsiK-FHIR-Endpunkt gelesen bzw. dort gespeichert werden können.

1. Benutzer einer Drittanbieter-Software (Patient/Practitioner) möchte Zugang zum einem IsiK-FHIR-Endpunkt erhalten (Lese/- und/oder Schreibrechte).

2. Drittanbieter-Software kommuniziert mit einem IsiK-FHIR-Endpunkt (Machine-Zu-Machine-Kommunikation)

Problemstellung: Um einen sicheren Zugang zu den Daten eines IsiK-FHIR-Endpunktes zu gewährleisten müssen entsprechende Maßnahmen rund um die Authentifizierung und Autorisierung von Benutzern sichergestellt werden. Insbesondere in Hinblick auf die Öffnung des KIS für den Zugriff durch Subsysteme sollte eine Standardisierung (Empfehlung von existierenden Profilen) angestrebt werden, sodass entsprechende Clients einheitliche Schnittstellen vorfinden können. Zudem wird darauf hingewiesen, dass für eine Schnittstelle mit Schreibrechten eine entsprechende Authentifizierung zwingend notwendig ist, sodass die Herkunft der Daten stets nachvollzogen werden kann.

Zusätzlich könnte das Modul umfassen:
- ~~FHIR-Endpoint-Discovery~~
- Client Registration
- Break The Glass

## Bezug KHZG
Kein direkter Bezug zum KHZG

## Stakeholder

* bvitg
* Endandwender der bestätigungsrelevanten Systeme in den Krankenhäusern

## Relevante Spezifikationen
* [SMART App Launch Framework](http://hl7.org/fhir/smart-app-launch/index.html)
* [SMART Backend Services: Authorization Guide](https://hl7.org/fhir/uv/bulkdata/authorization/index.html)
* [IHE IUA](https://profiles.ihe.net/ITI/IUA/index.html)
* [RFC7591](https://tools.ietf.org/html/rfc7591)
* [Validated Healthcare Directory Implementation Guide](http://hl7.org/fhir/uv/vhdir/2018Jan/index.html)

## Datenobjekte/FHIR-Ressourcen

### existierende Datenobjekte

* Erweiterung der Suchmöglichkeiten um eine Compartment-Search-API (notwendig zur Definition des Scopes für die Anfrage in einem Patientenkontext)

### neue Datenobjekte
~~* Endpoint~~ (nur notwendig für FHIR-Endpoint-Discovery)


## Potentielle Probleme/Risiken
* Bestehende Autorisierung- / Authentifizierungslösung + Rollen & Rechte Konzepte müssen mitbetrachtet werden => Feedback von Herstellern von bestätigungsrelevanten Systemen notwendig

### Anwendungsfälle Sicherheit

* Patientenportale / Zuweiserportale als SaaS
    * (1) Beispielsweise könnten Termine über externe Systeme über das Appointment-Modul angefragt und bestätigt werden
    * (2) Patientenportale könnten im Auftrag der PatientIn die Daten in ein externen System übertragen

* ISiP
    * (3) Anfrage von ambulanten Behandlungsdaten aus dem stationären Kontext heraus
    * (4) Pflegeüberleitungsbogen übertragen (PULL anstatt PUSH)

* Interne Kommunikation absichern
    * (5) Innerhalb des stationären Kontexts sollten die FHIR-Endpunkte abgesichert sein

* Machine-To-Machine-Kommunikation
    * (6) Abgleich von Behandlungsdaten ohne zentrales Repository (z.B. zwischen LIS / KIS)

* Rollen- / Rechtekonzepte

Hinweis: Das Modul strebt nicht an Vorgaben für die Systeme zu formulieren wie bestimmte Scopes zu interpretieren sind. Die bestätigungsrelevanten Systeme müssen entsprechend nur die standardisierten Scopes anwenden können. Die genaue Umsetzung hiervon wäre - wie bisher - systemintern.

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

Das Testing-Backend müsste erweitert werden um die Möglichkeit einen SMART App Launch durchzuführen (als Client).
