## 🔐 OAuth 2.0 – Secure Authorization

---

### 🧠 What is OAuth?

OAuth (Open Authorization) is a **secure authorization protocol** that allows apps to access **user data** without sharing login credentials (like passwords).

> 🔑 It lets users **grant limited access** to their data on another service (like Google, GitHub, or Facebook).

---

### 🔄 OAuth Roles

| Role                     | Description                            |
| ------------------------ | -------------------------------------- |
| **Resource Owner**       | The user who owns the data (you)       |
| **Client**               | The app requesting access (your app)   |
| **Resource Server**      | The API/data provider (Google, GitHub) |
| **Authorization Server** | Issues access tokens and handles login |

---

### 🧭 Basic OAuth Flow (Authorization Code Flow)

1. **User clicks "Login with Google"**
2. App (Client) redirects to **Google's Auth Server**
3. User logs in and **approves** the app’s request
4. Google sends back an **Authorization Code**
5. The app exchanges that code for an **Access Token**
6. App uses that token to access the **user’s data** from the API

---

### 🧾 OAuth Tokens

#### ✅ Authorization Code

- Temporary code sent to the client to be exchanged for an access token

#### 🔑 Access Token

- Short-lived
- Used to **access protected resources** (e.g., get user's email)

#### 🔁 Refresh Token

- Optional
- Used to **get a new access token** without logging in again

---

### 🔐 Why Use OAuth? (Real Benefits)

**Security:**

- 🔐 **More secure** – Your app never sees the user's password
- ✅ Users **authorize** limited access without revealing credentials
- 🛡️ If your app gets hacked, the attacker doesn't have passwords

**User Experience:**

- 🎯 **Single Sign-On (SSO)** – Users can login with Google/GitHub instead of passwords
- ⚡ **No password reset needed** – Stored/managed by trusted provider (Google, etc.)
- 🎯 **Scoped access** – Only request what you need (email, profile, calendar, etc.)
- 🔄 **Refresh tokens** – Smooth experience without constant re-login

**For Your App:**

- ✅ Don't need to store passwords (security liability)
- ✅ Leveraging trusted identity providers
- ✅ Legal compliance – Reduce PII exposure

---

### 🛠 Common OAuth Providers

| Provider | Use Case                 |
| -------- | ------------------------ |
| Google   | Gmail, Calendar, Profile |
| GitHub   | Developer tools, repos   |
| Facebook | Profile, photos          |
| Discord  | Bots and user info       |
| Spotify  | Music & playlists        |

---

### 🌐 Real-World Example (Google Login Flow)

```text
1. User lands on your app
   ↓
2. User clicks "Login with Google"
   ↓
3. Your app redirects to Google:
   https://accounts.google.com/o/oauth2/v2/auth?
   client_id=YOUR_APP_ID&
   redirect_uri=YOUR_CALLBACK_URL&
   scope=email%20profile
   ↓
4. User logs in to Google (if not already)
   ↓
5. User sees "MyApp wants access to your email & profile"
   User clicks "Allow"
   ↓
6. Google redirects back to your app with an Authorization Code:
   https://yourapp.com/callback?code=AUTH_CODE_123&state=STATE
   ↓
7. Your server exchanges the code for an Access Token:
   POST to Google with:
   - client_id
   - client_secret (kept secret on server!)
   - code (from step 6)
   ↓
8. Google responds with Access Token:
   {
     "access_token": "ya29.xxx...",
     "expires_in": 3599,
     "refresh_token": "1/xxx..."
   }
   ↓
9. Your app stores the token
   ↓
10. User is logged in! You can now:
    - Get user's email: GET /userinfo with token
    - Get user's profile: GET /profile with token
    - Keep user session
```

---

### 💻 Simple Implementation Example (Node.js/Express)

```js
const express = require("express");
const axios = require("axios");

const app = express();

// Step 1: User clicks "Login with Google"
// Browser redirects to: /auth/google
app.get("/auth/google", (req, res) => {
  const googleAuthUrl = `https://accounts.google.com/o/oauth2/v2/auth?
    client_id=${process.env.GOOGLE_CLIENT_ID}&
    redirect_uri=http://localhost:3000/auth/google/callback&
    scope=email%20profile&
    response_type=code`;

  res.redirect(googleAuthUrl);
});

// Step 2: Google redirects back with authorization code
app.get("/auth/google/callback", async (req, res) => {
  const code = req.query.code;

  // Step 3: Exchange code for access token
  const tokenResponse = await axios.post(
    "https://oauth2.googleapis.com/token",
    {
      code,
      client_id: process.env.GOOGLE_CLIENT_ID,
      client_secret: process.env.GOOGLE_CLIENT_SECRET, // Secret on server!
      redirect_uri: "http://localhost:3000/auth/google/callback",
      grant_type: "authorization_code",
    },
  );

  const accessToken = tokenResponse.data.access_token;

  // Step 4: Get user info with access token
  const userResponse = await axios.get(
    "https://www.googleapis.com/oauth2/v1/userinfo",
    { headers: { Authorization: `Bearer ${accessToken}` } },
  );

  const user = userResponse.data; // { email, name, picture, ... }

  // Step 5: Create session / store user
  req.session.user = user;

  // Step 6: Redirect to dashboard
  res.redirect("/dashboard");
});

app.listen(3000);
```

**Key security point:** The `client_secret` **never** goes to the browser. Only your server has it. This prevents attackers from forging tokens.

---

### 💬 Common Terms

- **Scope** – What kind of access the app is asking for (e.g. `email`, `profile`)
- **Consent screen** – What the user sees before approving
- **Redirect URI** – Where the authorization server sends the response

---

### 🧪 Alternatives & Add-ons

- **OpenID Connect (OIDC)** – Layer on top of OAuth for authentication (who the user is)
- **PKCE** – Used in mobile/public apps to protect authorization code flow

---

### 🚫 What OAuth Is Not

- ❌ **Not an authentication protocol** – But can be combined with OpenID Connect for login
- ❌ **Not suitable for apps** that need full control without user involvement
- ❌ **Not a substitute for passwords** – Still need passwords for your account on the OAuth provider

---

### 🔄 OAuth vs Alternatives

| Approach                  | Use Case                                 | Security     | Complexity |
| ------------------------- | ---------------------------------------- | ------------ | ---------- |
| **OAuth**                 | Third-party login, delegated permissions | ✅ High      | Medium     |
| **Simple password login** | Basic internal apps                      | ❌ Lower     | Low        |
| **SAML**                  | Enterprise SSO                           | ✅ Very High | High       |
| **OpenID Connect**        | OAuth + authentication                   | ✅ Very High | Medium     |
| **JWT tokens**            | API authentication                       | ✅ High      | Low-Medium |

**Recommendation:**

- **Personal projects:** Simple password login OK (use HTTPS!)
- **Web apps with OAuth providers:** Use OAuth (Google, GitHub)
- **Enterprise apps:** Use SAML or OpenID Connect
- **APIs:** Use JWT tokens or OAuth 2.0 Bearer tokens

---

Let me know if you want a diagram version or `.md` file export!
