<p align="center">
  <a href="https://github.com/compnew2006/MetaAI-Free-Hermes-Agent/stargazers">
    <img src="https://img.shields.io/github/stars/compnew2006/MetaAI-Free-Hermes-Agent?style=for-the-badge&logo=github&label=نجم&color=f5a623" alt="نجم هذا المشروع">
  </a>
</p>

<h1 align="center">🤖 MetaAI Free — شغّل Muse Spark من Meta مجاناً عبر Hermes Agent</h1>

<p align="center">
  <b>بدون مفاتيح API · بدون حدود · بدون تكلفة</b><br>
  <a href="https://github.com/nousresearch/hermes-agent">Hermes Agent</a> + <a href="https://meta.ai">Meta AI (Muse Spark)</a> + <a href="https://go.dev">Go API</a> + <a href="https://react.dev">React 19</a>
</p>

<p align="center">
  <img src="docs/images/demo-multiplatform.png" alt="Meta AI — واجهة ويب ↔ تليجرام ↔ واتساب عبر Hermes Agent" width="800">
  <br><em>نفس نموذج Meta AI، يرد على تليجرام وواتساب والويب — كل ذلك من سيرفرك الخاص</em>
</p>

> 🌐 **English** — [Read English Documentation](README.md)

> 💡 **إيه هذا؟** خادم Go مُستضاف ذاتياً يلفّ Meta AI (المُشغّل بـ Muse Spark) في نقطة نهاية متوافقة مع OpenAI. وصّله بـ **Hermes Agent** وتحدّث مع نموذج LLM الخاص بـ Meta مجاناً من تليجرام أو واتساب أو أي منصة مراسلة — بدون مفتاح API.

<details>
<summary>⭐ <b>عجبك المشروع؟ نجمّه على GitHub لدعمنا!</b></summary>
<br>

لو المشروع وفّر عليك فلوس API أو سهّل عليك شغلك، نجمه ⭐ هي أحسن طريقة تقول فيها شكراً! كمان بتساعد الناس التانية تكتشفه.

[⭐ نجم المشروع على GitHub →](https://github.com/compnew2006/MetaAI-Free-Hermes-Agent/stargazers)

</details>

> [!WARNING]
> **لأغراض تعليمية فقط:** تم إنشاء هذا المشروع خصيصاً للأغراض التعليمية والبحثية والتطويرية فقط. هذا دمج غير رسمي ويجب عدم استخدامه في بيئات الإنتاج التجارية دون احترام إرشادات وشروط الخدمة الرسمية لشركة Meta.

🎯 [نظرة عامة على المشروع](#-نظرة-عامة-على-المشروع) • 🏗️ [المعمارية](#-المعمارية) • 🔗 [التثبيت مع Hermes Agent](#-التثبيت-مع-hermes-agent) • 📦 [المتطلبات](#-المتطلبات) • 🔐 [إعداد الكوكيز](#-جلب-الكوكيز-من-meta-ai-خطوة-بخطوة) • 🚀 [أوضاع التشغيل](#-التشغيل--3-أوضاع) • 🔧 [واجهة الـ REST API](#-واجهة-الـ-rest-api-reference)

---

## 🔍 نظرة عامة على المشروع

* **[smart-studio](smart-studio/)**: واجهة تصميم وتسويق حديثة (React 19) توفر 11 استوديوًا متخصصًا للعلامات التجارية، التصوير الفوتوغرافي، الفيديو، التعليقات الصوتية، الكامباينز، وتحليل السوق.
* **[metaai-go](metaai-go/)**: خادم API سريع للغاية بلغة Go لـ Meta AI. يستقبل الطلبات من واجهة SMART Studio ويتواصل مباشرة مع Meta AI باستخدام مصادقة الكوكيز.

> [!NOTE]
> **تفاصيل التكامل (Integration):**
> تمر كل مكالمات الذكاء الاصطناعي في SMART Studio عبر خدمة مركزية (`smart-studio/services/geminiService.ts` والتي تقوم بعمل re-export من `aiService.ts`)، وتتصل مباشرة بـ REST API الخاص بـ `metaai-go`.

---

## 🌟 القدرات الأساسية

| الاستوديو / الميزة | الوصف | المحرك | الحالة |
| :--- | :--- | :--- | :--- |
| 💬 **الدردشة الذكية** | مدعومة بـ Muse Spark مع ميزة البحث الفوري من Bing | Meta AI | ✅ تعمل |
| 🎨 **Creator Studio** | توليد صور المنتجات باتجاهات مخصصة | Meta AI | ✅ تعمل |
| 🎬 **Video Studio** | توليد مقاطع فيديو سينمائية من النصوص أو الصور المرجعية | Meta AI | ✅ تعمل |
| 🔍 **تحليل الصور** | وصف الصور واستخراج الـ prompts وتدقيق هوية العلامة التجارية | Meta AI | ✅ تعمل |
| 📢 **Plan Studio** | توليد حملات تسويقية مكونة من 9 منشورات بالعامية المصرية | Meta AI | ✅ تعمل |
| 🎨 **Branding Studio** | توليد أفكار وتنوعات للشعارات واستخراج لوحة ألوان الهوية | Meta AI | ✅ تعمل |
| 🗣️ **Voice Over Studio** | تحويل النصوص إلى تعليق صوتي (TTS) | Gemini | 🔶 يتطلب مفتاح API |

---

## 🏗️ المعمارية وتدفق الطلب (Flow)

```
┌────────────────────────────────────────────────────────────────┐
│  Browser (SMART Studio React SPA)                              │
│                                                                │
│  components/*.tsx ──imports──▶ services/geminiService.ts      │
│    (11 studios)                   │                            │
│                                   ▼ (re-export shim)            │
│                          services/aiService.ts                  │
│                                   │                            │
│                                   ▼                            │
│                          services/metaaiClient.ts               │
│                       (single network layer: fetch + Bearer)    │
└────────────────────────────────────────┬───────────────────────┘
                                         │ HTTPS / same-origin
                                         ▼
┌────────────────────────────────────────────────────────────────┐
│  metaai-go REST Server  (Go binary)                             │
│                                                                 │
│  /chat  /analyze  /upload  /image  /image/fetch  /video*       │
│      rest/handlers.go  rest/analyze_handler.go                 │
│                                                                 │
│  + embedded SMART Studio SPA at /  (في prod build)               │
└────────────────────────────────────────┬───────────────────────┘
                                         │ WebSocket + GraphQL
                                         │ (cookies + access token)
                                         ▼
┌────────────────────────────────────────────────────────────────┐
│  Meta AI  (meta.ai)                                             │
│  - الصور المولدة على scontent-arn2-1.xx.fbcdn.net             │
│  - الفيديوهات المولدة على video-*.xx.fbcdn.net                 │
└────────────────────────────────────────────────────────────────┘
```

---

## 🔗 التثبيت مع Hermes Agent (خطوة بخطوة)

القسم ده بيشرح إزاي تثبّت المشروع ده وتوصّله بـ **[Hermes Agent](https://github.com/nousresearch/hermes-agent)** عشان تستخدم Meta AI كمزوّد LLM مجاني داخل Hermes على **تليجرام، واتساب، ديسكورد، أو أي منصة Hermes يدعمها**.

> [!IMPORTANT]
> محتاج **سيرفر/VPS** له IP عام (Linux أو Mac أو Windows). Hermes Agent بيتصل بسيرفرك عن طريق الشبكة — مش بيشتغل الملف التنفيذي محلياً.

### إزاي بيتشتغل؟

```
┌──────────────┐     HTTPS      ┌──────────────────────┐     WebSocket      ┌──────────┐
│  Hermes Agent │ ◄────────────► │  metaai-go Server    │ ◄────────────────► │ Meta AI  │
│  (أي جهاز)    │   :8787/v1     │  (سيرفرك/VPS)        │   cookies+token    │ meta.ai  │
│  تليجرام/واتس│                │  متوافق مع OpenAI     │                   │          │
└──────────────┘                └──────────────────────┘                   └──────────┘
```

Hermes Agent بيتكلم مع `metaai-go` عن طريق **البروكسي المتوافق مع OpenAI** (`/v1/chat/completions`). يعني Hermes بيتعامل مع Meta AI كمزوّد LLM عادي.

### الخطوة 1: تثبيت المتطلبات الأساسية

<details>
<summary><b>🐧 Linux (Ubuntu/Debian)</b></summary>

```bash
# تثبيت Go 1.21+
sudo apt update
sudo apt install -y golang-go build-essential

# تثبيت Node.js 18+ (عن طريق NodeSource)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# التحقق
go version    # go1.21+
node -v        # v18+
npm -v         # 9+
```

</details>

<details>
<summary><b>🍎 macOS</b></summary>

```bash
# التثبيت عن طريق Homebrew
brew install go node

# التحقق
go version    # go1.21+
node -v        # v18+
npm -v         # 9+
```

</details>

<details>
<summary><b>🪟 Windows</b></summary>

```powershell
# تثبيت Go: حمّل من https://go.dev/dl/ (ملف .msi)
# تثبيت Node.js: حمّل من https://nodejs.org/ (ملف LTS .msi)

# أو عن طريق Chocolatey/Winget:
winget install GoLang.Go
winget install OpenJS.NodeJS.LTS

# التحقق (في PowerShell أو CMD)
go version
node -v
npm -v
```

> [!NOTE]
> **مستخدمي Windows:** أوامر `make` مش هتشتغل بصورة طبيعية. تقدر:
> - تستخدم **WSL2** (Windows Subsystem for Linux) — المُوصى به، تجربة Linux كاملة
> - تستخدم **Git Bash** (بيجي مع [Git for Windows](https://gitforwindows.org/))
> - تستبدل `make run-rest` بـ `go run ./cmd/metaai-rest` وغيرها

</details>

### الخطوة 2: استنساخ المشروع والإعدادات

```bash
# استنساخ المشروع
git clone https://github.com/compnew2006/MetaAI-Free-Hermes-Agent.git
cd MetaAI-Free-Hermes-Agent

# إنشاء ملف .env للخادم الخلفي
cd metaai-go
cp .env.example .env
nano .env   # أو: code .env  (VS Code)  أو: notepad .env (Windows)
```

املاً كوكيز Meta AI (شوف [قسم إعداد الكوكيز](#-جلب-الكوكيز-من-meta-ai-خطوة-بخطوة) ادناه لإزاي تجيبهم):

```env
META_AI_DATR=your_datr_cookie_here
META_AI_ECTO_1_SESS=your_ecto_1_sess_cookie_here
META_AI_ACCESS_TOKEN=ecto1:...n

# إعدادات البروكسي (اللي Hermes بيتصل فيه)
META_AI_PROXY_ADDR=:8787
META_AI_PROXY_TOKEN=your-s...token### الخطوة 3: تشغيل السيرفر

**الخيار أ: وضع التطوير (مع إعادة التحميل التلقائي)**
```bash
cd metaai-go
make run-rest        # REST API على المنفذ :8000

# في نافذة تيرمينال جديدة — تشغيل البروكسي المتوافق مع OpenAI على :8787
cd metaai-go
go run ./cmd/metaai-server -addr :8787 -token "your-secret-api-key"
```

**الخيار ب: الإنتاج (تشغيل في الخلفية)**
```bash
cd metaai-go

# بناء ملف البروكسي التنفيذي
go build -o metaai-proxy ./cmd/metaai-server

# تشغيل في الخلفية
nohup ./metaai-proxy -addr :8787 -token "your-secret-api-key" > proxy.log 2>&1 &

# التحقق أنه يعمل
curl http://localhost:8787/v1/models
```

**الخيار ج: خدمة systemd (Linux فقط — مُوصى به للـ VPS)**
```bash
sudo tee /etc/systemd/system/metaai-proxy.service > /dev/null << 'EOF'
[Unit]
Description=MetaAI Proxy (متوافق مع OpenAI)
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/path/to/MetaAI-Free-Hermes-Agent/metaai-go
ExecStart=/path/to/MetaAI-Free-Hermes-Agent/metaai-go/metaai-proxy -addr :8787 -token "your-secret-api-key"
Restart=always
RestartSec=5
EnvironmentFile=/path/to/MetaAI-Free-Hermes-Agent/metaai-go/.env

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable metaai-proxy
sudo systemctl start metaai-proxy
sudo systemctl status metaai-proxy
```

### الخطوة 4: ربطه بـ Hermes Agent

دلوقتي هنعمل **Hermes Agent** يستخدم سيرفر Meta AI ك مزوّد LLM مخصص.

افتح ملف إعدادات Hermes (`~/.hermes/config.yaml`) وأضف/عدّل:

```yaml
# مزوّد مخصص — موجّه لسيرفر MetaAI البروكسي
custom_providers:
  - name: smart-studio
    models:
      meta-ai:
        supports_vision: true
      meta-ai-analyze:
        supports_vision: true

# تسجيل المزوّد مع عنوان URL ومفتاح API
providers:
  smart-studio:
    base_url: http://YOUR_SERVER_IP:8787/v1
    api_key: your-secret-api-key   # نفس META_AI_PROXY_TOKEN

# (اختياري) استخدام Meta AI كنموذج افتراضي
model:
  default: meta-ai
  provider: smart-studio
```

> [!TIP]
> - استبدل `YOUR_SERVER_IP` بـ IP الـ VPS العام (مثلاً `http://123.45.67.89:8787/v1`)
> - لو Hermes شغال **على نفس الجهاز** كبالبروكسي، استخدم `http://localhost:8787/v1`
> - لو أنت وراء NAT/firewall، محتاج تعمل port-forward لـ `8787` أو تستخدم reverse proxy (nginx/Caddy)
> - عشان HTTPS، حط reverse proxy (nginx + Let's Encrypt) قدام `:8787`

### الخطوة 5: التحقق

```bash
# اختبار البروكسي مباشرة
curl http://localhost:8787/v1/chat/completions \
  -H "Authorization: Bearer your-secret-api-key" \
  -H "Content-Type: application/json" \
  -d '{"model":"meta-ai","messages":[{"role":"user","content":"مرحبا!"}]}'

# تشغيل أو إعادة تشغيل Hermes
hermes start
# ابدأ الدردشة مع Meta AI على تليجرام أو واتساب أو أي منصة موصولة!
```

### الأسئلة الشائعة

<details>
<summary><b>❓ هل أقدر أشغّله على ويندوز بدون سيرفر؟</b></summary>

**أيوه!** تقدر تشغّل كل حاجة محلياً على ويندوز:

1. ثبّت [Go](https://go.dev/dl/) و [Node.js](https://nodejs.org/)
2. استنسخ المشروع وشغّل البروكسي: `go run ./cmd/metaai-server -addr :8787 -token "my-token"`
3. ثبّت Hermes Agent على ويندوز واضبط `base_url: http://localhost:8787/v1`

> ملاحظة: Hermes Agent رسمياً يدعم Linux/macOS. لويندوز، استخدم **WSL2** لأفضل تجربة. Hermes يقدر يتصل ببروكسي شغال على ويندوز عن طريق `http://YOUR_WINDOWS_IP:8787/v1`.

</details>

<details>
<summary><b>❓ إزاي أخليه متاح على الإنترنت؟</b></summary>

لو Hermes Agent على جهاز مختلف عن البروكسي:

1. **VPS (أسهل):** انشر البروكسي على VPS له IP عام → Hermes يتصل بـ `http://VPS_IP:8787/v1`
2. **سيرفر منزلي + NAT:** اعمل port-forward لـ `8787` على الراوتر → استخدم IP العام أو DDNS
3. **Cloudflare Tunnel (مجاني، بدون port forwarding):**
   ```bash
   cloudflared tunnel --url http://localhost:8787
   # استخدم رابط الـ tunnel كـ base_url في Hermes
   ```
4. **Tailscale (VPN):** ثبّت Tailscale على الجهازين → استخدم IP الـ Tailscale

</details>

<details>
<summary><b>❓ الكوكيز خلصت صلاحيتها — إزاي أجددّها؟</b></summary>

`ecto_1_sess` بتنتهي صلاحيته دورياً. لما يحصل كده:
1. روح [meta.ai](https://www.meta.ai) في متصفحك
2. افتح DevTools → Application → Cookies → انسخ الـ `ecto_1_sess` الجديد
3. حدّث ملف `.env` وأعد تشغيل البروكسي

</details>

---

## 📦 المتطلبات

| الأداة | الإصدار المطلوب | الغرض منها |
| :--- | :--- | :--- |
| **Go** | 1.21+ | تشغيل وبناء خادم `metaai-go` الخلفي |
| **Node.js** | 18+ | تثبيت الحزم وبناء واجهة `smart-studio` |
| **npm** | 9+ | مدير حزم الواجهة الأمامية وتبعيات Go UI |
| **حساب Meta AI** | - | حساب مجاني مسجل الدخول على `meta.ai` |

---

## 🔐 جلب الكوكيز من Meta AI (خطوة بخطوة)

للمصادقة مع Meta AI بدون مفاتيح API، يجب استخراج الكوكيز من جلسة المتصفح الخاصة بك.

1. افتح متصفحك وتوجه إلى [meta.ai](https://www.meta.ai) وسجل الدخول.
2. افتح أدوات المطورين DevTools بالضغط على **F12** (أو انقر بزر الفأرة الأيمن ← Inspect).
3. انتقل إلى علامة تبويب **Application** ← **Storage** ← **Cookies** ← `https://www.meta.ai`.
4. انسخ قيم الكوكيز التالية:
   * `datr` (مطلوب - رمز جهاز طويل المدى)
   * `ecto_1_sess` (مطلوب - رمز الجلسة، ينتهي صلاحيته دورياً ويجب تحديثه)
   * `rd_challenge` (موصى به - يتجاوز التحقق من القيود الإقليمية)
   * `abra_sess` (اختياري - يحسن التوافق في بعض المناطق)

---

## 🛠️ الإعدادات (`.env`)

### إعدادات الخادم الخلفي (`metaai-go/.env`)
أنشئ ملف `.env` داخل مجلد `metaai-go`:
```env
META_AI_DATR=your_datr_cookie_here
META_AI_ECTO_1_SESS=your_ecto_1_sess_cookie_here

# إعدادات موصى بها
META_AI_RD_CHALLENGE=your_rd_challenge_cookie
META_AI_DPR=1
META_AI_WD=1837x1240
META_AI_PS_L=1
META_AI_PS_N=1

# إعدادات السيرفر الاختيارية
META_AI_REST_ADDR=:8000
META_AI_REST_TOKEN=smart-...n### إعدادات الواجهة الأمامية (`smart-studio/.env`)
أنشئ ملف `.env` داخل مجلد `smart-studio`:
```env
VITE_METAAI_URL=http://localhost:8000
VITE_METAAI_TOKEN=smart-...n   # مطلوب فقط لـ Voice Over Studio (TTS)
```

---

## 🚀 التشغيل — 3 أوضاع

### 1. وضع التطوير Dev Mode (موصى به أثناء العمل)
يقوم بتشغيل خادم التطوير Vite للواجهة وخادم Go للـ API بشكل متوازٍ.
```bash
# تشغيل الخادم الخلفي على المنفذ :8000
cd metaai-go
make run-rest

# في نافذة تيرمينال جديدة: تشغيل الواجهة الأمامية على المنفذ :3000
cd smart-studio
npm install
npm run dev
```

أو إذا كان لديك أداة `air` مثبتة للـ hot-reload، يمكنك تشغيلهما بأمر واحد:
```bash
cd metaai-go
make run-dev
```

### 2. بناء ملف تنفيذي موحد للإنتاج (Single Binary Production Build)
يدمج واجهة SMART Studio وخادم Go API في ملف تنفيذي واحد سريع وجاهز للنشر.
```bash
cd metaai-go
make build-prod

# تشغيل الخادم الموحد (يمكن الوصول للتطبيق من http://localhost:8000)
META_AI_REST_TOKEN=smart-...oken ./bin/metaai-rest
```

### 3. التشغيل في الخلفية (للخوادم البعيدة / VPS)
```bash
cd metaai-go
go build -o /tmp/metaai-rest ./cmd/metaai-rest
nohup /tmp/metaai-rest > /tmp/metaai-rest.log 2>&1 &

# متابعة السجلات والـ logs
tail -f /tmp/metaai-rest.log
```

---

## 🔧 واجهة الـ REST API Reference

يجب أن تحتوي جميع الطلبات المرسلة على الرمز المميز كـ Bearer Auth في الـ Header:
```text
Authorization: Bearer <META_...n
| نقطة النهاية (Endpoint) | الطريقة | الوصف | الحالة |
| :--- | :--- | :--- | :--- |
| `/healthz` | GET | فحص حالة الخادم (بدون مصادقة) | ✅ يعمل |
| `/chat` | POST | إرسال رسائل الدردشة إلى Muse Spark | ✅ يعمل |
| `/upload` | POST | رفع الصور المرجعية (multipart form-data) | ✅ يعمل |
| `/analyze` | POST | تحليل الصور التي تم رفعها | ✅ يعمل |
| `/image` | POST | توليد الصور من النصوص | ✅ يعمل |
| `/image/fetch` | GET | جلب رابط صورة fbcdn وتحويلها إلى Base64 | ✅ يعمل |
| `/video/async` | POST | بدء عملية توليد فيديو غير متزامن | ✅ يعمل |
| `/video/jobs/{id}` | GET | التحقق من حالة عملية توليد الفيديو الجارية | ✅ يعمل |

### مثال: توليد صورة باستخدام cURL
```bash
curl -X POST http://localhost:8000/image \
  -H "Authorization: Bearer *** \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Cyberpunk cityscape at night", "orientation": "LANDSCAPE"}'
```

---

## 🌟 هيكل المشروع

```text
MetaAI-Free-Hermes-Agent/
│
├── 📁 smart-studio/           # الواجهة الأمامية React 19 Web App
│   ├── 📁 components/         # واجهات الاستوديوهات التسويقية الـ 11
│   ├── 📁 services/           # خدمات التوجيه ومصادر الـ AI
│   └── 📄 package.json        # التبعيات وسكربتات التشغيل
│
├── 📁 metaai-go/              # الخادم الخلفي و API Wrapper بلغة Go
│   ├── 📁 cmd/                # ملفات الدخول التنفيذية
│   │   ├── 📄 metaai-rest     # سيرفر REST API (خلفية SMART Studio)
│   │   └── 📄 metaai-server   # بروكسي متوافق مع OpenAI/Anthropic (لـ Hermes Agent)
│   ├── 📁 rest/               # موجه الـ REST API ومنطق المعالجة
│   ├── 📁 proxy/              # طبقة البروكسي المتوافقة مع OpenAI/Anthropic
│   ├── 📁 ui/                 # لوحة التحكم الإدارية المدمجة
│   ├── 📄 Makefile            # أوامر بناء المشروع والتحقق
│   └── 📄 go.mod              # تعريف تبعيات Go ومكتباته
│
├── 📄 README.md               # التوثيق باللغة الإنجليزية
├── 📄 README.ar.md            # التوثيق باللغة العربية
└── 📄 LICENSE
```

---

## 📜 الترخيص وإخلاء المسؤولية

هذا المشروع مرخص تحت رخصة MIT - انظر ملف [LICENSE](LICENSE) لمزيد من التفاصيل.

### ⚖️ إخلاء المسؤولية
هذا المشروع هو تنفيذ مستقل وليس تابعاً رسمياً لشركة Meta Platforms, Inc. أو أي من الشركات التابعة لها.
- ✅ لأغراض تعليمية وتطويرية فقط.
- ✅ استخدمه بمسؤولية وبشكل أخلاقي.
- ✅ الالتزام بشروط خدمة Meta.
- ✅ احترام حدود الاستخدام والسياسات.

**رخصة Muse Spark:** قم بزيارة [llama.com/muse-spark](https://llama.com/muse-spark/license) لمعرفة شروط استخدام Muse Spark.
