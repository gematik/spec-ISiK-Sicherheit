# Conformance: Scopes und Kontexte

Die Vorgaben von ISiK Security betreffen aktuell ausschließlich Systeme in der Rolle eines FHIR-Ressourcenservers. 

## Kontexte 


## Compartments

Bestätigungsrelevante Systeme in der Rolle eines FHIR-Ressourcenservers MÜSSEN bei der Durchsetzungen von Autorisierungen die folgenden [CompartmentDefinition](http://hl7.org/fhir/R4/compartmentdefinition.html)-Festlegungen unterstützen:
* [Patient](http://hl7.org/fhir/R4/compartmentdefinition-patient.html)
* [Encounter](http://hl7.org/fhir/R4/compartmentdefinition-encounter.html)
Weitere Compartments KÖNNEN unterstützt werden.

Die Unterstützung eines Compartments umfasst, dass die Festlegungen in der CompartmentDefinition 
* die maximal zulässigen Berechtigungen eines Zugriffs auf die den angegebenen Kontext darstellende Ressource bestimmen und
* die Gruppierung von über Scopes angegebenen Berechtigungen zu der als Kontext angegebenen Ressource bestimmen.

Beispiel: Der gegebene Kontext ist der Patient 123. Die über einen Scope angegebene Autorisierung ist 'patient/Observation.read'. Der ISiK-Ressourcenserver darf nur Anfragen ausführen, die lesend auf Observation-Ressourcen zugreifen, die über Observation.subject oder Observation.performer dem Patienten 123 zugeordnet sind.

## Berechtigungen auf Ressourcen

Berechtigungen auf Ressourcen MÜSSEN sowohl in der SMART Capabilities Datei als auch in den gegenüber dem FHIR Ressourcenserver bestätigten Autorisierungen in der folgenden Syntax kodiert werden:

```(patient | user | system) \/ (_Ressourcetyp_ | \*) \. c?r?u?d?s? (\? (_param_\=_value_) (\& _param_\=_value_)* )?```

### Scope-Level
SMART on FHIR Berechtigungen auf Ressourcen lassen sich in drei Kategorien einteilen:

* ["patient"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#patient-specific-scopes): Alle FHIR Restful Interaktionen werden auf die definierten Zugriffe auf eine als Kontext angegebene Instanz einer Patient- oder Encounter-Ressource, sowie damit über Compartments verknüpfte Ressourcen, eingeschränkt.  
* ["user"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#user-level-scopes): Alle FHIR Restful Interaktionen werden eingeschränkt hinsichtlich der definierten Zugriffe auf Ressourcen-Instanzen, die für bestimmte Benutzer sichtbar sind.
* ["system"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#system-level-scopes): Alle FHIR Restful Interaktionen werden eingeschränkt hinsichtlich der definierten Zugriffe auf Ressourcen-Instanzen, die sichtbar sind für einen bestimmten (technischen) Client, unabhängig davon welcher Benutzer hiermit interagiert.

Insbesondere mittels "system"-Level Scopes MUSS ein Vollzugriff auf einen ISiK-Ressourcenserver möglich sein, soweit dieser keine weiteren Einschränkungen in der Sichtbarkeit von Inhalten konfiguriert. Bestätigungsrelevante Systeme MÜSSEN die Option anbieten, einen Vollzugriff zu erlauben.

### Ressourcetyp und Operationen
Es MÜSSEN alle in ISiK Stufe 3 profilierten Ressourcetypen unterstützt werden. Sofern in ISiK-Stufe 3 auf einem Ressourcetyp als zulässig definiert, MÜSSEN alle in FHIR definierten lesenden und modifizierenden Operationen unterstützt werden:

|Berechtigung|FHIR Operation auf System-Ebene|FHIR Operation auf Typ-Ebene|FHIR Operation auf Instanz-Ebene|
|:-----------|:-----------------|:-----------------|---------------------|
|c           |                  |create            |                     |
|r           |                  |                  |read, vread, history |
|u           |                  |                  |update, patch        |
|d           |                  |                  |delete               |
|s           |search, history   |search, history   |                     |

Die Möglichkeit von [Wildcard-Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#wildcard-scopes) MUSS unterstützt werden.

### Filter
Alle in ISiK STufe 3 für den Ressourcetyp unterstützten Suchparameter inkl. Modifier und Kombinationsmöglichkeiten MÜSSEN als Teil eines Scopes unterstützt werden.

### Beispiele

to be done

## Zugriffstoken

to be done
