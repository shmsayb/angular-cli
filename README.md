بالتأكيد! سأبدأ الآن في صياغة "مستند المتطلبات الفنية التفصيلية" لمنصة "عمران تسهيل للخدمات الذكية". سيكون هذا المستند بمثابة الدليل الأساسي لجميع أعمال التطوير والاختبار والنشر المستقبلية.

سأحرص على أن يكون شاملاً، دقيقًا، احترافيًا، وقابلاً للتنفيذ، مع الالتزام بالمعايير العالمية.

---

# **مستند المتطلبات الفنية التفصيلية لمنصة "عمران تسهيل للخدمات الذكية" (Omran Tashil Smart Services)**

**إصدار:** 1.0
**التاريخ:** 2025-09-09
**اللغة:** الإنجليزية (للتوافق العالمي في التطوير) - سيتم توفير توثيق للمستخدمين النهائيين باللغة العربية.

---

## **1. مقدمة وهيكلية المنصة**

### **1.1. رؤية المشروع:**
أن تصبح "عمران تسهيل للخدمات الذكية" المنصة العالمية الرائدة التي تمكّن الأفراد والمؤسسات من خلال خدمات ذكية، مؤتمتة، وآمنة، مدعومة بالذكاء الاصطناعي، مع ضمان الشفافية والنزاهة وتسهيل الوصول لجميع المستخدمين، بما في ذلك ذوي الهمم.

### **1.2. الهيكلية المعمارية:**
*   **بنية الخدمات المصغرة (Microservices Architecture):** سيتم بناء المنصة كمجموعة من الخدمات المصغرة المستقلة، كل خدمة مسؤولة عن وظيفة محددة (مثل إدارة المستخدمين، التحقق من الوثائق، مطابقة الوظائف).
*   **التواصل بين الخدمات:** سيتم عبر APIs (RESTful أو gRPC) و/أو نظام رسائل غير متزامن (Message Queue مثل Kafka/RabbitMQ).
*   **البنية التحتية:** سحابية (Cloud-native) تعتمد على منصات مثل AWS, Azure, أو GCP.
*   **الأتمتة:** استخدام Docker و Kubernetes لإدارة ونشر الحاويات، مع خطوط أنابيب CI/CD.

### **1.3. التقنيات الأساسية المقترحة:**
*   **الواجهة الخلفية (Backend):** Python (FastAPI/Django) للـ AI/ML، Go/Rust للخدمات الحساسة للأداء، Node.js (NestJS) للـ APIs.
*   **الواجهة الأمامية (Frontend):** React.js / Next.js مع TypeScript.
*   **قواعد البيانات:** PostgreSQL (مع PostGIS)، MongoDB/Cassandra (NoSQL)، Elasticsearch، Redis.
*   **السحابة:** AWS / Azure / GCP.
*   **DevOps:** Docker, Kubernetes, Terraform, Ansible, GitHub Actions/GitLab CI.
*   **الأمان:** Keycloak/Auth0, HashiCorp Vault, WAF.
*   **المراقبة:** Prometheus, Grafana, ELK Stack.

---

## **2. تصميم قواعد البيانات (Database Schema)**

*(سيتم تقديم وصف مفصل لهياكل الجداول، الحقول، أنواع البيانات، العلاقات، والفهارس في ملف منفصل بصيغة SQL أو DDL، مع التركيز على `PostgreSQL` كقاعدة بيانات أساسية.)*

**مثال توضيحي لهيكل جدول `Users`:**

```sql
-- Table: Users
CREATE TABLE IF NOT EXISTS Users (
    user_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    username VARCHAR(255) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash TEXT NOT NULL, -- Hashed using bcrypt
    user_type_id INT NOT NULL, -- FK to UserTypes table
    registration_date TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    last_login TIMESTAMP WITH TIME ZONE,
    is_active BOOLEAN DEFAULT TRUE,
    is_verified BOOLEAN DEFAULT FALSE,
    profile_status VARCHAR(50) DEFAULT 'incomplete', -- e.g., 'incomplete', 'partial', 'complete'
    is_disabled_person BOOLEAN DEFAULT FALSE,
    personal_data JSONB, -- Encrypted sensitive personal details: name, dob, nationality, address, phone, ID number
    preferences JSONB, -- User preferences for notifications, language, etc.
    account_creation_method VARCHAR(50) DEFAULT 'email_password', -- e.g., 'email_password', 'google_oauth', 'phone_otp'
    preferred_language VARCHAR(10) DEFAULT 'en',
    CONSTRAINT fk_user_type FOREIGN KEY (user_type_id) REFERENCES UserTypes(user_type_id)
);

-- Index for quick lookup by email and username
CREATE INDEX idx_users_email ON Users (email);
CREATE INDEX idx_users_username ON Users (username);
```
*(سيتم تكرار هذا التفصيل لجميع الجداول المذكورة في الأجزاء السابقة، مع التأكيد على العلاقات، القيود، والفهارس لتحسين الأداء).*

---

## **3. متطلبات الخدمات المصغرة (Microservices Requirements)**

لكل خدمة مصغرة، سيتم تقديم:

*   **اسم الخدمة (Service Name):**
*   **الوصف الوظيفي (Functional Description):**
*   **اللغات والتقنيات المقترحة (Suggested Tech Stack):**
*   **واجهات برمجة التطبيقات (APIs):** (Endpoint, Method, Request Body Structure, Response Body Structure)
*   **الدوال والخوارزميات الرئيسية (Key Functions & Algorithms):** (وصف مفاهيمي مع أمثلة كود مفهومة)
*   **اعتماديات (Dependencies):** (خدمات أخرى، قواعد بيانات، مكتبات خارجية)
*   **متطلبات الأمان (Security Requirements):**
*   **متطلبات الأداء (Performance Requirements):**
*   **نماذج الذكاء الاصطناعي المطلوبة (AI Models Required):** (وصف للمدخلات، المخرجات، والمهام)

---

### **3.1. خدمة المستخدمين (`UserService`)**

*   **الوصف:** إدارة إنشاء، تحديث، وحذف بيانات المستخدمين، المصادقة، التصريح، وإدارة الملفات الشخصية.
*   **التقنيات:** Node.js (NestJS) / Go (Gin).
*   **APIs:**
    *   `POST /users/register`: (Request: username, email, password, user_type) -> Response: { user_id, token }
    *   `POST /users/login`: (Request: email/username, password) -> Response: { user_id, token, user_type }
    *   `GET /users/me`: (Auth: Bearer Token) -> Response: UserProfile { user_id, username, email, profile_data, ... }
    *   `PUT /users/me`: (Auth: Bearer Token, Request: update_data) -> Response: UpdatedUserProfile
    *   `POST /users/verify-email`: (Request: email, otp_code)
    *   `POST /users/refresh-token`: (Request: refresh_token) -> Response: { access_token }
*   **الدوال:** `register_user`, `login_user`, `authenticate_jwt`, `authorize_user`, `generate_tokens`, `get_user_profile`.
*   **نماذج AI:** قد تتضمن نماذج أولية لكشف أنماط محاولات تسجيل الدخول الاحتيالية (Fraud Detection).

### **3.2. خدمة الأمان (`SecurityService`)**

*   **الوصف:** توفير وظائف التشفير، تجزئة البيانات، إدارة مفاتيح التشفير، حماية API.
*   **التقنيات:** Go / Rust (للأداء والأمان).
*   **APIs:**
    *   `POST /security/encrypt`: (Request: { data_to_encrypt }) -> Response: { encrypted_data, nonce }
    *   `POST /security/decrypt`: (Request: { encrypted_data, nonce }) -> Response: { original_data }
    *   `POST /security/hash-password`: (Request: { plain_password }) -> Response: { password_hash }
    *   `POST /security/verify-password`: (Request: { password, stored_hash }) -> Response: { is_valid }
*   **الدوال:** `encrypt_data`, `decrypt_data`, `hash_password`, `verify_password`, `manage_encryption_keys`.
*   **التكامل:** مع KMS (Key Management Service) للمفاتيح.

### **3.3. خدمة التحقق من الوثائق (`VerificationService`)**

*   **الوصف:** التحقق من صحة الوثائق باستخدام OCR, AI, Computer Vision, و APIs الحكومية.
*   **التقنيات:** Python (FastAPI/Flask) مع مكتبات AI/ML.
*   **APIs:**
    *   `POST /verify/document`: (Auth: Bearer Token, Request: { document_id, user_id, live_capture_data? }) -> Response: { verification_status, details, ai_scan_data }
    *   `GET /verify/document/{doc_id}`: (Auth: Bearer Token) -> Response: DocumentVerificationStatus
*   **الدوال/الخوارزميات:** `process_document_verification` (كما تم وصفها سابقًا، تشمل OCR, NER, Face Rec, Seal Matching, API calls, Content Moderation check).
*   **نماذج AI:** Document Type Recognizer, OCR Model, NER Model, Face Recognition, Seal/Hologram Detector, Paper Analysis Model.

### **3.4. خدمة مطابقة التوظيف (`JobMatchingService`)**

*   **الوصف:** مطابقة طالبي العمل بالوظائف المتاحة.
*   **التقنيات:** Python (FastAPI).
*   **APIs:**
    *   `GET /jobs/match`: (Auth: Bearer Token, Query Params: user_id, filters) -> Response: [{ job_details, match_score }]
    *   `POST /jobs/search`: (Request: { query, filters }) -> Response: [{ job_details }]
*   **الدوال/الخوارزميات:** `find_and_match_jobs`, `calculate_job_match_score`, `semantic_matcher.get_vector`, `job_desc_parser.parse`.
*   **نماذج AI:** Semantic Matching Models (BERT-based), Skill Extraction Models, Job Description Parser.

### **3.5. خدمة توليد المستندات بالذكاء الاصطناعي (`AISkillService`)**

*   **الوصف:** إنشاء سير ذاتية، خطابات تغطية، وعروض تقديمية.
*   **التقنيات:** Python (FastAPI).
*   **APIs:**
    *   `POST /ai/cv`: (Auth: Bearer Token, Request: { user_profile_data, job_description? }) -> Response: { generated_cv_text }
    *   `POST /ai/cover-letter`: (Auth: Bearer Token, Request: { user_profile_data, job_details }) -> Response: { generated_letter_text }
*   **الدوال:** `generate_optimized_cv`, `generate_cover_letter`.
*   **نماذج AI:** Large Language Models (LLMs) مثل GPT-3.5/4 أو نماذج مفتوحة المصدر، Style & Grammar Checker Models.

*(سيتم تكرار هذا المستوى من التفصيل لجميع الخدمات المصغرة المذكورة: `EmployerService`, `GrantTrainingMatchingService`, `MedicalMatchingService`, `TravelAutomationService`, `PilgrimageMatchingService`, `DonationMatchingService`, `ConsultantMatchingService`, `AIMarketingEngine`, `ContentModerationService`, `MonitoringService`, إلخ.)*

---

## **4. متطلبات الواجهة الأمامية (Frontend Requirements)**

*   **التقنيات:** React.js / Next.js, TypeScript, Tailwind CSS / MUI.
*   **المنصة:** تطبيقات ويب متجاوبة (Responsive Web App)، وتطبيقات جوال (React Native أو Native iOS/Android) كخطوة تالية.
*   **المكونات الرئيسية:**
    *   **لوحة تحكم المستخدم (User Dashboard):** عرض جميع الخدمات، حالة الطلبات، الملف الشخصي.
    *   **صفحات خاصة بكل قسم:** (Job Search, Scholarship Finder, Hospital Search, etc.).
    *   **نماذج إدخال بيانات:** (Job Application Form, Grant Application Form, Medical Case Details, etc.).
    *   **نظام رفع المستندات:** مع مؤشرات تقدم وواجهة واضحة.
    *   **نظام الإشعارات:** (Notifications Panel).
    *   **واجهة إدارة للمسؤولين (Admin Panel):** لإدارة المستخدمين، المحتوى، المراجعات، الإعدادات.
*   **متطلبات سهولة الوصول (Accessibility):** الالتزام بمعايير WCAG 2.1 AA على الأقل.
    *   دعم قارئات الشاشة (Screen Readers).
    *   توفير بدائل نصية للصور (Alt Text).
    *   تصميمات قابلة للتخصيص (Contrast, Font Size).
    *   التنقل عبر لوحة المفاتيح.
*   **الاتصال بالـ Backend:** عبر RESTful APIs آمنة (HTTPS) مع استخدام JWTs للمصادقة.

---

## **5. استراتيجية التكامل (Integration Strategy)**

*   **APIs:** ستكون الواجهات الأساسية للتواصل بين الخدمات المصغرة. يجب أن تكون موثقة جيدًا (باستخدام OpenAPI/Swagger).
*   **نظام الرسائل (Message Queue - Kafka/RabbitMQ):** سيتم استخدامه للمهام غير المتزامنة (Asynchronous Tasks) مثل:
    *   إرسال إشعارات بعد عملية تحقق ناجحة.
    *   تشغيل عملية مطابقة بعد رفع سيرة ذاتية جديدة.
    *   إعلام خدمة أخرى بتحديث حالة معاملة.
    *   هذا يفصل العمليات ويمنع تعطل خدمة واحدة من التأثير على الأخرى.
*   **قواعد البيانات المشتركة:** بعض البيانات الوصفية قد تكون مشتركة (مثل `CountrySettings`)، وسيتم الوصول إليها عبر API أو النسخ (Replication) مع آلية مزامنة.

---

## **6. خطة DevOps (Infrastructure & Deployment)**

*   **البنية التحتية (Infrastructure):**
    *   **منصة سحابية:** AWS / Azure / GCP (سيتم اختيار واحدة بناءً على التكلفة والأداء).
    *   **تعريف البنية التحتية بالكود (IaC):** باستخدام Terraform لتعريف جميع الموارد السحابية (VMs, Databases, Networks, Load Balancers).
*   **الحاويات (Containerization):**
    *   **Docker:** سيتم بناء Dockerfiles لكل خدمة مصغرة.
*   **الأوركسترا (Orchestration):**
    *   **Kubernetes:** سيتم إعداد cluster لتشغيل وإدارة جميع الحاويات، مع استخدام Helm لتسهيل عمليات النشر.
*   **خطوط أنابيب CI/CD (Continuous Integration/Continuous Deployment):**
    *   **الأدوات:** GitHub Actions / GitLab CI / Jenkins.
    *   **العملية:**
        1.  المطور يرسل الكود إلى Git repository.
        2.  CI pipeline يبني الكود، يقوم بالاختبارات الآلية (Unit, Integration tests).
        3.  إذا نجحت الاختبارات، يتم بناء Docker image.
        4.  CD pipeline ينشر الـ image الجديد على بيئة التطوير/الاختبار (Staging).
        5.  بعد الموافقة، يتم النشر على بيئة الإنتاج (Production).
*   **المراقبة والتسجيل (Monitoring & Logging):**
    *   **التسجيل المركزي (Centralized Logging):** ELK Stack (Elasticsearch, Logstash, Kibana) لجمع وتحليل سجلات جميع الخدمات.
    *   **مقاييس الأداء (Metrics):** Prometheus لجمع المقاييس، و Grafana لعرض لوحات المعلومات (Dashboards) ومراقبة حالة النظام.
    *   **التنبيهات (Alerting):** إعداد تنبيهات (Alertmanager) للحالات الحرجة.

---

## **7. خطة الأمان (Security Plan)**

*   **OWASP Top 10:** سيتم تطبيق أفضل الممارسات لتجنب الثغرات الشائعة.
*   **التشفير:** TLS 1.3 للنقل، AES-256 (أو ما يعادلها) للبيانات الساكنة.
*   **المصادقة والتصريح:** OAuth2, JWTs, RBAC (Role-Based Access Control), MFA (Multi-Factor Authentication) للعمليات الحساسة.
*   **إدارة الأسرار (Secrets Management):** HashiCorp Vault أو خدمة KMS سحابية.
*   **جدار الحماية لتطبيقات الويب (WAF):** لحماية من هجمات الويب.
*   **تدقيق الأمان (Security Audits):** دورية، وفحوصات اختراق (Penetration Testing).
*   **مبدأ الحد الأدنى من الامتياز (Principle of Least Privilege):** منح أقل الصلاحيات اللازمة لكل خدمة ومستخدم.

---

## **8. خطة اختبارات شاملة (Comprehensive Testing Plan)**

*   **اختبارات الوحدة (Unit Tests):** لكل دالة ومكون منفرد.
*   **اختبارات التكامل (Integration Tests):** للتأكد من عمل الخدمات معًا بشكل صحيح.
*   **اختبارات النهاية إلى النهاية (End-to-End Tests - E2E):** لمحاكاة سيناريوهات المستخدم الكاملة.
*   **اختبارات الأداء (Performance & Load Testing):** لقياس الاستجابة تحت الضغط.
*   **اختبارات الأمان (Security Testing):** Penetration Testing, Vulnerability Scans.
*   **اختبارات تجربة المستخدم (Usability Testing):** ضمان سهولة الاستخدام.
*   **اختبارات سهولة الوصول (Accessibility Testing):** للتأكد من توافق معايير WCAG.

---

## **9. معايير الامتثال (Compliance Standards)**

*   **GDPR / CCPA:** لخصوصية البيانات.
*   **ISO 27001:** لنظام إدارة أمن المعلومات.
*   **WCAG 2.1 AA:** لسهولة الوصول.
*   **معايير الحوكمة الإلكترونية للدول المستهدفة:** (مثل NESA في الإمارات، أو معايير الاتحاد الأوروبي).

---

## **10. خطوات التنفيذ المستقبلية (Next Steps for Execution)**

1.  **تشكيل فريق التطوير:** توظيف فريق بشري يمتلك المهارات المحددة في المتطلبات (Backend, Frontend, DevOps, AI/ML, QA, UI/UX).
2.  **اختيار منصة سحابية:** بناءً على التكلفة، الميزات، والموقع الجغرافي.
3.  **إعداد البنية التحتية:** باستخدام Terraform/Ansible.
4.  **بدء التطوير الموازي:** تقسيم العمل على الخدمات المصغرة، مع التركيز على الخدمات الأساسية أولاً.
5.  **تطوير نماذج AI:** بالتعاون مع خبراء AI.
6.  **تنفيذ خطوط CI/CD:** لأتمتة عمليات البناء والنشر.
7.  **الاختبار المستمر:** التأكد من الجودة والأمان.
8.  **النشر الأولي (Deployment):** للمرحلة التجريبية (Beta).
9.  **جمع الملاحظات والتحسين المستمر.**

---

**هذا المستند هو الأساس النظري والتفصيلي. الخطوة التالية هي البدء في تحويل هذا إلى كود فعلي، وهذا يتطلب فريقًا متخصصًا وأدوات التطوير المذكورة.**
