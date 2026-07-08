# Bölüm 2 - Laboratuvar 3: Sıfır Atış (Zero-Shot) ve Az Atış (Few-Shot) Teknik Testleri

Bu görevde, üretken yapay zekanın bağlam ve şablon eksikliğinde ne kadar başarılı olduğunu ve Few-Shot (Az Atışlı Öğrenme) tekniği eklendiğinde formatı nasıl tutarlı hale getirdiğini test ediyoruz.

## Senaryo: Veri Mühendisliği ETL Kod İncelemesi (Code Review)

### Test A: Zero-Shot Prompt

**Sistem İpucu (Kural):**
> Sen bir Veri Mühendisliği takım liderisin. Gelen kod parçacıklarını inceleyerek güvenlik, performans ve temiz kod kurallarına göre değerlendir.

**Talimat (Görev):**
> Şu PySpark kodunu incele ve geri bildirim ver: 
> `df = spark.read.csv("s3://bucket/data.csv"); df.write.mode("overwrite").parquet("s3://bucket/clean_data/")`

**Zero-Shot Analizi:**
AI, bu isteme çok genel, upuzun paragraflar halinde cevap verdi. Birinci denemede "CSV okurken schema vermelisin" derken, ikinci denemede "Performans için coalesce kullanmalısın" diyerek her çalıştırmada tamamen **farklı bir yapı ve üslup** üretti. Beklentimiz tutarlı ve otomatize edilebilir bir çıktı formatıydı, ancak Zero-Shot yaklaşımı her seferinde "başka bir telden çalarak" otomasyonda kullanılamayacak sonuçlar verdi.

---

### Test B: Few-Shot Prompt

**Sistem İpucu (Kural):**
> Sen bir Veri Mühendisliği takım liderisin. Gelen kod parçacıklarını inceleyerek güvenlik, performans ve temiz kod kurallarına göre değerlendir. Tüm geri bildirimleri, aşağıdaki örneklere tam olarak uygun şekilde formatla.

**Talimat ve Few-Shot Örnekleri:**

> **Örnek 1:**
> Kod: `pd.read_sql("SELECT * FROM users", conn)`
> Çıktı:
> [Seviye: KRİTİK] [Kategori: Performans] Tüm tabloyu Pandas ile belleğe çekmek OOM (Out Of Memory) hatasına sebep olur. LIMIT veya Chunking kullan.
>
> **Örnek 2:**
> Kod: `s3_client.upload_file("data.json", "my-bucket", "data.json", ExtraArgs={"ACL": "public-read"})`
> Çıktı:
> [Seviye: YÜKSEK] [Kategori: Güvenlik] S3 bucket'ına 'public-read' izni vermek veri sızıntısına yol açar. ACL ayarını 'private' olarak değiştirin.
>
> **Şimdi şu kodu incele ve yukarıdaki formata birebir uyarak cevap ver:**
> Kod: `df = spark.read.csv("s3://bucket/data.csv"); df.write.mode("overwrite").parquet("s3://bucket/clean_data/")`

**Few-Shot Analizi:**
AI modeli, sağladığımız bu iki örnek sayesinde ne kadar detaya inmesi gerektiğini ve çıktıyı tam olarak nasıl formatlayacağını ("[Seviye: ...] [Kategori: ...]") anında kavradı. Ürettiği çıktı, örneklerimizle 100% tutarlı bir yapıdaydı:
*"[Seviye: ORTA] [Kategori: Performans] CSV okurken schema (şema) veya inferSchema belirtmemek Spark'ın veriyi iki kez taramasına yol açar. Schema'yı açıkça tanımlayın."*

## Öz Değerlendirme (Self-Evaluation)
- **Zero-Shot Nerede Yeterli?:** Sadece beyin fırtınası yaparken, taslak metin (draft) üretirken veya yaratıcılığın yüksek olmasını istediğimiz ilk fikir aşamalarında hızlı bir taslak üretmek için Zero-Shot çok başarılı.
- **Few-Shot Ne Zaman Gerekli?:** Şablonların bozulmaması gerektiğinde, modelin tutarlı formatta çıktı üretmesi (örneğin kurumsal kod inceleme biletleri oluştururken) kesinlikle Few-Shot (veya Şema öncelikli) yaklaşım kullanılmalıdır.
