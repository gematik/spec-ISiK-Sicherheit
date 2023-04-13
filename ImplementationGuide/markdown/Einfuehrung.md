<img src="https://raw.githubusercontent.com/gematik/api-ISiK/master/images/Gematik_Logo_Flag.jpg" alt="gematik logo" width="400"/>


----

Version: 3.0.0-rc1

Datum: 30.06.2022

Status: Aktiv

Realm: Deutschland



# Interoperabler Datenaustausch durch Informationssysteme im Krankenhaus (ISiK)

Die gematik wurde vom Gesetzgeber beauftragt, im Benehmen mit der Deutschen Krankenhausgesellschaft (DKG) und den maßgeblichen Bundesverbänden der Industrie im Gesundheitswesen, verbindliche Standards für den Austausch von Gesundheitsdaten mit Informationssystemen im Krankenhaus zu erarbeiten. 
Für diesen Zweck wurden [FHIR Profile](https://simplifier.net/guide/ImplementierungsleitfadenISiK-Basismodul/Einfuehrung) und ein REST-basiertes Application Programming Interface (API) entwickelt. Die REST-API wird im Wesentlichen [vom FHIR Standard vorgegeben](https://www.hl7.org/fhir/http.html).

Weitere Informationen siehe [§373 SGB V](https://www.gesetze-im-internet.de/sgb_5/__373.html).

Hinweis: Sowohl für die Implementierung der ISiK-Spezifikation als auch für den Betrieb eines Produktes, das die ISiK-Spezifikation implementiert, ist eine SNOMED-CT-Lizenz notwendig. Diese kann beim [National Release Center für SNOMED CT in Deutschland](https://www.bfarm.de/DE/Kodiersysteme/Terminologien/SNOMED-CT/_node.html) beantragt werden.

**ISiK-Sicherheit**

Sicherheit hat im Zusammenhang mit einem interoperablen Datenaustausch in und mit Krankenhäusern viele Facetten: Nutzer müssen authentisch sein, Berechtigungen müssen definiert und durchgesetzt werden, Daten müssen gegen Verfälschung geschützt und verfügbar sein, Datenänderungen müssen nachvollziehbar sein etc. Hierzu können Standards wie z. B. OAuth2, SAML, openID Connect, syslog, UMA oder XACML genutzt werden. Diese Standards sind domänenunabhängig spezifiziert und müssen daher für den Einsatz im Gesundheitswesen und ggf. auch für das Zusammenspiel mit dem FHIR Standard profiliert werden. Der FHIR Standard bietet hierzu unterstützende Ressourcen wie z. B. Consent, AuditEvent oder CompartmentDefinition an, die eine Bindung zwischen FHIR und dedizierten Sicherheitsstandards herstellen können. Im Fall von ISiK sind zusätzlich im deutschen Gesundheitswesen bereits definierte Bausteine der Gesundheitstelematik wie z. B. sektorale Identitätsdienste oder Konnektoren/TI-Gateways zu berücksichtigen, die idealerweise für die Authentifizierung an Patienten- und Zuweiserportalen, den Schutz von Gesundheitsdaten im Krankenhaus, einen Ende-zu-Ende gesicherten Datenaustausch mit Niedergelassene und andere potenziell ISiK-relevanten Themen genutzt werden können.

ISiK-Sicherheit konkretisiert in der aktuellen Stufe 3 zunächst nur die Anforderungen an eine Autorisierung für die Absicherung eines ISiK-konformen FHIR-Endpunkts ("ISiK-Autorisierung"). Ziel des Leitfadens ist es Drittsoftware zu ermöglichen, Abfragen per FHIR RESTful API auf Basis eines zuverlässigen und sicheren Autorisierungsprotokolls durchzuführen. Hierzu werden Teile des HL7-Standards 'SMART on FHIR' - insbesondere der für die Autorisierung menschlicher Akteure definierte 'SMART App Launch' - profiliert. Die vorgenommene Profilierung fokussiert auf die Absicherung der bereits in ISiK Stufe 2 betrachteten FHIR-Ressourcen wie z. B. Patient, Encounter oder Observation, macht den Herstellern und Krankenhäusern hierbei aber weder Vorgaben zu konkreten Berechtigungsmodellen/-regeln oder dem Zuschnitt der zum Identitäts- und Berechtigungsmanagement (IAM) einzusetzenden Produkte. 

Hersteller von bereits in ISiK Stufe 2 bestätigungsrelevanten Systemen sollen durch diesen Leitfaden in die Lage versetzt werden, eine konforme Erweiterung der bestehenden Implementierung zu erstellen und das Bestätigungsverfahren der gematik für ISiK-Sicherheit in ISiK STufe 3 erfolgreich zu absolvieren.


**Kontakt**

Bringen Sie allgemeine Fragen und Anmerkungen gerne über unser Anfrageportal ein: [Anfragen ISiK + ISiP](https://service.gematik.de/servicedesk/customer/portal/16)

Falls Sie keinen Zugang zum Anfrageportal haben und dieses nutzen wollen, senden Sie uns bitte eine Nachricht an die Adresse isik [ at ] gematik.de mit dem Betreff "Portalzugang".

**Herausgeber**

gematik GmbH

[Impressum](https://www.gematik.de/impressum/)

**Gender-Hinweis**

Zugunsten des Leseflusses wird in dieser Publikation meist die
männliche Form verwendet. Wir bitten, dies nicht als Zeichen einer
geschlechtsspezifischen Wertung zu deuten. Diese Variante deckt auch alle
weiteren Geschlechter, neben männlich und weiblich, ab.
