# Bölüm 2 - Laboratuvar 4: Gelişmiş Komut Teknikleri (ToT ve ReAct) A/B Karşılaştırması

Bu çalışmada, karmaşık ve tek bir doğru cevabı olmayan problemlerde, yapay zekanın farklı teknikler kullanılarak nasıl daha derinlemesine akıl yürütebileceğini inceliyoruz.

## Karşılaştırma Senaryosu: Ölçeklenebilir Veri Mimarisi Tasarımı

**Görev:** "Şirketimizin günde 100 milyon satır akan (streaming) veriyi işlemesi ve analitik ekibi için saniyenin altında gecikmeyle sunması gerekiyor. Maliyetin düşük olması şart. Mimari tasarımı oluştur."

### Test A: Genel Adım Adım Komut (Standart CoT)
İlk denememizde, AI'a standart "önce sorunu anla, sonra bileşenleri seç, en son mimariyi oluştur" şeklinde düz bir adım-adım talimat verdik. 
Model hemen Kafka ve Snowflake kullanmayı önerdi. Mimari mantıklıydı, ancak maliyetlerin aşırı yüksek olabileceğini gözden kaçırdı ve farklı bir bulut mimarisi denemedi. "Tek doğru cevap" varmış gibi hareket etti.

### Test B: Düşünce Ağacı (Tree of Thoughts - ToT) ile Seçim
Bu senaryoda, sorunun tek bir doğrusu olmadığını bildiğimiz için **Düşünce Ağacı (ToT)** yaklaşımını kullandık:

**Prompt Yapısı:**
> "Adım 1: Bu gereksinimleri karşılayacak tamamen birbirinden farklı (biri AWS native, diğeri Open-Source tabanlı) 3 adet farklı veri mimarisi senaryosu üret (Dallanma).
> Adım 2: Her bir mimariyi "Performans", "Maliyet" ve "Bakım Zorluğu" eksenlerinde eleştirel bir şekilde değerlendir (Değerlendirme).
> Adım 3: En uygun olan 2 mimariyi seçip melez (hybrid) bir optimal sistem önerisi sun (Yakınsama)."

**Sonuç ve Analiz:**
Model, ilk denemedeki (A) yüzeysel cevabın aksine;
1. Seçenek olarak AWS Kinesis + Redshift,
2. Seçenek olarak Apache Kafka + ClickHouse,
3. Seçenek olarak Pub/Sub + Athena sundu.
Sonra kendi ürettiği bu çözümlerin "Maliyet" kısıtlamasına uymayanları eledi ve bizim için en optimal (Kafka + ClickHouse) yapıyı sentezledi. 

**En büyük fark:** ToT yaklaşımı ile AI, "ilk bulduğu çözüme" atlamak yerine birden fazla hipotez kurup bunları çapraz değerlendirerek (kendi fikirlerini eleştirerek) jüri gibi davranmıştır.

## A/B Kullanım Kriterleri (Uygulama Kurallarım)

Elde ettiğim bu bulguları sentezleyerek gelecekteki projelerimde kullanacağım kendi "Altın Kuralımı" şu şekilde belirledim:

> "Eğer görev basit bir metin çevirisi, kod açıklaması veya rutin bir rapor yazımıysa **Prompt A'yı (Standart / Zero-Shot)** kullanacağım. 
> Ancak görev, mimari tasarım, kampanya stratejisi, hata ayıklama (debugging) gibi **tek bir doğru cevabı olmayan ve çok boyutlu karar gerektiren bir problemse**, modelin kör noktalarını gidermek için mutlaka **Prompt B'yi (ToT veya ReAct)** kullanacağım. Böylece AI sonuçlarının her zaman daha derinlemesine analiz edilmiş, güvenilir ve tutarlı olmasını sağlamak için modelin kendi kendini çapraz sorgulamasını garanti altına almış olurum."
