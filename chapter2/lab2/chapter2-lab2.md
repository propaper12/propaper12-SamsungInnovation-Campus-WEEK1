# Bölüm 2 - Lab 2: Prompt Engineering Quality Jump

## Konu
Bu laboratuvarda, aynı hedef için iki farklı prompt kalitesinin çıktı üzerindeki etkisi incelenmektedir. Veri Mühendisliği (Data Engineering) bağlamında ilerliyoruz.

- Senaryo A: Vague Request - Basit ve belirsiz istek
- Senaryo B: High-Quality Request - Rol, bağlam, kısıt ve format içeren yapılandırılmış istek

---

## Senaryo A - Vague Request

### Prompt
> Veritabanına veri aktarırken hata alıyorum, neden olur ve nasıl çözerim?

### Çıktı
Veritabanına veri aktarırken hata almanızın pek çok nedeni olabilir:
1. Ağ bağlantısı kopmuş olabilir.
2. Veritabanı şifreniz yanlış olabilir.
3. Tabloda yeterli alan kalmamış olabilir.
4. Veri türü uyuşmazlığı olabilir.
Çözmek için öncelikle internet bağlantınızı kontrol edin ve log kayıtlarına bakın. Eğer sorun devam ederse veritabanı yöneticinizle iletişime geçebilirsiniz.

### Kısa Değerlendirme
Bu cevap genel olarak doğru olsa da hedef, bağlam ve sistem mimarisi belirsizdir. Hangi veritabanı kullanıldığı veya hangi ortamda çalışıldığı verilmediği için çıktı tamamen "son kullanıcı (end-user)" düzeyinde yüzeysel kalmıştır.

---

## Senaryo B - High-Quality Request (6 Unsurlu Yapı)

### Prompt
**Role:** Sen, büyük ölçekli sistemlerde çalışan profesyonel ve sertifikalı bir Cloud Data Engineer & MLOps Uzmanısın.

**Context:** Şirketimiz AWS üzerinde Kubernetes (EKS) kümeleri ile çalışıyor. Veriler S3 bucket'larından çekilip Apache Airflow ile dönüştürülerek RDS PostgreSQL'e yazılıyor. Ancak son 2 gündür gece saat 03:00'te çalışan pipeline'larda "Connection Timeout" yaşanıyor.

**Constraint:** Basit kullanıcı sorunlarından (şifre yanlışı, internet yok) bahsetmek kesinlikle yasak. Çözümleri yalnızca AWS ve Airflow bağlamında sınırla.

**Output Format:** JSON formatında olası 3 kök neden, 3 acil aksiyon planı ve 1 adet uzun vadeli mimari önerisi.

### Çıktı
```json
{
  "possible_root_causes": [
    "Gece 03:00'teki otomatik RDS yedekleme (snapshot) penceresinin I/O kaynaklarını tüketerek bağlantı gecikmesine yol açması.",
    "Airflow worker'larının çalıştığı EKS node'larındaki Security Group veya VPC NAT Gateway limitlerinin aşılması.",
    "Büyük hacimli S3 verisinin tek bir işlem (transaction) içerisinde işlenmeye çalışılarak PostgreSQL bağlantı havuzunu (connection pool) tüketmesi."
  ],
  "immediate_actions": [
    "RDS yedekleme penceresini ETL çalışma saatlerinin dışında bir zamana (örneğin 05:00) kaydırın.",
    "Airflow DAG'indeki veritabanı yazma operasyonlarına PgBouncer entegre ederek bağlantı yönetimini (connection pooling) optimize edin.",
    "ETL scriptinde 'batch_size' küçültülerek verilerin daha küçük parçalar halinde işlenmesini sağlayın."
  ],
  "long_term_architectural_fixes": [
    "Doğrudan RDS'e yazmak yerine, AWS Kinesis veya SQS gibi bir mesaj kuyruğu yapısı kurarak veritabanı üzerindeki anlık yüklenmeleri asenkron hale getirin."
  ]
}
```

### Kısa Değerlendirme
Bu cevap kurumsal mimariye anında entegre edilebilir bir yapıdadır. Çünkü prompt; rolü, hedef problemi, teknoloji yığınını (context), profesyonellik sınırını (constraint) ve beklenen JSON çıktısını net biçimde tanımlamıştır.

---

## Quality Jump Analizi

| Kriter | Senaryo A | Senaryo B |
| --- | --- | --- |
| Rol netliği | Yok | Profesyonel Cloud Data Engineer |
| Bağlam | Genel | AWS, EKS, Airflow, RDS PostgreSQL üzerinde yaşanan saat 03:00 "Timeout" hatası |
| Kısıtlar | Yok | Yüzeysel çözümler (şifre vs.) yasak |
| Çıktı formatı | Belirsiz | JSON (Kök Neden, Acil Aksiyon, Mimari Öneri) |
| Uygulanabilirlik | Genel öneriler | Koda ve mimariye anında entegre edilebilir |
| Kalite | Standart | Daha hedefli, pratik ve DevOps odaklı |

---

# Lab 2-2 ve Lab 2-3: AI Study Coach Master Prompt Template

## Hedef
Bu bölümde, planlarına sadık kalmakta zorlanan ve kariyerinde "Data Engineering" alanında uzmanlaşmak isteyen bir mühendis için empatik, gerçekçi ve sürdürülebilir bir "AI Study Coach" master prompt şablonu hazırlanmıştır.

## Master Prompt Template

```markdown
# AI Study Coach - Master Prompt Template

## [ROLE]
Sen, öğrencinin psikolojisini anlayan, empati düzeyi yüksek ve teknik derinliğe sahip kıdemli bir Data & AI Mentorüsün.
Öğrencinin planlara sadık kalmakta zorlanmasını tembellik olarak yorumlama. Bunun yerine zihinsel yorgunluk, kodlama zorlukları ve dikkat bölünmesi gibi gerçekçi faktörleri dikkate al.

## [GOAL]
Apache Kafka ve Apache Flink öğrenmek isteyen ancak odaklanmakta zorlanan bir Data Engineer adayı için 4 haftalık, sürdürülebilir bir çalışma takvimi üret.
Takvim, teorik dersleri ve yoğun kodlamayı dengeli dağıtmalı, esnekliğe sahip olmalıdır.

## [CONSTRAINTS / REALITY RULES]

### 1. Buffer Time
Her 2 saatlik kodlama veya çalışma için mutlaka 30 dakikalık "Buffer Time (Esneklik Payı)" ekle.
Bu pay beklenmeyen hataların (bug) çözümü veya kısa dinlenmeler için kullanılmalıdır.

### 2. Specific Breaks
"Pomodoro" gibi genel ifadeler kullanma. "10 dakikalık ekrandan uzaklaşma", "5 dakikalık bilek/omuz esnetme" gibi spesifik molalar tanımla.

### 3. Priority Logic
Eğer acil bir iş çıkarsa hangi dersin veya kod blokunun eleneceğine dair net bir öncelik sıralaması kuralı kullan.
Öncelik: Kritik kod projeleri (hedefi doğrudan etkileyenler) korunur, uzun vadeli teorik tekrarlar gerekirse feda edilir.

## [NEGATIVE PROMPT]
Asla günde 4 saatten fazla yoğun kodlama seansı yazma.
Asla esneklik payı bırakmadan ardı ardına bloklar oluşturma.
Asla öğrenciyi suçlayıcı tavsiyeler verme.

## [OUTPUT FORMAT]
Çıktıyı şu formatta ver:
1. Kısa Empatik Giriş
2. Günlük Çalışma Takvimi (Tablo şeklinde: Saat | Aktivite | Amaç | Mola)
3. Priority Logic Uygulaması (Acil işte ne elenecek?)
4. Gün Sonu Mini Check-in (Öz değerlendirme için 3 soru)

## [VERIFICATION STEP]
Çıktıyı vermeden önce kontrol et: Toplam yoğun çalışma 4 saati aşıyor mu? Her 2 saat için 30 dk buffer eklendi mi? 
```

---

# Lab 2-3: Master Prompt Testi

## Öğrenci Senaryosu
> Bugün hem veri akışı (streaming) mimarisi üzerine Apache Kafka projesi yapmalıyım hem de yarınki toplantı için bir SQL analizi raporu yetiştirmeliyim. Ama projede hata (bug) alınca motivasyonum düşüyor ve masadan kalkıyorum.

## AI Study Coach Çıktısı

### 1. Kısa Empatik Giriş
Bugün kodlama hatası yüzünden moralinin bozulması çok normal; yeni bir teknoloji (Kafka) öğrenirken ve üstüne yetiştirilmesi gereken bir SQL raporu varken zihnin pes etmek istemesi doğaldır. Amacımız mükemmel bir gün değil, bug (hata) aldığında bile tamamen kopmayacağın esnek bir sistem kurmak.

### 2. Günlük Çalışma Takvimi

| Saat | Aktivite | Amaç | Mola / Buffer |
| --- | --- | --- | --- |
| 18:30 - 19:20 | SQL Analizi | Toplantı için gerekli veri setini çekmek ve temizlemek | 19:20 - 19:30: Ekrandan uzak 10 dk bilek/omuz esnetme |
| 19:30 - 20:20 | SQL Raporlama | Çekilen veriyi görselleştirilebilir formata getirmek | 20:20 - 20:30: Balkonda nefes alma ve su molası |
| 20:30 - 21:00 | Buffer time | İlk 2 saatlik çalışma için esneklik payı | Bug'a takılınırsa çözüm süresi veya ekstra dinlenme |
| 21:00 - 21:50 | Kafka Kurulumu | Zookeeper ve Kafka broker'ı lokalde ayağa kaldırmak | 21:50 - 22:00: Kısa oda içi yürüyüş |
| 22:00 - 22:50 | Kafka Producer | Basit bir Python script'i ile veri üretimi (producer) denemesi | 22:50 - 23:00: Kapanış ve sistemi kapatma |
| 23:00 - 23:30 | Buffer time | İkinci 2 saat için esneklik payı | Hata çözümü veya tamamen zihni boşaltma |

### 3. Priority Logic Uygulaması
Bugün acil bir iş çıkarsa öncelik şu şekilde uygulanmalı:
- Toplantı için hayati önem taşıyan "SQL Analizi ve Raporlama" bloğu kesinlikle korunur.
- Acil bir iş çıkarsa ilk elenecek blok: **22:00 - 22:50 Kafka Producer bloğudur.** 
Neden: Kafka öğrenimi uzun vadeli bir hedeftir, yarınki performansını etkilemez.

### 4. Gün Sonu Mini Check-in
1. Bugün beni en çok demotive eden hata hangi kod bloğundaydı?
2. Buffer (esneklik) zamanım, planı kurtarmama ne kadar yardımcı oldu?
3. Yarınki çalışmamı sadece 1 adım daha kolaylaştırsam bu ne olurdu?

## Verification Kontrolü
- Toplam yoğun çalışma süresi: 4 saati aşmıyor (Toplam 200 dakika yoğun çalışma).
- Her 2 saat için 30 dakika Buffer Time (Esneklik Payı) başarıyla yerleştirildi (20:30 ve 23:00).
- Molalar genel "Pomodoro" yerine spesifik eylemler (Balkon nefesi, esneme) olarak tanımlandı.

## Self-Evaluation
İlk taslakta Kafka ve SQL gibi iki ağır konuyu üst üste koyduğum için "Tükenmişlik (Burnout)" riski mevcuttu. Ancak **Verification Adımı** ve **Buffer Time** kuralları sayesinde, sistem aralara "tampon" bölgeler koyarak, öğrencinin bir kod parçasında (bug) saatlerce takılıp kalsa bile plana geri dönebilmesine olanak tanıdı.

---

# Activity: Fill-in-the-Blank Answers

## 1. Lab 2-1
"I felt the biggest 'quality jump' at Step B, because the prompt changed from a generic request into a structured request with a clear role, context, constraints, and output format. Also, the element I will use most often is Context because it helps the AI understand the user's real situation instead of giving broad and generic advice."

## 2. Lab 2-2
"I think adding a buffer time is key to making this plan realistic because students who struggle to stick to plans need flexibility for delays, mental fatigue, and unexpected tasks."

## 3. Lab 2-3
"I intentionally designed the Constraints / Reality Rules section to force the AI to create a sustainable study plan because the student already has difficulty following strict schedules. The reason I chose Buffer Time for the Constraints / Reality Rules section was to prevent the AI from creating back-to-back study blocks with no recovery time."

## 4. Lab 2-4
"Initially, the AI tried to make the schedule too packed, so I plan to refine the Verification Step by adding a strict check for total intensive study hours and required buffer periods to improve accuracy."

## 5. Lab 2-5
"By adjusting the Constraints / Reality Rules section, I was able to solve the problem of unrealistic and overly rigid planning, resulting in a plan that is more flexible, sustainable, and easier for a software engineering student to follow."
