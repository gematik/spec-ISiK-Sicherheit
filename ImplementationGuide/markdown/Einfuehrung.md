<img src="https://raw.githubusercontent.com/gematik/api-ISiK/master/images/Gematik_Logo_Flag.jpg" alt="gematik logo" width="400"/>


----

Version: 2.0.0

Datum: 30.06.2022

Status: Aktiv

Realm: Deutschland



# Interoperabler Datenaustausch durch Informationssysteme im Krankenhaus (ISiK)

Die gematik wurde vom Gesetzgeber beauftragt, im Benehmen mit der Deutschen Krankenhausgesellschaft (DKG) und den maßgeblichen Bundesverbänden der Industrie im Gesundheitswesen, verbindliche Standards für den Austausch von Gesundheitsdaten mit Informationssystemen im Krankenhaus zu erarbeiten. 
Für diesen Zweck wurden [FHIR Profile](https://simplifier.net/guide/ImplementierungsleitfadenISiK-Basismodul/Einfuehrung) und ein REST-basiertes Application Programming Interface (API) entwickelt. Die REST-API wird im Wesentlichen [vom FHIR Standard vorgegeben](https://www.hl7.org/fhir/http.html).
Dieser Leitfaden konkretisiert die Anforderungen an eine Autorisierung und Authentifikation für die Absicherung eines ISiK-konformen Endpunkts. Ziel des Leitfadens ist es Drittsoftware zu ermöglichen Abfragen per FHIR RESTful API auf Basis eines zuverlässigen und sicheren Autorisierungsprotokolls durchzuführen. Hierzu werden Teile des etablierten HL7-Standards 'SMART App Launch' innerhalb des Leitfadens profiliert. 

Hersteller bestätigungsrelevanter Systeme sollen durch diesen IG in die Lage versetzt werden, eine konforme Implementierung zu erstellen und das Bestätigungsverfahren der gematik erfolgreich zu absolvieren.

Weitere Informationen siehe [§373 SGB V](https://www.gesetze-im-internet.de/sgb_5/__373.html).

Hinweis: Sowohl für die Implementierung der ISiK-Spezifikation als auch für den Betrieb eines Produktes, das die ISiK-Spezifikation implementiert, ist eine SNOMED-CT-Lizenz notwendig. Diese kann beim [National Release Center für SNOMED CT in Deutschland](https://www.bfarm.de/DE/Kodiersysteme/Terminologien/SNOMED-CT/_node.html) beantragt werden.

**Kontakt**

[ISiK@gematik.de](mailto:ISiK@gematik.de)

Herausgeber

gematik GmbH

[Impressum](https://www.gematik.de/impressum/)

**Gender-Hinweis**

Zugunsten des Leseflusses wird in dieser Publikation meist die
männliche Form verwendet. Wir bitten, dies nicht als Zeichen einer
geschlechtsspezifischen Wertung zu deuten. Diese Variante deckt auch alle
weiteren Geschlechter, neben männlich und weiblich, ab.
