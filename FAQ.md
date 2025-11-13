# C4L- Cloud4Log FAQ
## Frequently Ask Questions
### Wie kann auf die API zugegriffen werden?

### **Option 1: Zugriff über Bearer Token (empfohlene Methode)**

1. Im **Frontend** anmelden.  
2. Im **Admin Panel** des **Org Admins** unter **„API-Clients“** einen neuen **Keycloak Client** erstellen.  
3. Dabei auswählen, für welche **Standorte** der Client gelten soll.  

#### 🔧 Client-Typen  
Der Typ des Clients hängt vom **Anwendungsfall** ab:

- **Public Client:** Für Frontend-Anwendungen von Drittanbietern.  
- **Confidential Client:** Für serverseitige Anwendungen, die ein Client-Secret sicher speichern und sich damit beim Authorization Server authentifizieren.  
  *(Flow: Authorization Code mit Client-Authentifizierung)*  
- **M2M Client:** Für Maschinen-zu-Maschinen-Kommunikation ohne Benutzerkontext.  
  *(Flow: Client Credentials)*  

Mit dem erstellten Client kann eine **Authentifizierung an der Keycloak-Instanz** erfolgen, um anschließend per **Bearer Token** mit der API zu kommunizieren.  

👉 Weitere Informationen sind in der Dokumentation zu finden:  
- **[API Documentation: OAuth2](https://github.com/JR-2022/C4L/blob/main/OAuth2-Authentication.md)**  

### **Option 2: Zugriff über Legacy API Key (veraltete Methode)**

Alternativ kann ein **Legacy API Key** erzeugt werden:

1. Als **Org Admin** im **Admin Panel** anmelden.  
2. Zum Bereich **„Legacy-API-Schlüssel“** wechseln.  
3. Einen neuen **Legacy API Key** erstellen und die gewünschten **Standorte** auswählen.  

Dieser **API Key** (auch bekannt als `sessionid` oder `dlssessionid`) kann ebenfalls verwendet werden, um Requests an die API zu senden.  

⚠️ **Hinweis:** Diese Methode ist nur für bestehende Integrationen vorgesehen und wird **ab dem 01.11.2026** durch die **OAuth2-Authentifizierung** ersetzt.  

### Wie kann ich über die neue Authentifizierung Keycloak SSO meines Unternehmens anbinden?
todo

---
### Wo finde ich Informationen über die API?
Here are the available API documentation versions:

- **[API Documentation v2](https://api.cloud4log.com/api-docs/)**  
  Stable version for existing integrations.

- **[API Documentation v3](https://api.cloud4log.com/api-docs-v3/)**  
  New version with **pagination support** and additional features.

- **[Combined API Documentation (v2 + v3)](https://api.cloud4log.com/api-docs-combined/)**  
  Useful if you need to work with both versions at the same time.

If you want to test the APIs through the Swagger-UI or Scalar-UI, you can authorize yourself in two ways:

1. **Legacy API Key** – Enter your API key directly.  
2. **Bearer Token (Keycloak Authentication)** –  
   - Obtain a token using the Keycloak login flow.  
   - **Remove the `Bearer ` prefix** before pasting it into the authorization field.  
   - Keep in mind: **Bearer tokens are valid for only 5 minutes**.

