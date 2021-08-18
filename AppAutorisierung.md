# App bittet um Autorisierung

Abhänig davon ob der Client durch einen EHR Launch oder Standalone Launch gestartet wird, existieren unterschiedliche Schritte für die Abfrage eines Authorisierungscodes.

## EHR Launch

Das bestätigungsrelevante System MUSS in der Lage sein durch einen externen Kontextaufruf den Client zu starten. Im Kontext des derzeitig eingeloggten Benutzers wird der Client gestartet, vgl. [EHR Launch / Standalone Launch - TODO LINK](). Der Aufruf des Clients MUSS alle Parameter dokumentiert in [EHR launch sequence](http://build.fhir.org/ig/HL7/smart-app-launch/index.html#ehr-launch-sequence) enthalten. Für alle Endpunkte MÜSSEN per HTTPS (TLS-Verschlüsselung) erreichbar sein. Im Echtbetrieb MUSS die Kommunikation ausschließlich per HTTPS erfolgen. Vorgaben zur einzusetzenden TLS Version, siehe [Sicherheitsaspekte](https://simplifier.net/guide/ImplementierungsleitfadenISiK-Basismodul/UebergreifendeFestlegungenRest).

Es sei drauf hingewiesen, dass jeder EHR Launch mit einem eindeutigen Lauch Parameter zu assozieren ist. Dies ist notwendig innerhalb der SMART authorization sequence die angeforderten [Launch Context Claims](http://build.fhir.org/ig/HL7/smart-app-launch/scopes-and-launch-context.html#scopes-for-requesting-context-data) an den Client zurückzugeben, vgl. Abschnitt [Austausch des Autorisierungscodes für ein Zugangstoken - TODO Link]().

## Standalone launch sequence

Als Einstiegspunkt für einen Standalone Launch MUSS dem Client die Url des FHIR Endpunktes bekannt gegeben werden. Anschließend erfolgt eine Abfrage der ".well-known/smart-configuration" Datei, welche durch den FHIR Endpunkt bereitgestellt werden MUSS. Vorgaben zum Format sind dem Abschnitt [Smart Configuration - TODO Link]() zu entnehmen.

Aufgrund des fehlenden Kontexts zwischen Client und bestätigungsrelevanten System KANN der Client durch Angabe von gewünschten Smart Launch Scopes bestimmen welche Details durch den Authorisierungsserver in der Access Token Response bereitgestellt werden MÜSSEN.