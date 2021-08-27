# App bittet um Autorisierung

Abhängig davon ob der Client durch einen EHR Launch oder Standalone Launch gestartet wird, existieren unterschiedliche Schritte für die Anfrage eines Authorisierungscodes.

## EHR Launch

Das bestätigungsrelevante System MUSS in der Lage sein durch einen externen Kontextaufruf den Client zu starten. Im Kontext des derzeitig eingeloggten Benutzers wird der Client gestartet, vgl. [EHR Launch / Standalone Launch - TODO LINK](). Der Aufruf des Clients MUSS alle in [EHR launch sequence](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#ehr-launch-sequence) dokumentierten Parameter enthalten.

Es sei darauf hingewiesen, dass jeder EHR Launch mit einem eindeutigen Launch Parameter zu assoziieren ist. Dieser Parameter dient dient dazu, dass der Client das Access Token verknüpfen kann mit dem Kontext aus dem der Client gestartet worden ist und kann beliebig gewählt werden (z.B. eine UUID). Der Kontext kann beispielsweise Informationen zum Patienten oder Kontakt/Fall enthalten welcher dem Anwender zuvor präsentiert worde. Dieser Kontext wird dem Client durch sogenannte [Launch Context Claims](http://build.fhir.org/ig/HL7/smart-app-launch/scopes-and-launch-context.html#scopes-for-requesting-context-data) vermittelt. Diese Claims enthalten IDs der FHIR-Ressourcen welche die zuvorgenannten Datenobjekte repräsentieren. Es ist notwendig innerhalb der SMART authorization sequence die angeforderten [Launch Context Claims](http://build.fhir.org/ig/HL7/smart-app-launch/scopes-and-launch-context.html#scopes-for-requesting-context-data) an den Client zurückzugeben, vgl. Abschnitt [Austausch des Autorisierungscodes für ein Zugangstoken - TODO Link](). Der Client kann Hinweise geben welche Kontextparameter gewünscht sind. Sollten diese jedoch nicht verfügbar sein (z.B. der Client wurde ohne Patientekontext aufgerufen), können die zurückgegebenen Launch Context Claims von den gewünschten Scopes abweichen.

## Standalone launch sequence

Aufgrund des fehlenden Kontexts zwischen Client und bestätigungsrelevanten System KANN der Client durch Angabe von gewünschten Smart Launch Scopes bestimmen, welche Details durch den Authorisierungsserver in der Access Token Response bereitgestellt werden MÜSSEN. Beispielsweise kann, äquivalent zum zum EHR Launch, der Patienten und/oder Kontakt/Fall Kontext angefordert werden. Genaue Details für die Syntax der Launch Context Scope siehe [SMART on FHIR Launch Context Scope Syntax - TODO Link]().

Der Aufruf des Clients MUSS alle in [1.0.6.1.1 - Step 1: App asks for authorization](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#ehr-launch-sequence) dokumentierten Parameter enthalten. Inbesondere gilt dies für die Untersützung von HTTP POST-basierten Autorisierunganfragen und der Untersützung von [Proof Key for Code Exchange by OAuth Public Clients](https://datatracker.ietf.org/doc/html/rfc7636).

## TLS

Alle Authorisierungsendpunkte MÜSSEN per HTTPS (TLS-Verschlüsselung) erreichbar sein.'. Im Echtbetrieb MUSS die Kommunikation ausschließlich per HTTPS erfolgen. Vorgaben zur einzusetzenden TLS Version, siehe [Sicherheitsaspekte](https://simplifier.net/guide/ImplementierungsleitfadenISiK-Basismodul/UebergreifendeFestlegungenRest).

## Abruf SMART Configuration

Als Einstiegspunkt für einen Standalone Launch MUSS dem Client die Url des FHIR Endpunktes bekannt gegeben werden. Anschließend erfolgt eine Abfrage des ".well-known/smart-configuration" Dokumentes, welche durch den FHIR Endpunkt bereitgestellt werden MUSS. Vorgaben zum Format sind dem Abschnitt [Smart Configuration - TODO Link]() zu entnehmen. Hierdurch erhält der Client dynamisch die Adresse der Authorisierungsservers inkl. "authorize" und "token" Endpunkt. Falls durch das bestätigungsrelevante System OAuth 2.0 Endpunkte optional über das CapabilityStatement des FHIR-Endpunktes zur Verfügung gestellt werden MÜSSEN diese Inhalte indentisch sein zu den Inhalten des ".well-known/smart-configuration" Dokumentes.

## Hinweise zu Identity Scopes

Um Informationen über den authentifizierten Endbenutzer zu erhalten kann ein Client per OpenID Connect ein Identitätstoken zusammen mit einem Zugangstoken anfragen. Hierzu sind in Kombination die Scopes "openid" und "fhirUser" zu verwenden. Zu bestätigende Systeme MÜSSEN die Vorgaben nach [2.0.4 - Scopes for requesting identity data](http://build.fhir.org/ig/HL7/smart-app-launch/scopes-and-launch-context.html#scopes-for-requesting-identity-data) umsetzen. Die Untersützung des "profile" claims ist optional. Inbesondere sind die Vorgaben zur Umsetzung der OpenID Connect Core specification bestätigungsrelevant.

## Hinweise zu Access Scopes

Innerhalb des Scope Parameters welcher als Teil der Autorisierungsanfrage versendet wird, kann der Client dem Server mitteilen welche Scopes zur korrekten Ausführung notwendig sind. Diese Scopes repräsentieren die Menge aller Scopes welche durch den Client gewünscht werden, jedoch nicht notwendigerweise durch den Server unterstüzt und/oder erlaubt werden. Es steht dem Autorisierungsserver frei diese Scopes einzuschränken, falls der Client für die Anforderung der Scopes nicht berechtigt ist. Weitere Details zur Syntax der Access Scopes siehe [SMART on FHIR Access Scope Syntax - TODO Link]().
