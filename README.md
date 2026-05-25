
## Structure

```
my-ai-api/
├── agents/
│   └── my-agent/
│       └── index.ts       ← Définition de l'agent 21st.dev
├── api/
│   ├── chat.ts            ← Fonction serverless Vercel (/chat)
│   └── health.ts          ← Health check (/health)
├── vercel.json            ← Config Vercel (routes, runtime Node 20)
├── package.json
├── tsconfig.json
├── .env.example
└── README.md
```

## Endpoints

### `POST /chat`

**Headers**
```
x-api-key: ta-cle-secrete
Content-Type: application/json
```

**Body — réponse JSON**
```json
{
  "message": "Explique les closures en JavaScript",
  "sessionId": "sb_xxx"
}
```

**Body — streaming SSE**
```json
{
  "message": "Écris un composant React",
  "stream": true,
  "sessionId": "sb_xxx"
}
```

**Réponse JSON**
```json
{
  "reply": "Une closure est une fonction qui...",
  "sessionId": "sb_abc123"
}
```

---

### `GET /health`
```json
{ "status": "ok", "agent": "my-agent", "timestamp": "..." }
```

---

## Exemples d'appel

### JavaScript / React
```js
// Appel simple
const res = await fetch("https://ton-projet.vercel.app/chat", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "x-api-key": "ta-cle-secrete"
  },
  body: JSON.stringify({ message: "Bonjour !" })
})
const { reply, sessionId } = await res.json()

// Continuer la conversation
const suite = await fetch("https://ton-projet.vercel.app/chat", {
  method: "POST",
  headers: { "Content-Type": "application/json", "x-api-key": "ta-cle-secrete" },
  body: JSON.stringify({ message: "Et en Python ?", sessionId })
})
```

### Python
```python
import requests

def chat(message, session_id=None):
    res = requests.post(
        "https://ton-projet.vercel.app/chat",
        headers={"x-api-key": "ta-cle-secrete"},
        json={"message": message, "sessionId": session_id}
    )
    return res.json()

r = chat("Explique les closures")
print(r["reply"])

# Suite de conversation
r2 = chat("Donne un exemple de code", r["sessionId"])
print(r2["reply"])
```

### cURL
```bash
curl -X POST https://ton-projet.vercel.app/chat \
  -H "x-api-key: ta-cle-secrete" \
  -H "Content-Type: application/json" \
  -d '{"message": "Bonjour !"}'
```
