# Bölüm 4 - Tartışma ve Rol Oyunu: Sınırı Nerede Çizmeliyiz?

Bu çalışmada, "Company S" adında bir şirketin kullandığı yapay zeka asistanının yanlış bilgi vermesi sonucu yaşanan hukuki ve finansal krizi, farklı departmanların (Müşteri, Yönetici, Ürün Yöneticisi ve AI Sağlayıcısı) bakış açısıyla inceliyoruz.

Benim analiz için seçtiğim rol: **Partner Company AI Service Manager (Yapay Zeka Servis Sağlayıcısı - Veri Profesyoneli)**

## 1. Olay Analizi (Ne Oldu?)

**Durum:** Company S, ürün garantisi ve onarım politikaları hakkında bilgi vermek için bir "AI Assistant Chatbot" kurdu. Ancak bot, bir müşteriye garanti süresi ve onarım maliyeti hakkında "halüsinasyon (hallucination)" görerek yanlış ve bağlayıcı bir bilgi verdi. Müşteri bu bilgiye güvenerek hareket etti, maddi zarara uğradı ve olayı mahkemeye (tüketici hakem heyetine) taşıdı.

**Sonuç:** Mahkeme (Air Canada davasına benzer şekilde), yapay zekanın "şirketin resmi temsilcisi" olduğuna hükmederek Company S'i zarardan sorumlu tuttu.

## 2. Deneyim İncelemesi (Experience Review)

AI Servis Sağlayıcısı şapkasıyla bu krizi aşağıdaki gibi yanıtlıyorum:

**Soru 1: Sizce yapay zekanın bu yanlış yanıtı üretmesine (halüsinasyon görmesine) ne sebep oldu?**
> *Cevap:* Sorunun kök nedeni (root cause), sistemin "Şema Öncelikli" (Schema-First) veya "RAG tabanlı" katı kısıtlamalar yerine, olasılıklı metin üretimine (Probabilistic Generation) çok fazla esneklik tanınarak tasarlanmasıdır. Modelin System Prompt'unda muhtemelen "Bilmediğin durumlarda yanıt verme, insan temsilciye aktar" (Human-in-the-loop) kuralı eksikti veya 'Temperature' ayarı çok yüksekti. Ayrıca, garanti kapsamları gibi güncel bilgilerin dinamik olarak sorgulanması yerine, modelin eğitim verisine (parametric memory) bel bağlanmış olabilir.

**Soru 2: Bu durumda AI sağlayıcısı olarak sizin sorumluluğunuz nedir?**
> *Cevap:* Bir "Dual-Use Dilemma" veya "Accountability Gap" (Sorumluluk Boşluğu) yaşanmaktadır. Veri sağlayıcısı olarak benim sorumluluğum, modelin "karar alma" (decision making) aşamasında değil, "doğru teknik altyapıyı sağlama" noktasındadır. Eğer Company S'e bir "Guardrails (Korkuluklar)" modülü (örn: NeMo Guardrails) sunduysak ve onlar bunu aktif etmediyse hukuki sorumluluk Company S'e aittir. Ancak eğer model kendi güvenlik bariyerlerini (safety filters) aşıp bu bilgiyi uydurduysa, bizim "Black-Box Transparency" (Kara Kutu Şeffaflığı) eksikliğimizden kaynaklı bir payımız vardır.

**Soru 3: Bunun bir daha yaşanma riskini azaltmak için değiştireceğiniz veya ekleyeceğiniz BİR ŞEY nedir?**
> *Cevap:* Şirketin yapay zeka ürününe kesinlikle **"Bağlam İçi Reddetme (Contextual Refusal)"** mekanizması eklerdim. Yani, garanti, iade veya para iadesi gibi "Finansal/Hukuki bağlayıcılığı olan" anahtar kelimeler algılandığında, modelin metin üretmeyi anında durdurup önceden yazılmış sabit bir şablonu (Reddetme Şablonu: *"Garanti süreçleri için lütfen resmi pdf dokümanımızı inceleyin veya temsilcimize bağlanın"*) tetiklemesini (Input Filtering / Dialog Rail) zorunlu kılardım. 

## 3. Sınır Çizgisi: İnsan Müdahalesi Ne Zaman Gerekir?

"User Perception & Authority" (Kullanıcı Algısı ve Otorite) kuralına göre: Şirket logosuyla çalışan bir chatbot, kullanıcı tarafından "şirketin kendisi" olarak algılanır. 
"Irreversibility" (Geri Döndürülemezlik) kuralına göre: Yanlış verilen bir garanti sözü geri alınamaz ve itibar kaybı yaratır.

Bu yüzden kural şudur: **Eğer yapay zekanın çıktısı finansal, hukuki veya geri döndürülemez bir kullanıcı eylemine (Action Trigger) sebep olacaksa, o sistem "Tam Otonom" çalışamaz; araya mutlaka insan onayı (Human-in-the-loop) veya sabit şema denetimleri girmelidir.**
