# Conformance

In Anlehnung an ein CapabilityStatement eines FHIR Ressourcenservers MUSS ein Authorisierungsserver eine SMART Capabilities JSON Datei als ".well-known"-Dokument (vgl. [RFC5785](https://datatracker.ietf.org/doc/html/rfc5785)) anbieten. Clients können auf diese Art und Wiese abfragen u.a. welche Launch-Kontexte, Client-Authentifizierungsmethoden, sowie Scopes seitens des Authorisierungsserver bzw. Ressourcenservers unterstützt werden.

Bestimmte SMART-Funktionalitäten werden zusammengefasst in SMART Capability Sets. Folgende Capability Sets MÜSSEN auf Basis der vorliegenden Spezifikation verpflichtend unterstützt werden. Die Kodierung dieser Capability Sets MUSS den Vorgaben aus [SMART App Launch - 8.2 - FHIR Authorization Endpoint and Capabilities Discovery using a Well-Known Uniform Resource Identifiers (URIs)](https://hl7.org/fhir/smart-app-launch/STU2/conformance.html#using-well-known) entsprechen. Eine Beschreibung der Capability Sets ist ebenfalls der Kernspezifikation zu entnehmen.

### Launch Modes

* ```launch-ehr```
* ```launch-standalone```

### Authorization Methods

* ```authorize-post```

### Client Types
   
* ```client-public```
* ```client-confidential-symmetric```
* ```client-confidential-asymmetric```

### Single Sign-on

* ```sso-openid-connect```

### Launch Context
    
* ```context-ehr-patient```
* ```context-ehr-encounter```
* ```context-standalone-patient```
* ```context-standalone-encounter```

### Permissions
    
* ```permission-offline```
* ```permission-patient```
* ```permission-user```
* ```permission-v2```

Aus den verpflichtenden Capability Sets ergibt sich, dass folgende Angaben zudem in den SMART Capabilities angegeben werden MÜSSEN:

* ```issuer``
* ```jwks_uri``
* ```authorization_endpoint``
* ```grant_types_supported``
* ```token_endpoint``
* ```capabilities``
* ```code_challenge_methods_supported``