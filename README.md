# RapidAct
A Turkish version of MikeOSS

Yapay zeka destekli hukuki belge asistanı. Next.js frontend, Express backend, Supabase (Auth/Postgres) ve Cloudflare R2 uyumlu nesne depolama üzerine kuruludur.

## Gereksinimler

Node.js 20+, npm, bir Supabase projesi, S3 uyumlu bir bucket (Cloudflare R2 vb.) ve en az bir LLM API anahtarı (Anthropic, Gemini, OpenAI veya OpenRouter). DOC/DOCX dönüştürme için opsiyonel olarak LibreOffice gerekir.

## Kurulum ve Çalıştırma

Bağımlılıkları kurun:

```bash
npm install --prefix backend
npm install --prefix frontend
```

Aşağıdaki Environment Variables bölümüne göre `.env` dosyalarını oluşturun. Ardından veritabanını hazırlayın: yeni bir Supabase veritabanında `backend/schema.sql` dosyasını SQL editöründe çalıştırın; mevcut bir veritabanında ise `backend/migrations/` altındaki tarihli migration dosyalarını sırayla uygulayın.

Uygulamayı başlatın ve tarayıcıda `http://localhost:3000` adresini açın:

```bash
npm run dev --prefix backend
npm run dev --prefix frontend
```

## Environment Variables

### `backend/.env`

`backend/.env.example` dosyasını kopyalayarak başlayın:

```bash
cp backend/.env.example backend/.env
```

| Değişken | Zorunlu | Açıklama |
|---|---|---|
| `PORT` | Hayır | Backend port numarası (varsayılan: `3001`) |
| `FRONTEND_URL` | Evet | Frontend URL'i, CORS için (ör. `http://localhost:3000`) |
| `DOWNLOAD_SIGNING_SECRET` | Evet | İndirme URL'leri için HMAC anahtarı. `openssl rand -hex 32` ile üretin |
| `SUPABASE_URL` | Evet | Supabase proje URL'i |
| `SUPABASE_SECRET_KEY` | Evet | Supabase service role key |
| `R2_ENDPOINT_URL` | Evet | Cloudflare R2 (veya S3 uyumlu) endpoint URL'i |
| `R2_ACCESS_KEY_ID` | Evet | R2 access key |
| `R2_SECRET_ACCESS_KEY` | Evet | R2 secret key |
| `R2_BUCKET_NAME` | Evet | R2 bucket adı |
| `USER_API_KEYS_ENCRYPTION_SECRET` | Evet | Kullanıcı API anahtarlarının şifrelenmesi için secret. `openssl rand -hex 32` ile üretin |
| `GEMINI_API_KEY` | * | Google Gemini API anahtarı |
| `ANTHROPIC_API_KEY` | * | Anthropic (Claude) API anahtarı |
| `OPENAI_API_KEY` | * | OpenAI API anahtarı |
| `OPENROUTER_API_KEY` | * | OpenRouter API anahtarı |
| `COURTLISTENER_API_TOKEN` | Hayır | CourtListener API token (ABD içtihat araması) |
| `COURTLISTENER_BULK_DATA_ENABLED` | Hayır | Lokal CourtListener verisi kullanımı (varsayılan: `false`) |
| `MCP_CONNECTORS_ENCRYPTION_SECRET` | Hayır | MCP connector şifreleme anahtarı (`USER_API_KEYS_ENCRYPTION_SECRET`'e fallback eder) |
| `MCP_OAUTH_CLIENT_ID` | Hayır | MCP connector OAuth client ID |
| `MCP_OAUTH_CLIENT_SECRET` | Hayır | MCP connector OAuth client secret |
| `MCP_OAUTH_DEFAULT_SCOPE` | Hayır | MCP connector varsayılan OAuth scope |

> \* LLM API anahtarlarından en az bir tanesi gereklidir. Kullanıcılar kendi anahtarlarını **Hesap > Modeller & API Anahtarları** bölümünden de ekleyebilir.

### `frontend/.env.local`

`frontend/.env.local.example` dosyasını kopyalayarak başlayın:

```bash
cp frontend/.env.local.example frontend/.env.local
```

| Değişken | Zorunlu | Açıklama |
|---|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Evet | Supabase proje URL'i |
| `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_DEFAULT_KEY` | Evet | Supabase anon/public key |
| `NEXT_PUBLIC_API_BASE_URL` | Evet | Backend API URL'i (ör. `http://localhost:3001`) |

## MCP Connector: Türk Hukuk Verileri

RapidAct, MCP (Model Context Protocol) connector desteği sayesinde dış veri kaynaklarına bağlanabilir. Türk kanun, mevzuat ve içtihatlarına erişim için aşağıdaki MCP sunucusunu connector olarak ekleyebilirsiniz:

```
https://yargi.betaspacestudio.com/mcp
```

Bağlandığında, yapay zeka asistanı konuşma sırasında Türk hukuk mevzuatını, kanun maddelerini ve yargı kararlarını doğrudan sorgulayabilir ve yanıtlarında referans olarak kullanabilir. Connector'ı eklemek için uygulama içindeki MCP bağlantı ayarlarından yukarıdaki URL'yi girin.

## Lisans

[LICENSE](LICENSE) dosyasına bakın.
