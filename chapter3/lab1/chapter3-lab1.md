# Chapter 3 - Lab 1: Endüstriye Özel Temel Modeller (Case Exploration)

Bu laboratuvarda, genel amaçlı modeller (General Purpose AI) yerine, Data Engineering sektörüne yönelik tasarlanmış dikey AI (Vertical AI) kullanım senaryolarını analiz ediyoruz.

## 1. Analiz Edilen Sektör: Veri Mühendisliği ve MLOps

**Neden bu sektör?**
- **Ön Bilgi İhtiyacı (Prior Knowledge):** Çok Yüksek. Spark optimizasyonu, dağıtık sistemler (Kafka) derin teknik bilgi gerektirir.
- **Risk ve Sorumluluk Seviyesi (High vs Low Stakes):** Yüksek Risk. Üretilen hatalı bir veri aktarım kodu, tüm üretim veritabanını çökertebilir.
- **Jargon Yoğunluğu:** Çok Yüksek.

## 2. Gerçek Dünya Kullanım Örnekleri

### Örnek 1: Databricks DBRX
- **Kullanım Şekli:** Kurumsal veri havuzlarında (Data Lakehouse) SQL sorguları oluşturmak ve veri mühendisliği süreçlerini otomatize etmek için.
- **Analiz:** Jargon yoğunluğunun ve veriye özel bağlamın (schema) en çok kullanıldığı, sektöre özel eğitilmiş yapı.

### Örnek 2: GitHub Copilot / OpenAI Codex
- **Kullanım Şekli:** IDE içinde anlık kod tamamlama, Unit Test yazma.
- **Analiz:** "Next Token Prediction" mantığını kullanarak milyonlarca satır repodan beslenir, yazılımcının ne yapmak istediğini anlar.

---

# Chapter 3 - Lab 2: Temel Modellerin İç Çalışma Mantığı (Deep Dive)

## 1. Kavramların Özeti
- **Tokenization:** Kelimeleri (örn: unhappiness) alt parçalara (un-happi-ness) bölerek matematiksel ID'lere atama.
- **Word Embedding:** Kelimeleri vektör uzayında koordinatlara dönüştürerek anlamsal ilişkileri kurma.
- **Transformer (Self-Attention):** Kelimeleri sırayla değil, paralel okuyarak bir kelimenin cümledeki diğer kelimelerle olan bağlamını (ağırlığını) çözme.

## 2. Derinlemesine Kavram İncelemesi: "Halüsinasyon"

**A. Neden Meydana Gelir?**
Model bir "Fact Checker" değil, "Olasılıklı Metin Tamamlayıcısıdır". Eğitim verisindeki boşlukları, kulağa en mantıklı gelen yalanlarla (halüsinasyon) doldurur.

**B. Optimizasyon ve Kontrol**
Bunu çözmenin en iyi yolu **RAG (Retrieval-Augmented Generation)** kullanmaktır. RAG, modelin "ezberinden" konuşmasını yasaklar; modele önce kurumun kendi belgelerini (PDF/DB) verir ve "sadece bu dokümana bakarak cevap ver" kısıtlamasını getirir.

---

# Chapter 3 - Lab 3: RAG (Retrieval-Augmented Generation) Konseptinin Test Edilmesi

## Senaryo: Veri Akışı API Yapılandırma Hatası

### Adım 1: Doküman Olmadan Soru Sorma (Baseline - Context Free)
**Komut:** "DataFlowX API'sini kullanarak Kafka'ya veri gönderirken Error 4043 alıyorum. Çözüm nedir?"
**AI Çıktısı (Özet):** JSON veri tiplerinizi kontrol edin, dokümantasyona bakın.
**Analiz:** Cevap mantıklı ama tamamen genel geçer (Halüsinasyon).

### Adım 2: Doküman Sağlayarak Soru Sorma (RAG Simülasyonu)
**Komut:** "[DOKÜMAN] Error 4043: event_timestamp alanı ISO-8601 değilse ve user_id eksikse fırlatılır... Soru: DataFlowX API Error 4043 nasıl çözülür?"
**AI Çıktısı (Özet):** Dokümana göre event_timestamp'i ISO-8601 yapın ve user_id eklediğinizden emin olun.
**Analiz:** %100 doğru, spesifik ve kanıta dayalı (Grounded).

## Self-Evaluation
Gerçek bir kurumsal veri mimarisinde, modelin yaratıcılık katsayısı (Temperature) düşürülüp dışarıdan RAG ile doğrulanmış bilgiyle sınırlandırılması, halüsinasyon risklerini ortadan kaldırır. "High-Stakes" olan Data Engineering alanında RAG olmazsa olmazdır.

---

# Activity: Fill-in-the-Blank Answers

1. "I chose to explore the Data Engineering industry because it involves High Stakes risks like database crashes and requires deep prior knowledge of distributed systems."
2. "When evaluating Databricks DBRX, I realized that Industry-Specific AI provides much more accurate SQL and Spark generation than general models."
3. "The most important concept for our field is Hallucination because in software development, a plausible-sounding but fake code snippet can lead to massive security breaches."
4. "Using RAG fundamentally changed the AI's output from generic guesswork into pinpoint accurate, documented solutions."
5. "If I were a CTO, I would never deploy a general LLM without RAG for internal enterprise data, because the risk of confident factual errors is simply too high."
