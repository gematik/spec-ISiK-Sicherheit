# Schritt 4: Austausch des Autorisierungscodes für ein Zugangstoken

Sowohl für Public als auch Confidential Client erfolgt durch den SMART App Launch ein Authorization Code Flow. Dieser Ablauf wird verwendet um die in [Implicit Grant - OAuth 2.0 Security Best Current Practice](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics-09#section-2.1.2) dokumentierten Sicherheitsrisiken zu minimieren. Im folgenden Schritt wird somit mittels einer vom Client initierte Anfrage der Authorization Code durch ein Access Token ausgetauscht.

## Authentifizierung der Clients

Für Confidential Clients besteht die Pflicht sich gegenüber dem "Token"-Endpunkt des Authorisierungsserver zu authentifizieren. Hierfür MÜSSEN folgende beide Möglichkeiten untersützt werden:

1. HTTP Basic authentication:

Der Client tauscht während der Registrierung (siehe Schritt 1 - Registrierung einer SMART App mit dem bestätigungsrelevanten System) ein Client Secret mit dem bestätigungsrelevanten System aus. Eine Authentifizierung des Client erfolgt per [RFC7617 - The 'Basic' HTTP Authentication Scheme](https://datatracker.ietf.org/doc/html/rfc7617). Als "username" MUSS die Client Id verwendet werden. Das Password MUSS das vorher ausgetauschte Client Secret verwendet werden.

2. JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants