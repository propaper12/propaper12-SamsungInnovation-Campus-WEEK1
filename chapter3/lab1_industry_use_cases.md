# Bölüm 3 - Laboratuvar 1: Endüstriye Özel Temel Modeller (Case Exploration)

Bu laboratuvarda, genel amaçlı modeller (General Purpose AI) yerine, spesifik bir sektöre yönelik tasarlanmış dikey AI (Vertical AI) kullanım senaryolarını analiz ediyoruz. Benim uzmanlık alanım olan **Veri Mühendisliği ve Yazılım Geliştirme (Software/Data Engineering)** sektörünü baz aldım.

## 1. Analiz Edilen Sektör: Yazılım ve Veri Mühendisliği

**Neden bu sektör?**
- **Ön Bilgi İhtiyacı (Prior Knowledge):** Yüksek. Kod yazımı, mimari dizaynı ve performans optimizasyonu derin teknik terminoloji ve spesifik teknoloji bilgisi (Kafka, Spark, AWS vb.) gerektirir.
- **Risk ve Sorumluluk Seviyesi (High vs Low Stakes):** Yüksek Risk. Üretilen bir SQL sorgusundaki veya veri aktarım (ETL) boru hattındaki tek bir hata, şirketin tüm veritabanını çökertebilir veya veri sızıntılarına (data breach) yol açabilir.
- **Jargon Yoğunluğu:** Çok Yüksek. Yazılım ve veri mimarisi günlük konuşma dilinden tamamen uzaktır.

## 2. Gerçek Dünya Kullanım Örnekleri (Real-World AI Use Cases)

Aşağıdaki araştırmaları yapay zeka asistanı kullanarak gerçekleştirdim ve kendi yorumlarımla harmanladım.

### Örnek 1: Databricks DBRX (Data-Centric LLM)
- **Kullanım Şekli:** Doğrudan kurumsal veri havuzlarında (Data Lakehouse) SQL sorguları oluşturmak ve veri mühendisliği süreçlerini otomatize etmek için kullanılır.
- **Analiz:** Bu model, standart bir metin yazarı olmaktan ziyade, büyük veri kümelerini anlamak, sorgulamak ve Spark/Delta Lake gibi spesifik teknolojilerde kod üretmek üzere eğitilmiş (Fine-Tuning) özel bir modeldir. 

### Örnek 2: GitHub Copilot / OpenAI Codex
- **Kullanım Şekli:** IDE (VS Code vb.) içinde anlık kod tamamlama, Unit Test yazma ve Legacy (eski) kodu modernize etmede kullanılır.
- **Analiz:** Jargon yoğunluğunun en fazla olduğu yer burasıdır. Modelin mimarisi "Sequential to Parallel Processing" (Transformer) yeteneklerini kullanarak, milyonlarca satır açık kaynak kod reposundan edindiği parametrelerle yazılımcının ne yapmak istediğini "Next Token Prediction" yöntemiyle saniyenin altında gecikmeyle (low latency) tahmin eder.

### Örnek 3: NVIDIA NIM ve NeMo Framework
- **Kullanım Şekli:** Hassas şirket içi verilerin dışarı çıkmasını engellemek için, şirketlerin kendi sunucularında (On-Premise) çalışan güvenlik kritik (Security-Critical) AI dağıtımlarında kullanılır.
- **Analiz:** Yüksek risk barındıran veri bilimi projelerinde, sLLM (Small LLM) mimarileri kurularak tamamen kapalı devre bir şirket içi asistan yaratılmasını sağlar.

## 3. Bulguların Sentezi (Step-by-Step Sequence)

Yaptığım araştırmanın sonucunu şu cümleyle özetleyebilirim:

> "Genel amaçlı bir yapay zekanın (General-purpose AI) aksine, **Databricks DBRX gibi sektöre özel (Industry-Specific) bir modeli** tercih ediyorum. Çünkü **veritabanı sorguları ve Spark optimizasyonu gibi yüksek doğruluk gerektiren, hata toleransının olmadığı (High Stakes) durumlar**, standart genel modeller tarafından kolayca gözden kaçırılabilir veya halüsinasyon yoluyla şirkete ciddi zararlar verebilir."
