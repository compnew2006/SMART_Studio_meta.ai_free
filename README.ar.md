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

> 💡 **إيه هذا؟** خادم Go مُستضاف ذاتياً يلفّ Meta AI (المُشغّل بـ Muse Spark) في نقطة نهاية متوافقة مع OpenAI. وصّله بـ **Hermes Agent** وتحدّث مع نموذج LLM الخاص بـ Meta مجاناً من تليجرام أو واتساب أو أي منصة مراسلة — بدون مفتاح API.

<details>
<summary>⭐ <b>عجبك المشروع؟ نجمّه على GitHub لدعمنا!</b></summary>
<br>

لو المشروع وفّر عليك فلوس API أو سهّل عليك شغلك، نجمه ⭐ هي أحسن طريقة تقول فيها شكراً! كمان بتساعد الناس التانية تكتشفه.

[⭐ نجم المشروع على GitHub →](https://github.com/compnew2006/MetaAI-Free-Hermes-Agent/stargazers)

</details>

> [!WARNING]
> **لأغراض تعليمية فقط:** تم إنشاء هذا المشروع خصيصاً للأغراض التعليمية والبحثية والتطويرية فقط. هذا دمج غير رسمي ويجب عدم استخدامه في بيئات الإنتاج التجارية دون احترام إرشادات وشروط الخدمة الرسمية لشركة Meta.

🎯 [نظرة عامة على المشروع](#-نظرة-عامة-على-المشروع) • 🏗️ [المعمارية](#-المعمارية) • 📦 [المتطلبات](#-المتطلبات) • 🔐 [إعداد الكوكيز](#-جلب-الكوكيز-من-meta-ai-خطوة-بخطوة) • 🚀 [أوضاع التشغيل](#-التشغيل--3-أوضاع) • 🔧 [واجهة الـ REST API](#-واجهة-الـ-rest-api-reference)

---

## 🔍 نظرة عامة على المشروع

* **[smart-studio](file:///Users/noiemany/Downloads/meta.ai/smart-studio)**: واجهة تصميم وتسويق حديثة (React 19) توفر 11 استوديوًا متخصصًا للعلامات التجارية، التصوير الفوتوغرافي، الفيديو، التعليقات الصوتية، الكامباينز، وتحليل السوق.
* **[metaai-go](file:///Users/noiemany/Downloads/meta.ai/metaai-go)**: خادم API سريع للغاية بلغة Go لـ Meta AI. يستقبل الطلبات من واجهة SMART Studio ويتواصل مباشرة مع Meta AI باستخدام مصادقة الكوكيز.

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
│  components/*.tsx ──imports──▶ services/geminiService.ts        │
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
│  metaai-go REST Server  (Go binary)                            │
│                                                                │
│  /chat  /analyze  /upload  /image  /image/fetch  /video*       │
│      rest/handlers.go  rest/analyze_handler.go                 │
│                                                                │
│  + embedded SMART Studio SPA at /  (في prod build)             │
└────────────────────────────────────────┬───────────────────────┘
                                         │ WebSocket + GraphQL
                                         │ (cookies + access token)
                                         ▼
┌────────────────────────────────────────────────────────────────┐
│  Meta AI  (meta.ai)                                            │
│  - الصور المولدة على scontent-arn2-1.xx.fbcdn.net              │
│  - الفيديوهات المولدة على video-*.xx.fbcdn.net                 │
└────────────────────────────────────────────────────────────────┘
```

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

# إعدادات الخادم الاختيارية
META_AI_REST_ADDR=:8000
META_AI_REST_TOKEN=smart-studio-dev-token
META_AI_CORS_ORIGIN=http://localhost:3000
```

### إعدادات الواجهة الأمامية (`smart-studio/.env`)
أنشئ ملف `.env` داخل مجلد `smart-studio`:
```env
VITE_METAAI_URL=http://localhost:8000
VITE_METAAI_TOKEN=smart-studio-dev-token
GEMINI_API_KEY=your_gemini_api_key_here  # مطلوب فقط لـ Voice Over Studio (TTS)
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
META_AI_REST_TOKEN=smart-studio-dev-token ./bin/metaai-rest
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
Authorization: Bearer <META_AI_REST_TOKEN>
```

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
  -H "Authorization: Bearer smart-studio-dev-token" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Cyberpunk cityscape at night", "orientation": "LANDSCAPE"}'
```

---

## 🌟 هيكل المشروع

```text
meta.ai-sdk/
│
├── 📁 smart-studio/           # الواجهة الأمامية React 19 Web App
│   ├── 📁 components/         # واجهات الاستوديوهات التسويقية الـ 11
│   ├── 📁 services/           # خدمات التوجيه ومصادر الـ AI
│   └── 📄 package.json        # التبعيات وسكربتات التشغيل
│
├── 📁 metaai-go/              # الخادم الخلفي و API Wrapper بلغة Go
│   ├── 📁 cmd/                # ملفات الدخول التنفيذية (metaai-rest, server)
│   ├── 📁 rest/               # موجه الـ REST API ومنطق المعالجة
│   ├── 📁 ui/                 # لوحة التحكم الإدارية المدمجة
│   ├── 📄 Makefile            # أوامر بناء المشروع والتحقق
│   └── 📄 go.mod              # تعريف تبعيات Go ومكتباته
│
├── 📄 README.md               # التوثيق باللغة الإنجليزية
└── 📄 README.ar.md            # التوثيق باللغة العربية
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
