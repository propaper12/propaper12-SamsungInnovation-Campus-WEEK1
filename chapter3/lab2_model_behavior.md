# Bölüm 3 - Laboratuvar 2: Temel Modellerin (Foundation Models) İç Çalışma Mantığı

Bu çalışmada, üretken yapay zeka modellerinin "sihir" olmadığını, arkasında yatan matematiksel ve mimari bileşenleri (Unit 2 Konseptleri) inceleyerek anlamlandırıyoruz.

## 1. Kavramların Özeti (Understanding Check)

Aşağıdaki kavramlar, LLM'lerin (Büyük Dil Modelleri) insan dilini nasıl işlediğini ve metin ürettiğini açıklar:

- **Tokenization (Sözcükleri Parçalama):** AI, bizim yazdığımız cümleleri doğrudan anlamaz. BPE veya WordPiece algoritmalarını kullanarak "unhappiness" gibi bir kelimeyi "un", "happi", "ness" gibi alt birimlere (sub-word) böler ve bunlara matematiksel ID'ler atar. Bu, modelin çok büyük kelime dağarcıklarını verimli şekilde yönetmesini sağlar.
- **Word Embedding (Kelimeleri Anlama):** Token ID'leri rastgele sayılardır. Model, bu sayıları çok boyutlu bir uzayda koordinatlara (vektörlere) dönüştürür. "Apple" ve "Banana" kelimeleri bu uzayda birbirine çok yakındır. Bu sayede model "anlamsal ilişkileri" kavramış olur (Örn: King - Man + Woman = Queen mantığı).
- **Transformer ve Self-Attention (Bağlamı Yakalama):** Eski modeller kelimeleri sırayla okurdu (RNN). Transformer ise kelimeleri paralel okur. "Self-Attention" (Öz-Dikkat) mekanizması, cümledeki bir kelimenin diğer kelimelerle olan ilişkisini ağırlar. Örneğin "Oturduğu bank kırıldı" cümlesindeki "bank" kelimesinin finans kurumu değil, oturak olduğunu etrafındaki kelimelere "dikkat kesilerek" anlar.
- **Decoding ve Temperature (Yaratıcılığı Kontrol Etme):** Model, geçmiş bağlama bakarak "bir sonraki kelimeyi (Next Token Prediction)" tahmin eder. `Temperature` (Sıcaklık) ayarı 0.1 ise model her zaman en yüksek olasılıklı kelimeyi seçer (kod yazımı için ideal). 0.9 ise daha düşük olasılıklı kelimelere şans verir ve yaratıcı (veya bazen tutarsız) metinler üretir.

## 2. Derinlemesine Kavram İncelemesi (Deep Dive): "Hallucination" (Halüsinasyon)

Benim derinlemesine incelemek üzere seçtiğim kavram: **Halüsinasyon (Hallucination)**

**A. Neden Meydana Gelir? (Underlying Mechanism)**
Temel model bir "Gerçeklik Kontrolcüsü (Fact Checker)" değil, bir "Olasılıklı Metin Tamamlayıcısıdır (Probabilistic Text Completer)". Eğer model pre-training (ön-eğitim) sırasında kirli, çelişkili veya eksik veriyle (Training Data Contamination) beslenmişse, aradaki boşlukları kulağa en mantıklı ve en "olası" gelen şekilde doldurmaya çalışır. Model, bir gerçeği bilmediğini söylemek yerine, bağlama uygun pürüzsüz bir yalan sentezler.

**B. Optimizasyon ve Kontrol**
Bu sorunu çözmek için modeli "Fine-Tuning" (İnce Ayar) ile eğitmek tek başına yetmez, çünkü bilgiler sürekli değişir. Halüsinasyonu engellemenin en iyi yolu **RAG (Retrieval-Augmented Generation)** kullanmaktır. RAG, modelin "ezberinden" (parametric memory) konuşmasını yasaklar; modele önce kurumun gerçek PDF/Veritabanı dokümanlarını verir ve "sadece bu dokümana bakarak cevap ver" (Grounded Generation) kısıtlamasını getirir.

**C. Etki ve Gelecek Vizyonu**
Halüsinasyon, Finans ve Hukuk gibi "High-Stakes" (Yüksek riskli) sektörlerde yapay zekanın benimsenmesinin önündeki en büyük engeldir (Örn: ChatGPT'nin sahte mahkeme emsalleri uydurması vakası). Gelecekte, LLM'lerin "bilmiyorum" demeyi öğrenebilmesi ve "ReAct" gibi düşünce ağaçlarıyla kendi ürettiği metni internetten eş zamanlı doğrulayabilmesi (Self-Correction) bu sorunu çözecektir.

## 3. Konsept Anlayış Raporu

Araştırmalarım sonucunda aşağıdaki senaryoyu şu şekilde tamamlıyorum:

> "Genel amaçlı bir yapay zekanın (Örn: ChatGPT) serbest ürettiği cevapların aksine, **RAG tabanlı veya Şema Öncelikli sistemlerin belirli belgelere dayanarak verdiği yanıtlar daha güvenilirdir**; çünkü **modelin yaratıcılık katsayısı (Temperature) düşürülüp dışarıdan doğrulanmış bilgiyle sınırlandırılması**, standart komutlarda genellikle göz ardı edilen **'Halüsinasyon' ve 'Bağlam Eksikliği (Lack of Context)' risklerini doğrudan ortadan kaldırır.**"
