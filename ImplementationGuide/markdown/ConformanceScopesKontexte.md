> **NOTE:**
> * Das Kapitel vermischt "Scope" und "Permissions" und sollte dahingehend überarbeitet werden. Der Scope definiert Beschränkungen für die App. Der Scope ist immer App bezogen. Permissions (Berechtigungen) sind nutzerbezogen und vom ResourceServer unter Beachtung des jeweiligen Kontexts durchzusetzen. Dementsprechend werden nicht Nutzerberechtigungen delegiert, sondern der Eigentümer definiert mit Scopes, was die App in seinem Namen mit den Resourcen machen darf.


# Conformance: Scopes und Kontexte

Die Vorgaben von ISiK-Sicherheit betreffen aktuell ausschließlich Systeme in der Rolle eines ISiK-Ressourcenservers. Diese Systeme MÜSSEN die auf dieser Seite beschriebenen Autorisierungsinformationen bei jedem Zugriffsversuch auf FHIR-Ressourcen verarbeiten können.

## Kontexte 

Bestätigungsrelevante Systeme in der Rolle eines ISiK-Ressourcenservers DÜRFEN KEINE Zugriffstoken (_Access Token_) akzeptieren, in denen kein Kontext als Bezugspunkt für die gewährten Zugriffsrechte angegeben ist bzw. per _Introspection_ ermittelt werden kann. 

Es MÜSSEN alle Kontexte unterstützt werden, für die eine FHIR _CompartmentDefinition_ existiert. Der Name des Kontexts entspricht dem kleingeschriebenen Wert des Elements 'CompartmentDefinition.code' ("patient", "encounter", etc.).

Beispiele: 

```"patient": "87a339d0-8cae-418e-89c7-8651e6aab3c6"```

```"encounter": "dd345a12-ed67-e451-3422-9813d3a400bc"```

## Compartments

Bestätigungsrelevante Systeme in der Rolle eines ISiK-Ressourcenservers MÜSSEN bei der Durchsetzungen von Autorisierungen die Festlegungen zum [_Compartment Patient_](http://hl7.org/fhir/R4/compartmentdefinition-patient.html) unterstützen. 

Bestätigungsrelevante Systeme in der Rolle eines ISiK-Ressourcenservers SOLLEN bei der Durchsetzungen von Autorisierungen die Festlegungen zum [_Compartment Encounter_](http://hl7.org/fhir/R4/compartmentdefinition-encounter.html) unterstützen. 

Bestätigungsrelevante Systeme in der Rolle eines ISiK-Ressourcenservers KÖNNEN weitere der in FHIR vorgegbenen [_CompartmentDefinition_](http://hl7.org/fhir/R4/compartmentdefinition.html)-Festlegungen unterstützen:
* [Practitioner](http://hl7.org/fhir/R4/compartmentdefinition-practitioner.html)
* [RelatedPerson](http://hl7.org/fhir/R4/compartmentdefinition-relatedperson.html)
* [Device](http://hl7.org/fhir/R4/compartmentdefinition-device.html)

Die Unterstützung eines Compartments umfasst, dass die Festlegungen in der _CompartmentDefinition_ 
* die Gruppierung von über _Scopes_ angegebenen Berechtigungen zu der als Kontext angegebenen Ressource bestimmen und
* im _"patient"-Level Scope_ (s.u.) die maximal zulässigen Berechtigungen eines Zugriffs auf die den angegebenen Kontext darstellende Ressource bestimmen.

Beispiel: Der gegebene Kontext ist der Patient "123". Die über einen _Scope_ angegebene Autorisierung ist 'patient/Observation.r'. Der ISiK-Ressourcenserver darf nur Anfragen ausführen, die lesend auf _Observation_-Ressourcen zugreifen, die über 'Observation.subject' oder 'Observation.performer' dem Patienten "123" zugeordnet sind.

## Berechtigungen auf Ressourcentypen

Berechtigungen auf Ressourcentypen MÜSSEN sowohl in der _SMART Capabilities_ Datei als auch in den gegenüber dem ISiK-Ressourcenserver bestätigten _Scopes_ in der folgenden Syntax kodiert werden:

```(patient | user | system) \/ (_Ressourcetyp_ | \*) \. c?r?u?d?s? (\? (_param_\=_value_) (\& _param_\=_value_)* )?```

### Scope-Level
SMART-on-FHIR-Berechtigungen auf Ressourcen lassen sich in drei Kategorien einteilen, die alle durch ISiK-konforme Ressourcenserver unterstützt werden MÜSSEN:

* ["patient"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#patient-specific-scopes) geben an, welche verfügbaren Nutzerberechtigungen auf allen Ressourcen im gewählten Patient-Compartment an den Client delegiert werden bzw. werden sollen.  
* ["user"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#user-level-scopes) geben an, welche verfügbaren Nutzerberechtigungen auf allen Ressourcen durch den Benutzer an den Client delegiert werden bzw. werden sollen.
* ["system"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#system-level-scopes) geben an, welche verfügbaren Nutzerberechtigungen auf allen Ressourcen an einen (technischen) Client delegiert werden bzw. werden sollen, unabhängig davon welcher Benutzer hiermit interagiert.

Autorisierungen in einem SMART-on-FHIR _Launch Kontext_, für den keine Compartment-Definition existiert (z. B. 'launch/location'), SOLLEN in einem _"user"_- oder _"system"-Level Scope_ erfolgen (z. B. 'user/Location.rs').

### Ressourcetyp und Operationen
Es MÜSSEN alle in ISiK Stufe 3 profilierten Ressourcentypen unterstützt werden. Sofern in ISiK-Stufe 3 auf einem Ressourcentyp als zulässig definiert, MÜSSEN alle in FHIR definierten lesenden und modifizierenden Operationen unterstützt werden:

|Berechtigung|FHIR Operation auf System-Ebene|FHIR Operation auf Typ-Ebene|FHIR Operation auf Instanz-Ebene|
|:-----------|:------------------------------|:---------------------------|--------------------------------|
|c           |                               |create                      |                                |
|r           |                               |                            |read, vread, history            |
|u           |                               |                            |update, patch                   |
|d           |                               |                            |delete                          |
|s           |search, history                |search, history             |                                |

Berechtigungen werden im _Scope_ in der dargestellten Reihenfolge ('cruds') angegeben (vgl. https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#clinical-scope-syntax). Bei einer falschen Reihenfolge SOLL der Ressourcenserver einen Zugriffsfehler auslösen.

Die Möglichkeit von [Wildcard-Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#wildcard-scopes) MUSS unterstützt werden.

### Filter
Alle in ISiK STufe 3 für den Ressourcetyp unterstützten Suchparameter inkl. Modifier und Kombinationsmöglichkeiten MÜSSEN als Teil eines _Scopes_ unterstützt werden.

### Beispiele

Die folgende Beispiele geben gültige _Scopes_ wieder:

* ```patient/Patient.rs```
* ```patient/Observation.cruds```
* ```patient/Observation.rs?category=http://terminology.hl7.org/CodeSystem/observation-category|laboratory```

