# Scope-Syntax

SMART on FHIR Access Scopes lassen sich in drei Bereiche kategorisieren, welche sich darin unterscheiden auf wie der zum Authorisierungssystem zugehörige Ressourcenserver entscheiden muss ob eine Restful Interaktion erlaubt ist oder abgewiesen werden muss. Clients können folgende Arten von Scopes anfragen:

* ["patient"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#patient-specific-scopes): Alle FHIR Restful Interaktionen werden eingeschränkt auf den Zugriff auf ein bestimmte Instanz einer Patient-Ressource, sowie damit verknüpfte Ressourcen. Eine Zusammengehörigkeit von Ressourcen KANN mittels des [FHIR-Patient-Compartments](http://www.hl7.org/fhir/compartmentdefinition-patient.html) evaluiert werden. Es MÜSSEN jedoch mindestens die dort aufgelisteten Relationen unterstüzt werden. Weitere für Patienten-unabhänige Ressource, welche für den Kontext relevant sein können, SOLLTEN ebenfalls mittels eines entsprechenden Scope herausgegeben werden, z.B. "Practitioner"- oder "Organisation"-Ressourcen.
* ["user"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#user-level-scopes): Alle FHIR Restful Interaktionen werden eingeschränkt auf den Zugriff auf Ressourcen-Instanzen die sichtbar sind für ein bestimmte Benutzer*in.
* ["system"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#system-level-scopes): Alle FHIR Restful Interaktionen werden eingeschränkt auf den Zugriff auf Ressourcen-Instanzen die sichtbar sind für einen bestimmten (technischen) Client, unabhängig davon welcher Benutzer hiermit interagiert.

Alle oben genannten Arten von Scopes müssen durch den Authorisierungsserver unterstüzt werden in der Anfrage eines Tokens. Inbesondere mittels "system"-Level Scopes MUSS ein Vollzugriff auf den Ressourcen-Server erlaubt werden, soweit für den keine weiteren Einschränkungen in der Sichtbarkeit von Inhalten konfiguriert. Bestätigungsrelevante Systeme MÜSSEN die Option anbieten einen Vollzugriff zu erlauben.

Die "Clinical Scope Syntax" MUSS durch einen Authorisierungsserver vollständig unterstüzt werden, in Abhängigkeit davon welche Interaktionen auf dem dazugehörigen Ressourcen-Server erlaubt sind:

* Alle unterstützten Create, Update, Delete, Read, Search-Interaktionen MÜSSEN als Teil eines Scopes unterstützt werden
* Alle unterstützten Ressourcen-Typen MÜSSEN als Teil eines Scopes unterstützt werden
* Alle unterstützten Suchparameter inkl. Modifier und Kombinationsmöglichkeiten MÜSSEN als Teil eines Scopes unterstützt werden

Die Möglichkeit von [Wildcard-Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#wildcard-scopes) MUSS unterstützt werden.

# Launch-Context