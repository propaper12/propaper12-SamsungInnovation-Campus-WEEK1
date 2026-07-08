# Bölüm 2 - Lab 4: Genel Komut (General Prompt) vs. Adım Adım Talimatlar (Step-by-Step)

## Laboratuvar Senaryosu

Bu görevde, bir "Data Engineering" öğrencisi için karmaşık bir ETL mimarisi tasarım planı çıkarılması istenmektedir. AI'ın farklı bulut (cloud) servislerini seçmesi, maliyet hesaplaması yapması ve gecikme sürelerine göre uygun veri tabanı mimarisini doğrulaması gerekmektedir.

Bu çok adımlı, mantıksal ve teknik bir görevdir çünkü AI şunları yapmalıdır:
1. Gerekli veri mimarisi bileşenlerini (Ingestion, Storage, Processing) belirlemek.
2. Her bir bileşen için maliyet ve performans değerlendirmesi yapmak.
3. Toplam gecikme süresini (latency) hesaplamak.
4. Nihai mimarinin gereksinimleri (saniyenin altında gecikme) karşılayıp karşılamadığını doğrulamak.

---

## A/B Test Simülasyonu

## Prompt A - Genel Komut (General Prompt)

### Komut Stili
Tüm talimatlar ve kısıtlamalar tek bir paragraf halinde peş peşe verilmiştir. 
*("Günde 100 milyon satır akan veriyi işleyecek düşük maliyetli ve saniyenin altında gecikmeli bir mimari tasarla ve maliyeti hesapla.")*

### Simüle Edilen Sonuç
Prompt A, kullanılabilir ancak güvenilirliği düşük bir mimari üretti. Hızlıca AWS Kinesis ve Redshift önerdi ancak toplam maliyetin nasıl hesaplandığını adım adım göstermedi. Gecikme süresinin (latency) saniyenin altında olup olmayacağını ise sadece "olacaktır" diyerek geçiştirdi.

### Gözlemlenen Zayıflıklar
- AI doğrudan nihai cevaba ve sonuca atladı (Jump to conclusion).
- Maliyet hesaplama süreci şeffaf değildi (Kara Kutu).
- Bileşenlerin seçimi (neden Kafka değil de Kinesis?) gerekçelendirilmedi.
- Kısıtlamaların (düşük maliyet ve saniyenin altında gecikme) gerçekten sağlanıp sağlanmadığını doğrulamak zordu.

---

## Prompt B - Adım Adım Komut (Step-by-Step Prompt)

### Komut Stili
Görev, nihai mimariyi üretmeden önce mantıksal aşamalara (Step 1, Step 2, Step 3) bölünmüştür (Chain-of-Thought / Tree-of-Thoughts yaklaşımı).
*(Adım 1: Farklı bulut sağlayıcılarından 3 farklı mimari seçeneği üret. Adım 2: Her birini maliyet ve gecikme süresi açısından değerlendir. Adım 3: En uygun olanı seç ve hesaplamalarını göster.)*

### Simüle Edilen Sonuç
Prompt B çok daha kesin, kanıta dayalı ve doğrulanabilir bir sonuç üretti. Önce AWS, GCP ve Open-Source mimarilerini listeledi, ardından her birinin maliyet kalemlerini hesapladı ve son olarak bütçe kısıtını sağlayan Kafka + ClickHouse melez (hybrid) mimarisini seçti.

### Gözlemlenen Güçlü Yönler
- AI görünür bir mantıksal sıra izledi (Reasoning).
- Maliyet ve gecikme süresi hesaplamaları adım adım yapıldığı için doğrulaması çok kolaydı.
- Mimari seçimi rastgele değil, karşılaştırmalı analize dayanıyordu.
- Nihai cevap çok daha profesyonel, mimari bir karara (Architectural Decision Record) benziyordu.

---

## Activity Answers (Aktivite Cevapları)

### 1. Lab 4-1
"Bu görevi mükemmel bir şekilde çözmek için, AI'ın önce gereksinimleri ve veri hacmini belirlemesi, ardından maliyet ve gecikme hesaplamalarını yapması, son olarak da nihai mimarinin gerçekçi ve bütçeye uygun olup olmadığını doğrulaması (verify) gerekir."

### 2. Lab 4-2
"Prompt A, AI'ın gecikme sürelerini ve bileşen maliyetlerini detaylıca hesaplama kısıtlamasını atlaması nedeniyle eksik ve yüzeysel bir sonuç üretti."

### 3. Lab 4-3
"Adım Adım (Step-by-Step) talimatlar kullanarak AI, mimari planlamayı, maliyet hesaplamayı, bileşen seçimini ve doğrulamayı net aşamalara ayırabildi; bu şeffaflık Prompt A'nın genel yaklaşımında tamamen eksikti."

### 4. Lab 4-4
"En büyük fark mantıksal izlenebilirlik (logical traceability) idi. Prompt A hızlı bir taslak gibi hissettirirken, Prompt B yapılandırılmış ve doğrulanabilir bir çözüm sundu çünkü AI'ı nihai mimariyi sunmadan önce hesaplamaları ve karşılaştırmaları yapmaya zorladı."

### 5. Lab 4-5
"Basit bir kod açıklaması veya hızlı bir taslak metin (brainstorming) gerektiğinde Prompt A'yı kullanacağım. Ancak görev çok adımlı mantık yürütme, mimari doğruluk, hata ayıklama (debugging) veya kısıt validasyonu gerektiriyorsa (böylece AI sonuçlarımın her zaman güvenilir ve profesyonelce yapılandırılmış olmasını sağlamak için) kesinlikle Prompt B'yi kullanacağım."

---

## Kısa Yansıma (Short Reflection)

Bu A/B karşılaştırması, Prompt A'nın (Genel Komut) basit ve düşük riskli (low-risk) görevler için yararlı olabileceğini, ancak Prompt B'nin hesaplama, önceliklendirme, hata ayıklama veya validasyon gerektiren karmaşık Veri Mühendisliği iş akışları için çok daha uygun olduğunu açıkça göstermektedir. Karmaşık teknik planlama görevlerinde, adım adım (step-by-step) yönlendirme gizli halüsinasyon hatalarını azaltır ve nihai sonucun denetlenmesini (audit) çok daha kolay hale getirir.
