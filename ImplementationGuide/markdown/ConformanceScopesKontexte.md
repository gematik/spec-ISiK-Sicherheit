# Conformance: Scopes und Kontexte

Die Vorgaben von ISiK-Sicherheit betreffen aktuell ausschließlich Systeme in der Rolle eines ISiK-Ressourcenservers. Diese Systeme MÜSSEN die auf dieser Seite beschriebenen Autorisierungsinformationen bei jedem Zugriffsversuch auf FHIR-Ressourcen verarbeiten können.

## Kontexte 

Bestätigungsrelevante Systeme in der Rolle eines ISiK-Ressourcenservers DÜRFEN KEINE Zugriffstoken (_Access Token_) akzeptieren, in denen kein Kontext als Bezugspunkt für die gewährten Zugriffsrechte angegeben ist bzw. per _Introspection_ ermittelt werden kann. 
Es MÜSSEN alle Kontexte unterstützt werden, für die eine FHIR _CompartmentDefinition_ existiert. Der Name des Kontexts entspricht dem kleingeschriebenen Wert des Elements 'CompartmentDefinition.code' ("patient", "encounter", etc.).

Beispiele: 

```"patient": "87a339d0-8cae-418e-89c7-8651e6aab3c6"```

```"encounter": "dd345a12-ed67-e451-3422-9813d3a400bc"```

## Compartments

Bestätigungsrelevante Systeme in der Rolle eines ISiK-Ressourcenservers MÜSSEN bei der Durchsetzungen von Autorisierungen alle in FHIR vorgegbenen [_CompartmentDefinition_](http://hl7.org/fhir/R4/compartmentdefinition.html)-Festlegungen unterstützen:
* [Patient](http://hl7.org/fhir/R4/compartmentdefinition-patient.html)
* [Encounter](http://hl7.org/fhir/R4/compartmentdefinition-encounter.html)
* [Practitioner](http://hl7.org/fhir/R4/compartmentdefinition-practitioner.html)
* [RelatedPerson](http://hl7.org/fhir/R4/compartmentdefinition-relatedperson.html)
* [Device](http://hl7.org/fhir/R4/compartmentdefinition-device.html)


Die Unterstützung eines Compartments umfasst, dass die Festlegungen in der _CompartmentDefinition_ 
* alle für die angegebene Fokusressource relevanten Zugriffsberechtigungen auf in beziehunge stehende Ressourcen definiert
* die Gruppierung von über _Scopes_ angegebenen Berechtigungen zu der als Kontext angegebenen Ressource bestimmen.

Bestätigungsrelevante Systeme in der Rolle eines ISiK-Ressourcenservers MÜSSEN NICHT für alle in ISiK spezifizierten Ressourcen entsprechende Compartmentdefinitionen anlegen. Die HL7 Spezifikationen lassen keine eigenständige erweiterung der Compartmentdefinitionen zu. 

Beispiel: Der gegebene Kontext ist der Patient "123". Die über einen _Scope_ angegebene Autorisierung ist 'patient/Observation.read'. Der ISiK-Ressourcenserver darf nur Anfragen ausführen, die lesend auf _Observation_-Ressourcen zugreifen, die über 'Observation.subject' oder 'Observation.performer' dem Patienten "123" zugeordnet sind.

## Berechtigungen auf Ressourcen

Berechtigungen auf Ressourcen MÜSSEN sowohl in der _SMART Capabilities_ Datei als auch in den gegenüber dem ISiK-Ressourcenserver bestätigten Autorisierungen in der folgenden Syntax kodiert werden:

```(patient | user | system) \/ (_Ressourcetyp_ | \*) \. c?r?u?d?s? (\? (_param_\=_value_) (\& _param_\=_value_)* )?```

### Scope-Level
SMART-on-FHIR-Berechtigungen auf Ressourcen lassen sich in drei Kategorien einteilen:

* ["patient"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#patient-specific-scopes): Alle FHIR Restful Interaktionen werden auf die definierten Zugriffe auf eine als Kontext angegebene FHIR-Ressource, sowie damit über Compartments verknüpfte Ressourcen, eingeschränkt.  
* ["user"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#user-level-scopes): Alle FHIR Restful Interaktionen werden eingeschränkt hinsichtlich der definierten Zugriffe auf Ressourcen, die für bestimmte Benutzer sichtbar sind.
* ["system"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#system-level-scopes): Alle FHIR Restful Interaktionen werden eingeschränkt hinsichtlich der definierten Zugriffe auf Ressourcen, die sichtbar sind für einen bestimmten (technischen) Client, unabhängig davon welcher Benutzer hiermit interagiert.


**Autorisierungen in einem _"patient"-Level Scope_, die Ressourcen abfragen die über die in der Compartmentdefiniton definierten beziehungen zur Fokussressource des Patienten hinausgehen, MÜSSEN ignoriert werden.**

**Autorisierungen in einem eigens definierten SMART on FHIR Launch Kontext bzw. Wildcard Scope, für die keine Compartmentdefinition existiert, dürfen nicht ignoriert werden. Zugriffe auf Instanzen dieser Ressourcentypen SOLLTEN über Autorisierungen in einem "user"- oder "system"-Level erfolgen. Bspw. launch/code oder launch/location**

Autorisierungen in einem _"patient"-Level Scope_, die Ressourcentypen betreffen, zu denen keine 'CompartmentDefinition' existiert, MÜSSEN ignoriert werden. Zugriffe auf Instanzen deser Ressourcentypen MÜSSEN über Autorisierungen in einem "user"- oder "system"-Level erfolgen.

Insbesondere mittels _"system"-Level Scopes_ MUSS ein Vollzugriff auf die im ISIK- Kontext definieren Ressourcen auf einem ISiK-Ressourcenserver möglich sein, soweit dieser keine weiteren Einschränkungen in der Sichtbarkeit von Inhalten konfiguriert. Bestätigungsrelevante Systeme MÜSSEN die Option anbieten, einen Vollzugriff zu erlauben und SOLLTEN diesen Vollzugriff über einen _"system"-Level Scopes_ umsetzen.

### Ressourcetyp und Operationen
Es MÜSSEN alle in ISiK Stufe 3 profilierten Ressourcentypen unterstützt werden. Sofern in ISiK-Stufe 3 auf einem Ressourcentyp als zulässig definiert, MÜSSEN alle in FHIR definierten lesenden und modifizierenden Operationen unterstützt werden:

|Berechtigung|FHIR Operation auf System-Ebene|FHIR Operation auf Typ-Ebene|FHIR Operation auf Instanz-Ebene|
|:-----------|:------------------------------|:---------------------------|--------------------------------|
|c           |                               |create                      |                                |
|r           |                               |                            |read, vread, history            |
|u           |                               |                            |update, patch                   |
|d           |                               |                            |delete                          |
|s           |search, history                |search, history             |                                |

Berechtigungen MÜSSEN im _Scope_ in der dargestellten Reihenfolge ('cruds') angegeben sein. Bei einer falschen Reihenfolge SOLL der Ressourcenserver einen Zugriffsfehler auslösen.

Die Möglichkeit von [Wildcard-Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#wildcard-scopes) MUSS unterstützt werden.

### Filter
Alle in ISiK STufe 3 für den Ressourcetyp unterstützten Suchparameter inkl. Modifier und Kombinationsmöglichkeiten MÜSSEN als Teil eines Scopes unterstützt werden.

### Beispiele

Die folgende Beispiele geben gültige Scopes wieder:

* ```patient/Patient.rs```
* ```patient/Observation.cruds```
* ```patient/Observation.rs?category=http://terminology.hl7.org/CodeSystem/observation-category|laboratory```

