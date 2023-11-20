# Scope-Syntax

SMART on FHIR Access Scopes lassen sich in drei Kategorien einteilen. Diese Kategorien verwendet der zum Autorisierungssystem zugehörige Ressourcenserver, um zu entscheiden, ob eine Restful Interaktion erlaubt ist oder abgewiesen werden muss. Clients können folgende Arten von Scopes anfragen:

* ["patient"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#patient-specific-scopes): Alle FHIR Restful Interaktionen werden auf den Zugriff auf eine bestimmte Instanz einer Patient-Ressource, sowie damit verknüpfte Ressourcen, eingeschränkt. Eine Zusammengehörigkeit von Ressourcen KANN mittels des [FHIR-Patient-Compartments]https://www.hl7.org/fhir/R4/compartmentdefinition-patient.html) evaluiert werden. Es MÜSSEN jedoch mindestens die dort aufgelisteten Relationen unterstützt werden. Weitere für den Kontext relevante, aber Patienten-unabhängige Ressourcen, SOLLTEN ebenfalls mittels eines entsprechenden Scope herausgegeben werden, z.B. "Practitioner"- oder "Organisation"-Ressourcen.
* ["user"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#user-level-scopes): Alle FHIR Restful Interaktionen werden eingeschränkt hinsichtlich des Zugriffs auf Ressourcen-Instanzen, die für bestimmte Benutzer sichtbar sind.
* ["system"-Level Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#system-level-scopes): Alle FHIR Restful Interaktionen werden eingeschränkt hinsichtlich des Zugriffs auf Ressourcen-Instanzen, die sichtbar sind für einen bestimmten (technischen) Client, unabhängig davon welche Benutzer:in hiermit interagiert.

Alle oben genannten Arten von Scopes müssen durch den Autorisierungsserver in der Anfrage eines Tokens unterstützt werden. Insbesondere mittels "system"-Level Scopes MUSS ein Vollzugriff auf den Ressourcen-Server erlaubt werden, soweit dieser keine weiteren Einschränkungen in der Sichtbarkeit von Inhalten konfiguriert. Bestätigungsrelevante Systeme MÜSSEN die Option anbieten einen Vollzugriff zu erlauben.

Die "Clinical Scope Syntax" MUSS durch einen Autorisierungsserver vollständig unterstüzt werden, in Abhängigkeit davon welche Interaktionen auf dem dazugehörigen Ressourcen-Server erlaubt sind:

* Alle unterstützten CREATE, UPDATE, DELETE, READ, SEARCH-Interaktionen MÜSSEN als Teil eines Scopes unterstützt werden.
* Alle unterstützten Ressourcen-Typen MÜSSEN als Teil eines Scopes unterstützt werden.
* Alle unterstützten Suchparameter inkl. Modifier und Kombinationsmöglichkeiten MÜSSEN als Teil eines Scopes unterstützt werden.

Die Möglichkeit von [Wildcard-Scopes](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#wildcard-scopes) MUSS unterstützt werden.

# Launch-Context

Clients können über die zuvor beschriebenen Access-Scopes hinaus sogenannte Launch-Context-Scopes bei dem Autorisierungsserver anfordern. Launch Claims werden nicht als Teil des Access-Tokens, sondern als Teil des JSON-Dokumentes als Antwort auf die Anfrage eines Access-Tokens zurückgeliefert. Diese Claims enthalten entweder spezifische IDs oder URIs, die FHIR-Referenzen abbilden, um kontext-relevante Informationen vorab zu übermitteln, sodass der Client entsprechend einen "Einstiegspunkt" in das System erhält.

Launch-Scopes besitzen die Syntax ```launch/<resourcen-typ>```. Der "<resourcen-typ>" Teil des Scopes ist case-senstive und MUSS kleingeschrieben werden. Der Autorisierungsserver MUSS einen Patienten- (launch/patient) und Fallkontext (launch/encounter) herstellen können. Weitere Kontexte KÖNNEN durch den Autorisierungsserver etabliert werden - [vgl. SMART App Launch - 3.0.3.3.1 - fhirContext](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#fhircontext).

Die Kontextinformationen sind durch den Autorisierungsserver bei dem Ressourcen-Server anzufordern. Als Implementierungsunterstützung können die Informationen durch den "launch"-Parameter, welcher während eines EHR-Launchs ausgetauscht wurde, abgeleitet werden. Im Falle eines Standalone Launchs, können die Informationen manuell durch die Benutzer ausgewählt oder durch sonstige geeignete Maßnahmen bestimmt werden. Hierzu bestehen seitens der Spezifikation keine Vorgaben.

Die Vorgaben zur Kodierung der [Patient-, Encounter- und fhirContext- Claims](https://hl7.org/fhir/smart-app-launch/STU2/scopes-and-launch-context.html#launch-context-arrives-with-your-access_token) MÜSSEN implementiert werden.
