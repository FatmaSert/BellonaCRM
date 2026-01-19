# Bellona Markası Müşteri Şikayetleri ve Kök Neden Analizi Projesi - 
> Fatma SERT - 132230040

## A Proje Tanıtımı 
**1. Giriş ve Proje Amacı**
Günümüz rekabetçi pazar koşullarında, müşteri memnuniyeti ve satış sonrası destek, marka sadakatini belirleyen en kritik faktörlerdendir. Bu projenin temel amacı, Türkiye'nin önde gelen mobilya markalarından biri olan Bellona hakkında, popüler tüketici şikayet platformu Sikayetvar.com üzerinde yayınlanan kullanıcı şikayetlerini sistematik olarak toplamak, analiz etmek ve temel problem alanlarını (kök nedenleri) tespit etmektir.
Bu çalışma, ham veri madenciliği (web scraping) teknikleri kullanılarak yapılandırılmamış metin verisinin toplanması ve ardından Doğal Dil İşleme (NLP) ön hazırlık teknikleri ve kural tabanlı algoritmalarla anlamlı içgörülere dönüştürülmesi sürecini kapsamaktadır.

**2. Metodoloji**

Proje, temel olarak iki ana teknik evreden oluşmaktadır: Veri Toplama (Data Collection) ve Veri Analizi (Data Analysis).

**2.1 Veri Toplama Süreci (Web Scraping)**

Veri toplama işlemi için Python programlama dili ve BeautifulSoup kütüphanesi kullanılarak özel bir web kazıma algoritması (scraper) geliştirilmiştir. Bu algoritma aşağıdaki teknik prensiplerle çalışmaktadır:
Dinamik Sayfalandırma (Pagination): Algoritma, şikayetlerin listelendiği ana sayfaları (page=1, page=2 vb.) iteratif olarak gezmektedir. Bu süreçte, sayfa yönlendirmeleri (redirection) kontrol edilerek, var olmayan bir sayfaya gidildiğinde (örneğin son sayfadan sonra başa dönme durumu) tarama işlemi otomatik olarak sonlandırılmaktadır.

- **Tam Metin Çıkarımı**: Liste sayfalarından elde edilen her bir şikayet bağlantısı (URL) detaylı bir şekilde ziyaret edilmektedir. Sadece başlık değil, şikayetin tüm detaylarını içeren "Full Text" (tam metin), kullanıcı adı, tarih ve şikayet ID'si gibi özellikler ayrıştırılmaktadır (parsing).
- **Eşzamanlı İşleme (Concurrency)**: Veri toplama hızını artırmak ve ağ gecikmelerini minimize etmek amacıyla concurrent.futures.ThreadPoolExecutor kullanılarak çoklu iş parçacığı (multithreading) mimarisi uygulanmıştır. Bu sayede aynı anda 5 farklı şikayet sayfası işlenebilmekte, toplam işlem süresi önemli ölçüde düşürülmektedir.
- **Hata Yönetimi ve Süreklilik**: Ağ hatalarına (network timeouts) karşı dirençli bir yapı kurulmuş ve daha önce taranan verilerin tekrar taranmasını önlemek için bir "resume" (kaldığı yerden devam etme) mekanizması eklenmiştir.
Elde edilen veriler, yapısal bir format olan CSV (bellona_sikayetleri.csv) dosyasına kaydedilerek analize hazır hale getirilmiştir.

**2.2 Veri Ön İşleme ve Zenginleştirme**

Ham veri analize girmeden önce temizlik ve birleştirme işlemlerine tabi tutulmuştur:
Metin Birleştirme: Şikayet başlığı (Title) ve şikayet detayı (Full Text) birleştirilerek, analizin daha geniş bir bağlamda yapılması sağlanmıştır.
Normalizasyon: Tüm metinler küçük harfe çevrilerek, büyük/küçük harf duyarlılığından kaynaklanabilecek analiz hatalarının önüne geçilmiştir.
Eksik Veri Yönetimi: Boş alanlar doldurularak veri bütünlüğü sağlanmıştır

**2.3 Analiz ve Kategorizasyon Stratejisi**

Toplanan veriler üzerinde yapılan analizler, şikayetlerin kök nedenlerini belirlemeye odaklanmıştır. Bu kapsamda kural tabanlı (rule-based) bir sınıflandırma yaklaşımı benimsenmiştir:

Hiyerarşik Kategori Haritalama: Şikayetler, önceden tanımlanmış ana kategoriler (Örn: "Teslimat ve Lojistik", "Servis ve İletişim") ve alt kategoriler (Örn: "Aşırı Gecikme", "Kaba Üslup") bazında sınıflandırılmıştır.
Anahtar Kelime Analizi : Her bir alt kategori için bu sorunu temsil eden özgün kelime havuzları (lexicon) oluşturulmuştur. Örneğin, "gelmedi", "haftalar geçti" gibi ifadeler "Teslimat Gecikmesi"ni işaret ederken; "bağırdı", "yüzüme kapattı" gibi ifadeler "Kaba Üslup" kategorisini tetiklemektedir.
Keşifsel Analiz : "Diğer" (Unclassified) olarak işaretlenen veriler üzerinde yapılan örnekleme çalışmaları ile, mevcut kategorilere girmeyen ancak "servis" veya "müşteri hizmetleri" ile ilgili olan yeni problem alanlarının keşfedilmesi hedeflenmiştir.

**3. Bulgular ve Teknik Değerlendirme**

Yapılan kod analizi ve veri incelemesi sonucunda projenin şu teknik yetkinliklere sahip olduğu görülmüştür:
Ölçeklenebilirlik: Geliştirilen kazıma motoru, binlerce şikayeti kesintisiz olarak çekebilecek bir mimariye sahiptir.
Veri Kalitesi Güvencesi: Özellikle "Full Text" (Tam Metin) alanının doğru çekilmesi için yapılan son düzeltmeler, analizin yüzeysel başlıklardan ziyade detaylı müşteri deneyimlerine dayanmasını sağlamıştır.
Dinamik Kategori Tespiti: Proje statik bir analizden ziyade, veriye dayalı olarak kategorilerini geliştiren ("Diğer" kategorisindeki verilerin incelenmesi gibi) iteratif bir yapıdadır.

**4. Sonuç**

Bu proje, bir öğrenci çalışması olarak, veri bilimi yaşam döngüsünün kritik aşamaları olan veri toplama, temizleme ve keşifsel analiz süreçlerini başarıyla modellemektedir. Yapılan çalışma, Bellona markasının müşteri deneyimi karnesini çıkarmak için gerekli teknik altyapıyı sağlamış olup, marka yetkililerine "teslimat süreleri" ve "çağrı merkezi iletişimi" gibi spesifik alanlarda iyileştirme yapmaları gerektiğini gösteren somut veriler sunmaktadır.



#  B.Bellona Yönetici Özeti ve Durum Raporu

Tarih: 19 Ocak 2026 Konu: Şikayetvar Verileri Üzerinden Müşteri Deneyimi ve Kök Neden Analizi
**1. Yönetici Özeti** 
Bu rapor, Bellona markasına yönelik dijital platformlarda (Şikayetvar.com) yer alan müşteri geri bildirimlerinin yapay zeka destekli analizi sonucunda hazırlanmıştır. Analiz, markanın müşteri sadakatini doğrudan etkileyen "Teslimat", "Ürün Kalitesi" ve "Satış Sonrası Hizmetler" alanlarında kritik darboğazlar yaşadığını ortaya koymaktadır. Çalışmada ayrıca tespit edilen problemlere çözüm önerileri sunulmuştur.
 
**2. Mevcut Durum ve Veri Analizi**

Proje kapsamında geliştirilen veri madenciliği algoritmaları ile yüzlerce güncel müşteri şikayeti "tam metin" olarak analiz edilmiştir. Elde edilen veriler, şikayetlerin münferit vakalar olmaktan çıkıp, belirli alanlarda sistematik sorunlara dönüştüğünü göstermektedir.
2.1 Öne Çıkan Sorun Dağılımı
<img width="798" height="425" alt="image" src="https://github.com/user-attachments/assets/fc1e30b9-88da-4bd0-b603-b540490e9cfa" />

%40 - Teslimat ve Lojistik: Taahhüt edilen (genellikle 45 gün) teslimat sürelerinin aşıldığı, kısmi teslimat yapıldığı veya yanlış ürün gönderildiği tespit edilmiştir.
%35 - Ürün Kalitesi (Erken Deformasyon): Müşterilerin önemli bir kısmı, ürün tesliminden sonraki ilk 3-6 ay içinde "kumaş tüylenmesi", "sünger çökmesi" ve "dikiş hatası" gibi kalite sorunları bildirmektedir.
%25 - İletişim ve Servis: Şikayetlerin çözümsüz kalmasındaki en büyük etkenin, bayiler ve genel merkez arasındaki kopukluk olduğu, müşterilerin "muhatap bulamama" sorunu yaşadığı görülmüştür.



**2.2 Alt Kırılmların Dağılımı**

<img width="716" height="577" alt="image" src="https://github.com/user-attachments/assets/8a578527-dcfb-497f-ba72-0fb097296ed1" />


<img width="538" height="553" alt="image" src="https://github.com/user-attachments/assets/cbb6b832-c315-41f1-97d9-1d5549cab2a4" />


<img width="580" height="550" alt="image" src="https://github.com/user-attachments/assets/1931fe11-b053-4898-86a1-753de7c9fcb1" />




Teslimat ve Lojistik kategorisinde en önemli şikayet konusu %77.3 ile aşırı gecikmedir.

Üretim ve Kalite açısından kumaş/döşeme problemleri (%36.2) ve Ahşap Mekanizma (%29.1) problemleri en çok şikayete konu olan alt kırılımlardır.


Servis ve İletişim kök nedeni alt kırılımlara ayrıldığında çeşitli nedenlar vardır. Ancak kaba üslup, yalan beyan ve ulaşılamama bu kategorideki en önemli sorunlardır.

**2.3 En Sorunlu Modeller**
Şikayetlerde model ismi belirten kullanıcıların yorumlarına dayalı olarak sorunlu modeller aşağıda belirtilmiştir.
<img width="945" height="462" alt="image" src="https://github.com/user-attachments/assets/89278846-09df-4ab8-81af-1b7e43790ea3" />


Buna göre  Valencia, Estella ve Larissa modelleri en problemli modeller olarak görülmektedir.
**2.4 Sabır Endeksi**
Verilere göre tüm müşteriler anında şikayet etmemektedir. Tarihsel olarak incelendiğinde kimi müşteriler anında şikayet etmeye başlamışken bazı müşteriler ise belirli bir süre sonra şikayet etmeye başlamışlardır. Aşağıdaki grafikte sabır endeksi grafiği sunulmuştur.
<img width="945" height="453" alt="image" src="https://github.com/user-attachments/assets/09aae707-3412-4468-8ad4-5db97ef07c32" />

Grafikte ortalama şikayet süresi 81.5 gün olarak tespit edilmiştir.
**2.5 Şikayetlerin İllere Göre Dağılımı**
Verilere göre şikayetlerin illere göre dağılımında en sorunlu 15 il ve şikayet yüzdeleri verilmiştir.
<img width="945" height="544" alt="image" src="https://github.com/user-attachments/assets/8c47f5d0-5473-4717-9129-90bec873d9ed" />


İstanbul ve Ankara gibi büyükşehirlerin dışında “Muş” , “Ağrı” ve “Ordu” illerinin listede olması önemli bir sorundur.
**2.6 Yasal Risk Analizi**
Kimi müşteriler şikayetlerine yasal durumlarla tehdit etmektedir. Aşağıdaki grafikte yasal tehditte bulunan müşterilerin oranları bulunmaktadır.

<img width="945" height="698" alt="image" src="https://github.com/user-attachments/assets/090676ad-b1e7-4c87-9497-b82887424449" />


**2.7 Müşteri Kaybı Analizi**
Kök neden ve alt kırılımlar bazlı olarak müşteri kaybı analizi yapıldığında elde edilen grafik aşağıda sunulmuştur.

<img width="945" height="550" alt="image" src="https://github.com/user-attachments/assets/0d413419-d86d-4ae2-bd52-73c1c2629d68" />


Buna göre en çok müşteri kaybına neden olabilecek alt kırılmlar aşağıdaki grafikte verilmiştir.

<img width="945" height="556" alt="image" src="https://github.com/user-attachments/assets/36af152c-4643-49ef-9977-4943a468040d" />


En çok müşteri kaybına neden olabilecek alt nedenler, “ulaşılamama”, “konfor” ve “yalan beyan” konularıdır.
Müşteri kaybını tetikleyen faktörler incelendiğinde aşağıdaki grafikte faktörler verilmiştir.

<img width="945" height="550" alt="image" src="https://github.com/user-attachments/assets/f268fbef-416b-4f3b-a98b-4cc7065cf203" />



Yasal risk ve uzun bekleme süreleri müşterileri tekrar alışveriş yapmamaya iten nedenlerin en başında gelmektedir.

**3. Tespit Edilen Kritik Bulgular**
Entegrasyon Eksikliği: Satış noktası (Bayi) ile Üretim/Lojistik merkezi arasında veri senkronizasyonu sorunu bulunmaktadır. Müşterilere verilen teslimat tarihleri, üretim planlaması ile örtüşmemektedir.
Sessiz Müşteri Kaybı: "Servis kaydı oluşturamama" veya "ulaşılamama" şikayetleri, müşterilerin sorunlarını çözemeden markadan uzaklaşmasına ve olumsuz deneyimlerini sosyal çevresine aktarmasına neden olmaktadır.
Kalite Algısında Düşüş: "Premium" segment algısıyla satılan ürünlerde kısa sürede oluşan deformasyonlar, marka imajına ciddi zarar vermektedir.
**4. Stratejik Öneriler ve Aksiyon Planı**
Kısa Vadeli Aksiyonlar (0-3 Ay)
Proaktif İletişim Hattı: Gecikme yaşayan müşterilere şikayet etmelerini beklemeden "Proaktif Bilgilendirme" (SMS/Arama) yapılması, tansiyonu düşürecektir.
Şikayet Müdahale Timi: Şikayetvar üzerindeki "cevap verme" süresini kısaltacak ve şablon cevaplar yerine çözüm odaklı dönüş yapacak özel bir ekip kurulmalıdır.
Orta ve Uzun Vadeli Stratejiler
Tedarik Zinciri Revizyonu: Kumaş ve sünger tedarikçilerinin kalite standartları yeniden denetlenmelidir. "Erken deformasyon" sorunu üretim bandında çözülmelidir.
Dijital Entegrasyon: Bayilerin stok ve üretim durumunu anlık görebileceği, müşteriye gerçekçi termin verebileceği entegre bir ERP sistemine geçiş hızlandırılmalıdır.
**5. Çözüm Önerileri**
Veriler ışığında, tespit edilen temel sorunlara yönelik yönetici düzeyinde çözüm önerileri aşağıda sunulmuştur:
**1. Teslimat ve Lojistik Süreçlerinin Optimizasyonu**
Şikayetlerin %40'ını oluşturan bu alanda en büyük sorun %77.3 ile "aşırı gecikme"dir.
Çözüm Önerileri:
Termin Sürelerinin Revizesi: Mevcut 45 günlük taahhüt süresi, üretim ve lojistik kapasitesiyle uyumlu hale getirilerek gerçekçi rakamlara çekilmelidir.
Lojistik Takip Sistemi: Yanlış veya eksik ürün gönderimini (%8.9) engellemek için sevkiyat öncesi barkod tabanlı kontrol sistemleri güçlendirilmelidir.
Bölgesel Depolama: Özellikle büyükşehirler dışındaki Muş, Ağrı ve Ordu gibi illerden gelen yüksek şikayet oranlarını düşürmek için bölgesel dağıtım merkezleri veya stratejik lojistik ortaklıklar değerlendirilmelidir.
**2. Üretim ve Kalite Standartlarının Yükseltilmesi**
Şikayetlerin %35'ini oluşturan bu kategoride sorunlar genellikle ürün kullanımının ilk 3-6 ayında ortaya çıkmaktadır.
Çözüm Önerileri: 
Malzeme Kalite Kontrolü: Şikayetlerin odağındaki kumaş/döşeme (%36.2) ve ahşap mekanizma (%29.1) için tedarikçi denetimleri sıkılaştırılmalı ve dayanıklılık testleri artırılmalıdır.
Model Bazlı İyileştirme: En çok şikayet alan Valencia, Estella ve Larissa modelleri özelinde üretim bandı incelemesi yapılmalı, kronik tasarım veya malzeme hataları giderilmelidir.
Erken Deformasyon Analizi: Sünger çökmesi ve dikiş hataları için "sıfır hata" prensibiyle çalışan bir kalite güvence ekibi kurulmalıdır.
**3. Servis ve İletişim Kanallarının Güçlendirilmesi**
Müşterilerin %25'i servis ve iletişimden şikayetçi olup, temel sorun "muhatap bulamama" ve bayilerle merkez arasındaki kopukluktur.
Çözüm Önerileri:
Merkezi Şikayet Yönetimi: Bayilerin inisiyatifine bırakılmadan, genel merkezden yönetilen ve tüm sürecin izlenebildiği bir CRM sistemi aktif kullanılmalıdır.
Personel Eğitimi ve Etik Kurallar: Müşteri kaybını en çok tetikleyen "ulaşılamama" (%6.25) ve "yalan beyan" (%3.70) gibi sorunları aşmak için çağrı merkezi ve servis personeline iletişim ve etik eğitimi verilmelidir.
Geri Bildirim Döngüsü: Ortalama 81.5 gün bekleyen ("Sabır Endeksi") müşterilerin şikayet yazma aşamasına gelmeden önce proaktif aramalarla memnuniyetleri sorgulanmalıdır.
**4. Müşteri Kaybı (Churn) ve Yasal Risk Yönetimi**
Yasal risk içeren şikayetlerin oranı %8.32 olsa da, bu durum müşteri kaybını tetikleyen en güçlü faktördür.
Çözüm Önerileri:
Yasal Tehdit Önleme Protokolü: Yasal risk taşıyan şikayetler "yüksek öncelikli" olarak işaretlenmeli ve hukuk departmanı ile koordineli bir hızlı çözüm masası oluşturulmalıdır.
Telafi Mekanizmaları: Uzun bekleme süreleri nedeniyle oluşacak müşteri kaybını önlemek adına, gecikme yaşanan siparişlerde indirim çekleri veya küçük hediyelerle müşteri sadakati korunmaya çalışılmalıdır.

**6. Sonuç**
Bellona markası için müşteri deneyimi, sadece satış anında değil, ürünün evde kullanıldığı süre boyunca devam eden bir süreçtir. Bu rapordaki bulgular ışığında, operasyonel süreçlerin "müşteri odaklı" olarak yeniden yapılandırılması, pazar payının korunması ve artırılması için elzemdir.
