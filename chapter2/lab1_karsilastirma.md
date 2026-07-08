# Bölüm 2 - Laboratuvar 1: Prompt Mühendisliği Kalite Karşılaştırması

Bu çalışmada, üretken yapay zekaya verilen komutların (prompt) kalitesinin çıktıya olan doğrudan etkisini inceliyoruz. Ahmet'in genel "sağlık" senaryosundan farklı olarak, bu çalışmada tamamen **Veri Mühendisliği (Data Engineering) ve MLOps** odaklı bir vaka üzerinden ilerleyeceğiz.

## Senaryo: Veri Hattı (Data Pipeline) Hata Analizi

### 1. Kötü Komut (Vague Request)

**Prompt:**
> "Veritabanına veri aktarırken hata alıyorum, neden olur ve nasıl çözerim?"

**Çıktı (AI):**
> Veritabanına veri aktarırken hata almanızın pek çok nedeni olabilir:
> 1. Ağ bağlantısı kopmuş olabilir.
> 2. Veritabanı şifreniz yanlış olabilir.
> 3. Tabloda yeterli alan kalmamış olabilir.
> 4. Veri türü uyuşmazlığı olabilir.
>
> Çözmek için öncelikle internet bağlantınızı kontrol edin. Ardından şifrenizin doğruluğundan emin olun ve log kayıtlarına bakın. Eğer sorun devam ederse veritabanı yöneticinizle iletişime geçebilirsiniz.

**Neden Yetersiz?**
- **Amaç Belirsiz:** Hangi veritabanı (PostgreSQL, MongoDB vb.), hangi ortam (Cloud, On-Premise) belli değil.
- **Hedef Kitle (Rol) Yok:** AI genel, son kullanıcıya hitap eden yüzeysel bir cevap ("İnternet bağlantınızı kontrol edin") veriyor.
- **Format Eksik:** Geri dönüş sadece düz metin. Yapılandırılmış bir hata ayıklama kontrol listesi sunmuyor.

---

### 2. İyileştirilmiş Komut (6 Unsurlu Yapı)

Eğitimde öğrendiğimiz Rol, Hedef, Bağlam, Kısıtlamalar, Stil ve Çıktı Biçimi (6 Elements Framework) kurallarını uygulayarak aynı komutu profesyonel hale getirdik.

**Prompt:**
> **[Rol]** Sen, büyük ölçekli sistemlerde çalışan kıdemli bir Cloud Data Engineer ve MLOps Uzmanısın.
> 
> **[Hedef]** Staging ortamındaki PostgreSQL veritabanına Apache Airflow üzerinden yapılan günlük ETL veri aktarımında oluşan "Connection Timeout" hatasının kök nedenlerini ve adım adım çözüm planını oluştur.
> 
> **[Bağlam]** Şirketimiz AWS üzerinde Kubernetes (EKS) kümeleri ile çalışıyor. Veriler S3 bucket'larından çekilip dönüştürülerek RDS PostgreSQL'e yazılıyor. Ancak son 2 gündür gece saat 03:00'te çalışan pipeline'larda bağlantı zaman aşımı yaşanıyor.
> 
> **[Kısıtlamalar]** 
> - Basit kullanıcı sorunlarından (şifre yanlışı, internet yok) bahsetme.
> - Çözümleri yalnızca AWS ve Airflow bağlamında sınırla.
> - Varsayımsal çözümler üretme, mimariyi baz al.
> 
> **[Stil]** Üst düzey teknik bir dille, tamamen analitik ve çözüm odaklı yaz.
> 
> **[Çıktı Biçimi]** 
> Lütfen çıktıyı aşağıdaki JSON yapısına uygun şekilde ve Türkçe olarak ver:
> ```json
> {{
>   "possible_root_causes": ["neden1", "neden2"],
>   "immediate_actions": ["aksiyon1", "aksiyon2"],
>   "long_term_architectural_fixes": ["mimari_oneri1"]
> }}
> ```

**Çıktı (AI):**
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

## Karşılaştırma Sonucu ve Alınan Ders
Kötü komut, son kullanıcı düzeyinde işe yaramaz ve genel geçer "interneti kontrol et" yanıtları üretirken; **6 Unsurlu Yapı** kullanılarak tasarlanmış komut doğrudan kod tabanımıza entegre edebileceğimiz, DevOps mühendislerinin okuyup anında aksiyon alabileceği bir JSON manifestosu üretmiştir.
