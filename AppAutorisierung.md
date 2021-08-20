# App bittet um Autorisierung

Abhängig davon ob der Client durch einen EHR Launch oder Standalone Launch gestartet wird, existieren unterschiedliche Schritte für die Abfrage eines Authorisierungscodes.

## EHR Launch

Das bestätigungsrelevante System MUSS in der Lage sein durch einen externen Kontextaufruf den Client zu starten. Im Kontext des derzeitig eingeloggten Benutzers wird der Client gestartet, vgl. [EHR Launch / Standalone Launch - TODO LINK](). Der Aufruf des Clients MUSS alle in [EHR launch sequence](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#ehr-launch-sequence) dokumentierten Parameter enthalten. Alle Endpunkte MÜSSEN per HTTPS (TLS-Verschlüsselung) erreichbar sein.'. Im Echtbetrieb MUSS die Kommunikation ausschließlich per HTTPS erfolgen. Vorgaben zur einzusetzenden TLS Version, siehe [Sicherheitsaspekte](https://simplifier.net/guide/ImplementierungsleitfadenISiK-Basismodul/UebergreifendeFestlegungenRest).

Es sei darauf hingewiesen, dass jeder EHR Launch mit einem eindeutigen Launch Parameter zu assoziieren ist. Dieser Parameter dient dient dazu, dass der Client das Access Token assoziieren kann mit dem Kontext aus dem der Client gestartet worden ist und kann beliebig gewählt werden (z.B. eine UUID). Der Kontext kann beispielsweise Informationen zum Patienten oder Kontakt/Fall enthalten welcher dem Anwender zuvor präsentiert worde. Dieser Kontext wird dem Client durch sogenannte [Launch Context Claims](http://build.fhir.org/ig/HL7/smart-app-launch/scopes-and-launch-context.html#scopes-for-requesting-context-data) vermittelt. Diese Claims enthalten IDs der FHIR-Ressourcen welche die zuvorgenannten Datenobjekte repräsentieren. Es ist notwendig innerhalb der SMART authorization sequence die angeforderten [Launch Context Claims](http://build.fhir.org/ig/HL7/smart-app-launch/scopes-and-launch-context.html#scopes-for-requesting-context-data) an den Client zurückzugeben, vgl. Abschnitt [Austausch des Autorisierungscodes für ein Zugangstoken - TODO Link]().

## Standalone launch sequence

Als Einstiegspunkt für einen Standalone Launch MUSS dem Client die Url des FHIR Endpunktes bekannt gegeben werden. Anschließend erfolgt eine Abfrage der ".well-known/smart-configuration" Datei, welche durch den FHIR Endpunkt bereitgestellt werden MUSS. Vorgaben zum Format sind dem Abschnitt [Smart Configuration - TODO Link]() zu entnehmen.

Aufgrund des fehlenden Kontexts zwischen Client und bestätigungsrelevanten System KANN der Client durch Angabe von gewünschten Smart Launch Scopes bestimmen welche Details durch den Authorisierungsserver in der Access Token Response bereitgestellt werden MÜSSEN. Beispielsweise kann äquivalent zum zum EHR Launch der Patienten und/oder Kontakt/Fall Kontext angefordert werden.