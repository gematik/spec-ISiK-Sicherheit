<img src="https://github.com/gematik/spec-ISiK-Sicherheit/blob/3.0.0-rc/ImplementationGuide/Material/Gematik_Logo_Flag.png?raw=true" alt="gematik logo" width="400"/>

----

Version: 3.0.0-rc3

Datum: 25.04.2023

Status: Kommentierung

Realm: Deutschland



# Interoperabler Datenaustausch durch Informationssysteme im Krankenhaus (ISiK)

Die gematik wurde vom Gesetzgeber beauftragt, im Benehmen mit der Deutschen Krankenhausgesellschaft (DKG) und den maßgeblichen Bundesverbänden der Industrie im Gesundheitswesen, verbindliche Standards für den Austausch von Gesundheitsdaten mit Informationssystemen im Krankenhaus zu erarbeiten. 
Für diesen Zweck wurden [FHIR Profile](https://simplifier.net/guide/ImplementierungsleitfadenISiK-Basismodul/Einfuehrung) und ein REST-basiertes Application Programming Interface (API) entwickelt. Die REST-API wird im Wesentlichen [vom FHIR Standard vorgegeben](https://www.hl7.org/fhir/http.html).

Weitere Informationen siehe [§ 373 SGB V](https://www.gesetze-im-internet.de/sgb_5/__373.html).

Hinweis: Sowohl für die Implementierung der ISiK-Spezifikation als auch für den Betrieb eines Produktes, das die ISiK-Spezifikation implementiert, ist eine SNOMED-CT-Lizenz notwendig. Diese kann beim [National Release Center für SNOMED CT in Deutschland](https://www.bfarm.de/DE/Kodiersysteme/Terminologien/SNOMED-CT/_node.html) beantragt werden.

**ISiK-Sicherheit**

Sicherheit hat im Zusammenhang mit einem interoperablen Datenaustausch in und mit Krankenhäusern viele Facetten: Nutzer müssen authentisch sein, Berechtigungen müssen definiert und durchgesetzt werden, Daten müssen gegen Verfälschung geschützt und verfügbar sein, Datenänderungen müssen nachvollziehbar sein etc. Hierzu können Technologien wie z. B. [_SAML_](http://saml.xml.org/saml-specifications) oder [_UMA_](https://docs.kantarainitiative.org/uma/wg/rec-oauth-uma-grant-2.0.html) genutzt werden. Diese Technologien basieren auf einem etablierten und erprobten Stack aus Standards wie [_ReST Representational State Transfer_] (https://restfulapi.net/), [_OAuth2_](https://oauth.net/2/) oder , [_OpenID Connect_](https://openid.net/developers/specs/). Für die Spezifikation von ISiK-Sicherheit wird der Fokus auf die Standards [_OAuth2_](https://oauth.net/2/), [_OpenID Connect_](https://openid.net/developers/specs/) und deren Einsatz im Kontext von SMART on FHIR gelegt. Diese Standards sind domänenunabhängig spezifiziert und müssen daher für den Einsatz im deutschen Gesundheitswesen und ggf. auch für das Zusammenspiel mit dem HL7 FHIR-Standard profiliert werden. FHIR bietet hierzu unterstützende Ressourcen wie z. B. [_Consent_](http://hl7.org/fhir/consent.html), [_AuditEvent_](http://hl7.org/fhir/auditevent.html) oder [_CompartmentDefinition_](http://hl7.org/fhir/compartmentdefinition.html) an, die eine Bindung zwischen FHIR und dedizierten Sicherheitsstandards herstellen können. ISiK-Sicherheit leitet sich hierbei in Teilen von der [_'SMART on FHIR' API_](https://smarthealthit.org/smart-on-fhir-api/) ab, beschränkt sich jedoch auf die für den Ressourcen Server(FHIR Server) relevanten spezifikationen. Ein Im Fall von ISiK sind zusätzlich im deutschen Gesundheitswesen bereits definierte Bausteine der Gesundheitstelematik wie z. B. sektorale Identitätsdienste oder Konnektoren/TI-Gateways zu berücksichtigen, die idealerweise für die Authentifizierung an Patienten- und Zuweiserportalen, den Schutz von Gesundheitsdaten im Krankenhaus, einen Ende-zu-Ende gesicherten Datenaustausch mit Niedergelassenen und andere potenziell ISiK-relevanten Themen genutzt werden sollen.

ISiK-Sicherheit konkretisiert in der aktuellen Stufe 3 zunächst nur die Anforderungen an eine Autorisierung zur Absicherung eines ISiK-konformen FHIR-Endpunkts ("ISiK-Sicherheit Teil: Autorisierung"). In zukünftigen Ausbaustufen werden weitere sicherheitsrelevante Komponenten wie die Teile Audit, Autentisierung und IDPs spezifiziert. Ziel des Leitfadens ist es Drittsoftware zu ermöglichen, Abfragen per FHIR RESTful API auf Basis eines zuverlässigen und sicheren Autorisierungsprotokolls durchzuführen. Hierzu werden Teile des von _SMART Health IT_ und _Boston Childrens Hospital_ auf Basis bestehender Standards definierten [_'SMART on FHIR' API_](https://smarthealthit.org/smart-on-fhir-api/) für den Einsatz im Kontext der bestehenden ISiK-Festlegen profiliert. Das SMART-on-FHIR-API soll die Weiterentwicklung von _Electronic Health Records_ (EHR) zu Plattformen befördern, bei denen - analog zu mobilen Plattformen wie IOS oder Android - Anwendungen aus einem _Store_ in eine solche Plattform eingebracht und dort in einer sicheren Umgebung ausgeführt werden (_"SMART App Launch"_).

In ISiK Stufe 2 wurde die SMART-on-FHIR-API als informativer Bestandteil der Spezifikation an die Hersteller und Krankenhäuser kommuniziert. Teile dieser Spezifikation werden nun normativ jedoch nicht die vollständige Spezifkation der Stufe 2. Zum einfacheren Verständnis welche Komponenten des Moduls ISiK-Sicherheit notmativ sind und welche den Informativen charakter der Stufe 2 behalten, werden einzelnen Dokumente der Spezifikation jeweils eine Statusinformation enthalten welche Bestandteile informativ und welche normativ sind.

Beispiel: 

---
### Informativ
---

Die in ISiK Stufe 3 vorgenommene Profilierung des SMART-on-FHIR-API fokussiert auf die Absicherung der bereits in ISiK Stufe 2 betrachteten FHIR-Ressourcen wie z. B. _Patient_, _Encounter_ oder _Observation_, macht den Herstellern und Krankenhäusern hierbei aber weder Vorgaben zu konkreten Berechtigungsmodellen/-regeln oder dem Zuschnitt der zum Identitäts- und Berechtigungsmanagement (IAM) einzusetzenden Produkte. Der Fokus der Spezifikation auf die FHIR-Ressourcen soll auch dazu dienen den Herstellern und Krankenhäusern keine tiefgreifenden systemischen Änderungen an bestehender Autorisierungs und Autentififizierungsinfrastruktur aufzuerlegen, wie es beispielsweise bei der Implementierung eines Patient Pickers für SMART on FHIR notwendig wäre.

Hersteller von bereits in ISiK Stufe 2 bestätigungsrelevanten Systemen sollen durch diesen Leitfaden in die Lage versetzt werden, eine konforme Erweiterung der bestehenden Implementierung zu erstellen und das Bestätigungsverfahren der gematik für ISiK-Sicherheit in ISiK Stufe 3 erfolgreich zu absolvieren.


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
