---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #0f172a
color: #ffffff
style: |
  section {
    font-family: 'Segoe UI', Roboto, sans-serif;
    padding: 40px 50px;
    font-size: 1.4em;
  }
  h1 {
    color: #38bdf8;
    font-size: 1.9em;
    font-weight: bold;
    margin-bottom: 12px;
    text-shadow: 0 2px 4px rgba(0,0,0,0.6);
  }
  h2 {
    color: #38bdf8;
    font-size: 1.4em;
    margin-top: 0;
  }
  footer {
    color: #64748b;
  }
  ul, ol {
    margin-top: 10px;
  }
  li {
    font-size: 0.95em;
    margin-bottom: 12px;
    line-height: 1.4;
    color: #ffffff; 
  }
  strong {
    color: #38bdf8;
    font-weight: bold;
  }
  blockquote {
    background: #1e293b;
    border-left: 6px solid #38bdf8;
    padding: 12px 20px;
    margin: 10px 0;
    font-size: 0.85em;
    line-height: 1.4;
    color: #ffffff;
    border-radius: 4px;
  }
  table {
    border-collapse: collapse;
    width: 100%;
    margin-top: 15px;
  }
  th {
    background-color: #1e293b;
    color: #38bdf8;
    border: 2px solid #334155;
    padding: 10px;
    font-size: 0.85em;
    font-weight: bold;
  }
  td {
    border: 1px solid #334155;
    padding: 10px;
    font-size: 0.82em;
    color: #ffffff;
  }
  .highlight-green {
    color: #4ade80;
    font-weight: bold;
  }
  .highlight-red {
    color: #f87171;
    font-weight: bold;
  }
---

# Kurumsal Güven Boşluğunu Kapatmak
## Bağlamsız AI (Context-Free) vs. Temellendirilmiş (Grounded) RAG
### Bölüm 03 - Lab 3: RAG Performans Analizi

**Ömer Çakan**
*Data & AI Engineer*

---

## 📌 Yönetici Özeti (Executive Summary)

* **Temel Problem:** Önceden eğitilmiş Büyük Dil Modelleri (LLM'ler) inanılmaz derecede güçlü akıl yürütme motorlarıdır, ancak şirket içi kurumsal verilere karşı *olgusal olarak kördürler*. Bu durum, modası geçmiş cevaplara veya yüksek riskli halüsinasyonlara yol açar.
* **Çözüm:** Retrieval-Augmented Generation (RAG), metin üretmeden hemen önce ilgili doküman bağlamlarını dinamik olarak prompt'a (komuta) besleyerek modeli temellendirir (grounding).
* **Kurumsal Zorunluluk:** Veri Mühendisliği ve yazılım mimarisi gibi profesyonel uygulamalar deterministik, denetlenebilir ve doğrulanabilir çıktılar gerektirir. Temellendirme (Grounding) bir seçenek değil; temel bir gereksinimdir.

---

## 🔍 Vaka İncelemesi: Adım 1 vs. Adım 2 Uygulaması

### Senaryo: Veri Akışı API (Streaming API) Yapılandırma Hatası ve Şema Validasyonu

* **Adım 1: Bağlamsız (Doküman Olmadan - Context-Free)**
  > *"Error 4043 genellikle API'ye gönderdiğiniz JSON payload'unun sunucu tarafında beklenen şema ile uyuşmamasından kaynaklanır. Çözmek için JSON verisindeki veri tiplerinin doğru olduğundan emin olun..."* 
  > ❌ **Eleştiri:** Belirsiz, kaçamak ve teknik olarak kullanılamaz (Halüsinasyon).

* **Adım 2: Temellendirilmiş (RAG ile Doküman Sağlanarak - Grounded)**
  > *"'Error 4043' hatasını çözmek için: Gönderdiğiniz veride `event_timestamp` alanının tam olarak **ISO-8601 formatında** olduğundan emin olun ve API v2.1 ile zorunlu hale gelen **`user_id`** alanını eklediğinizi doğrulayın..."* 
  > ✅ **Eleştiri:** Yüksek hassasiyetli, doğrudan aksiyon alınabilir, nokta atışı veri noktaları.

---

## 📊 Performans Karşılaştırma Matrisi

| Değerlendirme Kriteri | Adım 1: Bağlamsız (Dokümansız) | Adım 2: Temellendirilmiş (RAG ile) |
| :--- | :--- | :--- |
| **1. Doğruluk (Accuracy)** | <span class="highlight-red">Düşük - Orta</span> (Statik/Eski veri) | <span class="highlight-green">Kusursuz (%100)</span> (Aktif kaynaklar) |
| **2. Özgüllük (Specificity)** | <span class="highlight-red">Genel / Kaçamak</span> ("JSON'u kontrol et") | <span class="highlight-green">Aşırı Spesifik</span> (Net alan adları ve formatlar) |
| **3. Temellendirme (Grounding)** | <span class="highlight-red">Yok</span> (Sıfır kaynak takibi) | <span class="highlight-green">Tam</span> (Satır satır atıf yapabilme) |
| **4. Halüsinasyon Riski** | <span class="highlight-red">Yüksek</span> (Tahmin etmeye eğilimli) | <span class="highlight-green">Sıfır</span> (Prompt içine hapsedilmiş) |
| **5. Güvenilirlik** | <span class="highlight-red">Güvensiz</span> (Üretim ortamı için riskli) | <span class="highlight-green">Üretime Hazır (Production-Ready)</span> (Denetim için güvenli) |

---

## 🎯 Mimari Sonuç (Architectural Conclusion)

### Kurumsal Yapay Zekanın Altın Kuralı:
> "Üretken Yapay Zeka (Generative AI) asla gerçeklerin depolandığı statik bir veritabanı olarak hareket etmemelidir; sağlanan veri varlıkları üzerinde çalışan bir **akıl yürütme motoru (reasoning engine)** olarak işlev görmelidir."

* **Denetlenebilirlik ve Uyumluluk (Auditability & Compliance):** RAG, veri mühendislerinin üretilen her bir cümleyi, yetkilendirilmiş bir PDF veya veritabanındaki kesin bir paragrafa (kaynağa) kadar takip etmesine olanak tanır.
* **Maliyet ve Bakım Verimliliği:** Devasa parametreli modelleri sürekli yeniden eğitmek (re-training) veya ince ayar (fine-tuning) yapmak yerine, geliştiriciler altta yatan vektör veritabanındaki bağlamı saniyeler içinde güncelleyebilir veya değiştirebilirler.
