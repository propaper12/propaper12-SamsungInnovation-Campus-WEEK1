# Bölüm 3 - Laboratuvar 3: RAG (Retrieval-Augmented Generation) Konseptinin Test Edilmesi

Bu laboratuvarın amacı, dış bir bağlam veya doküman verilmeden (Context-Free) üretilen AI yanıtları ile, spesifik bir doküman sağlanarak (Grounded) üretilen AI yanıtları arasındaki farkı, bir Data Engineering API senaryosu üzerinden karşılaştırmaktır.

## Senaryo: Veri Akışı API (Streaming API) Yapılandırma Hatası

### Adım 1: Doküman Olmadan Soru Sorma (Baseline - Context Free)

**Komut:**
> "Şirketimizin geliştirdiği 'DataFlowX' API'sini kullanarak Kafka kuyruğuna veri göndermeye çalışıyorum ancak 'Error 4043: Schema validation failed' hatası alıyorum. Bu hatayı nasıl çözerim?"

**AI Çıktısı (Gemini):**
> Error 4043 genellikle API'ye gönderdiğiniz JSON yükünün (payload), sunucu tarafında beklenen şema ile uyuşmamasından kaynaklanır. Çözmek için:
> 1. Gönderdiğiniz JSON verisindeki veri tiplerinin doğru olduğundan emin olun (örneğin Integer yerine String göndermediğinize).
> 2. Gerekli tüm alanların (required fields) istek içinde yer aldığını kontrol edin.
> 3. API dokümantasyonunuzu inceleyerek schema gereksinimlerini teyit edin.

**Analiz:**
Cevap mantıklı görünüyor, ancak "DataFlowX" adında benim uydurduğum hayali bir API'nin detaylarını bilmediği için genel geçer yazılım mühendisliği prensiplerini sıraladı (Halüsinasyon). Spesifik bir çözüm yok, çünkü modelin ağırlıklarında (parametric memory) bu şirkete özel bilgi yok.

---

### Adım 2: Doküman Sağlayarak Soru Sorma (RAG Simülasyonu)

Modele, hayali "DataFlowX" API'sinin dokümantasyonundan bir parça vererek aynı soruyu sordum.

**Komut:**
> Aşağıdaki sistem dokümanını okuyarak soruma cevap ver.
> 
> [DOKÜMAN - DataFlowX API v2.1]
> Error 4043: Schema validation failed. Bu hata, sistem Kafka kuyruğuna veri yazmaya çalışırken `event_timestamp` alanının ISO-8601 formatında (`YYYY-MM-DDTHH:MM:SSZ`) gönderilmediği durumlarda fırlatılır. Ayrıca v2.1 güncellemesiyle birlikte `user_id` alanı zorunlu hale getirilmiştir, eksikse bu hata döner.
> 
> Soru: "DataFlowX API'sini kullanarak Kafka kuyruğuna veri göndermeye çalışıyorum ancak 'Error 4043: Schema validation failed' hatası alıyorum. Bu hatayı nasıl çözerim?"

**AI Çıktısı (Gemini - Grounded):**
> 'Error 4043' hatasını çözmek için dokümantasyonda belirtilen iki kritik noktayı kontrol etmelisiniz:
> 1. Gönderdiğiniz veride `event_timestamp` alanının tam olarak ISO-8601 formatında (Örn: `2026-07-08T15:30:00Z`) olduğundan emin olun.
> 2. API v2.1 ile birlikte zorunlu hale gelen `user_id` alanını JSON payload'unuza (isteğinize) eklediğinizi doğrulayın.

---

### Adım 3: Karşılaştırma ve Sonuç

Farklılıkları "Kalite Kriterlerine" göre özetliyorum:

- **Accuracy (Doğruluk):** Adım 1'deki cevap mantıklı ama olgusal olarak yanlıştı. Adım 2'deki cevap ise şirket dokümantasyonuna dayandığı için %100 doğruydu.
- **Specificity (Özgüllük/Netlik):** Adım 1 "JSON formatına bak" derken, Adım 2 doğrudan `"event_timestamp"` ve `"user_id"` değişkenlerine nokta atışı yaptı.
- **Grounding (Temellendirme):** İlk cevap tamamen modelin ezberine dayanırken, ikinci cevap tamamen sağlanan metne bağlı (grounded) olarak üretildi.
- **Hallucination Risk (Halüsinasyon Riski):** İlk durumda yüksek. Model, bilmediği bir API için çok profesyonel bir tonda genel tavsiyeler üreterek yazılımcıyı saatlerce yanlış yerlerde hata aramaya sevk edebilirdi.

**Güvenilirlik Refleksiyonu (Real Business Context):**
> Gerçek bir kurumsal senaryoda, özellikle DevOps, Finans veya Müşteri Destek süreçlerinde, **sadece Adım 2'deki (RAG destekli) yaklaşımı kullanırım.** Çünkü kurumsal bilgilerin her gün güncellendiği bir dünyada modelin kendi hafızasına güvenmek feci sonuçlar doğurabilir; RAG mimarisi, modeli bir "ezberci" olmaktan çıkarıp dış kaynaklı kanıtlara dayanan bir "analiste" dönüştürür.
