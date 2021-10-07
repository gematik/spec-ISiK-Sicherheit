# Schritt 6: Refresh Token & Revocation

Die Gültigkeitsdauer eines Access-Tokens kann beliebig durch das bestätigungsrelevante System gewählt werden. Mit Hinblick auf [RFC6819 -  OAuth 2.0 Threat Model and Security Considerations - Limited Access Token Lifetime](https://datatracker.ietf.org/doc/html/rfc6819#section-3.1.2) SOLLTE die Gültigkeitsdauer beschränkt werden auf Minuten oder Stunden. Das Gültikgkeitsdatum wird im Access Token durch den Parameter "expires_in" angegeben. Alternativ kann die Gültigkeit des Tokens über den {{pagelink:TokenIntrospection, text:Introsepection-Endpunkt}} abgefragt werden.

Um eine häufige Authentifizierung des Benutzers und/oder des Clients zu vermeiden MUSS das besätigungsrelevante System die Austellung von Refresh Tokens unterstützen.

Folgende Anforderungen aus [SMART App Launch - 2.0.12 - Refresh access token](http://build.fhir.org/ig/HL7/smart-app-launch/app-launch.html#refresh-access-token) sind zu beachten:

- Der "offline_access" Scope MUSS untersützte werden, der "online_access" Scope SOLLT untersützt werden.
- Ein Refresh Token MUSS an die anforderende Client Id gebunden werden, d.h. eine abweichende Client Id (übermittelt durch die Authorisierung des Clients) DARF NICHT verwendenbar sein um ein neues Access Token auszustellen.
- Der Authorisierungsserver MUSS sicherstellen bei der Ausstellung eines neuen Access Tokens beachten, dass nur ein Subset oder die gleiche Menge an Scopes erlaubt werden im Vergleich zum originalen Access Tokens. Fordert der Client neue Scopes an, SOLLTE das System die Menge aller Scopes einschränken auf die zuvor erlaubten Scopes.
- Der Launch Context MUSS zurückgegeben werden, falls die entsprechenden Scopes durch den Client angefordert wurden oder für das im originale Access Token angefordert wurden.

Alle verpflichtenden Implementierungsdetails aus [SMART App Launch - 2.0.12 - Refresh access token](http://build.fhir.org/ig/HL7/smart-app-launch/app-launch.html#step-7-refresh) MÜSSEN durch den Authorisierungsserver unterstützt werden.