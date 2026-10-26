# Proje Başlığı

Bu projenin farklı geliştirme aşamalarına aşağıdaki branch'lerden ulaşabilirsiniz:

* 🚀 [Yeni Özellik 1 Geliştirmeleri](../../tree/Şişeleme) - Bu branch üzerinde Şişeleme kısmı  yer almaktadır.
* 🛠️ [Yeni Özellik 2 Geliştirmeleri](../../tree/Paketleme) - Bu branch üzerinde Paketleme kısmı yer almaktadır.

# Gürmak Makina 3 Eksenli CNC Dik İşleme Merkezi — SWOT ve RAMS Analizi

## İçindekiler

1. [Proje Hakkında](#1-proje-hakkında)
2. [Makine Tanıtımı](#2-makine-tanıtımı)
3. [SWOT Analizi](#3-swot-analizi)
4. [RAMS İlkeleri](#4-rams-ilkeleri)
5. [SWOT ile RAMS İlişkisi](#5-swot-ile-rams-ilişkisi)
6. [Laboratuvar Çıktıları](#6-laboratuvar-çıktıları)
7. [Varsayımlar ve Sınırlar](#7-varsayımlar-ve-sınırlar)
8. [Kaynaklar](#8-kaynaklar)

---

## 1. Proje Hakkında

Bu depo, **Gürmak Makina (SAMİB Üyesi) 3 Eksenli CNC Dik İşleme Merkezi** için iki yöntemle yapılmış değerlendirmeyi içerir:

- **SWOT analizi:** Makinenin güçlü/zayıf yönleri ile fırsat ve tehditlerinin, özellikle fonksiyonel emniyet ve siber güvenlik açısından ortaya konması.
- **RAMS ilkeleri:** Makinenin **Güvenilirlik (Reliability)**, **Kullanılabilirlik (Availability)**, **Bakım Yapılabilirlik (Maintainability)** ve **Emniyet (Safety)** boyutlarında değerlendirilmesi.

Amaç, makinenin hem üretim sürekliliğini hem de insan emniyetini aynı çerçevede ele almak ve bunun siber güvenlikle ilişkisini göstermektir.

## 2. Makine Tanıtımı

| Özellik | Açıklama |
|---|---|
| Üretici | Gürmak Makina (SAMİB üyesi) |
| Makine tipi | 3 eksenli CNC dik işleme merkezi (X-Y-Z) |
| Ana hareketli parçalar | İş mili, X-Y-Z eksen sürücüleri, otomatik takım değiştirici (ATC) |
| Emniyet zinciri | Acil stop / kilitli kapı → Safety PLC → STO / kontaktör → Safe State |
| Normal kontrol zinciri | Sensör → PLC (CNC kontrolcü) → Aktüatör |
| Dijital kesişim noktası | IPC / uzak erişim (bakım ve güncelleme) |

Başlıca tehlikeler: dönen iş mili ve takım (sarılma, kesilme), eksen ve ATC hareketi (ezilme, sıkışma), takım/iş parçası/talaş fırlaması ve yangın (kesme sıvısı, talaş).

## 3. SWOT Analizi

| | **Olumlu** | **Olumsuz** |
|---|---|---|
| **İç etkenler** | **S — Güçlü yönler** | **W — Zayıf yönler** |
| **Dış etkenler** | **O — Fırsatlar** | **T — Tehditler** |

### S — Güçlü Yönler (Strengths)

- Emniyet fonksiyonları tanımlı ve ayrı bir safety yolunda yaşar: kilitli kapı, acil stop → STO, ayar modunda onay şalteri.
- Safety PLC çift kanallı çalışır, arızada güvenli duruma (STO) geçer.
- 3 eksenli yapı mekanik olarak sade olduğundan arıza noktası sayısı 5 eksenli makinelere göre azdır.
- Yerli üretici ve SAMİB üyeliği: yedek parça, servis ve eğitime erişim kolaylığı.
- Standart bir kontrol mimarisi (PLC + sürücüler) sayesinde bakım personelinin tanıdığı bir yapı.

### W — Zayıf Yönler (Weaknesses)

- Emniyet ve normal kontrol yolları dijital olarak kesişebilir; bu kesişim IPC / uzak erişim noktasındadır.
- Uzak servis erişiminin mevcut durumda doğrudan, paylaşılan hesapla ve sürekli açık olabileceği varsayılmıştır (doğrulanmalıdır).
- Safety PLC yazılımı/parametreleri mühendislik laptop'u ile yüklendiğinden yazılım bütünlüğü kontrolü gerekir.
- Yazılım bileşenlerinin (SBOM) ve sürümlerinin izlenmesi için süreç tanımı eksik olabilir.
- Siber olay müdahale planı ve kanıt koruma prosedürü tanımlı olmayabilir.

### O — Fırsatlar (Opportunities)

- **IEC 62443** zone/conduit yaklaşımıyla ağ bölümleme: emniyet bölgesinin (Z1) ağdan ayrılması.
- Uzak erişimde jump server + VPN + MFA + kişisel hesap + tam oturum kaydı + varsayılan kapalı ilkelerinin uygulanması.
- **AB Siber Dayanıklılık Yasası (CRA)** ile uyumlu ürün güvenliği sunarak ihracat pazarında güven avantajı.
- SBOM ve zafiyet yönetimi ile yama süreçlerinin düzene girmesi.
- Öngörücü bakım (titreşim, iş mili yükü izleme) ile plansız duruşların azaltılması.

### T — Tehditler (Threats)

- İnternete açık uzak servis yoluyla kapı kilidi atlatma veya hız/emniyet parametresi değiştirme.
- Mühendislik laptop'u veya USB/DNC ile gelen zararlı yazılım ve hatalı program.
- Ofis ağından HMI/SCADA istemcisine ulaşıp ATC gibi hareketleri tetikleme.
- Kritik zafiyet (ör. CVSS 9.8) bulunan bileşenlerin yama gecikmesi.
- Mevzuat baskısı: Makine Emniyeti Yönetmeliği, AB Makine Yönetmeliği (2023/1230) ve CRA yükümlülükleri.

## 4. RAMS İlkeleri

RAMS, bir sistemin **güvenilirlik, kullanılabilirlik, bakım yapılabilirlik ve emniyet** özelliklerinin bütünleşik yönetimidir (demiryolu alanında EN 50126 ile yaygınlaşmış, endüstriyel makinelerde de yaşam döngüsü yaklaşımı olarak kullanılır).

### 4.1 Güvenilirlik (Reliability)

Makinenin belirli koşullarda, belirli sürede arızasız çalışma olasılığıdır.

| Konu | Makineye uygulaması |
|---|---|
| Ölçüt | MTBF (arızalar arası ortalama süre) |
| Kritik bileşenler | İş mili rulmanları, eksen vidalı miller/lineer kızaklar, servo sürücüler, ATC mekanizması |
| Riskler | Aşınma, aşırı ısınma, kesme sıvısı ve talaş kirliliği, yazılım hataları |
| Önlemler | Yük/ısı izleme, periyodik yağlama, kritik yedek parça listesi, firmware sürüm kontrolü |
| Siber bağlantı | Zararlı yazılım veya bozuk firmware güvenilirliği doğrudan düşürür. |

### 4.2 Kullanılabilirlik (Availability)

Makinenin ihtiyaç duyulduğunda çalışır durumda olma oranıdır.

```text
Kullanılabilirlik = MTBF / (MTBF + MTTR)
```

| Konu | Makineye uygulaması |
|---|---|
| Duruş nedenleri | Arıza, takım değişimi, planlı bakım, güvenlik kaynaklı durma |
| Önlemler | Yedek parça stoğu, hızlı takım/ATC servisi, planlı bakım penceresi |
| Siber bağlantı | Fidye yazılımı veya yetkisiz erişim üretimi durdurabilir; yedekleme ve geri yükleme planı gerekir. |
| Denge | Aşırı sık emniyet durması kullanılabilirliği düşürür; koruma tasarımı kullanılabilirlik gözetilerek yapılmalıdır (aksi halde operatör korumayı atlatma eğiliminde olur). |

### 4.3 Bakım Yapılabilirlik (Maintainability)

Arızanın tespit edilip kısa sürede onarılabilmesi, yani MTTR'nin düşük tutulmasıdır.

| Konu | Makineye uygulaması |
|---|---|
| Tasarım | Modüler sürücüler, erişilebilir ATC ve iş mili, açık alarm kodları |
| Süreç | Bakım talimatları, sorumlu ve kayıt (iş emri), bakım sonrası yeniden doğrulama |
| Güvenli bakım | Kilitleme/etiketleme (LOTO), ayar modunda onay şalteri ve azaltılmış hız |
| Uzak bakım | Jump server + VPN + MFA + kişisel hesap + oturum kaydı ile varsayılan kapalı, yalnız planlı bakımda süreli açılan erişim |
| Siber bağlantı | Yama/güncelleme önce test ortamında denenir, emniyet parametre raporu öncesi-sonrası karşılaştırılır. |

### 4.4 Emniyet (Safety)

Makinenin insana zarar vermemesi; arızada güvenli duruma (safe state) geçmesidir.

| Konu | Makineye uygulaması |
|---|---|
| Fonksiyonlar | SF1 kilitli kapı · SF2 acil stop → STO · SF3 ayar modu onay şalteri + azaltılmış hız |
| Güvenilirlik seviyesi | Risk analizine göre PLr / SIL belirlenir (kilitli kapı için genelde PL d ≈ SIL 2 beklenir). |
| Safe state | İş mili ve eksenler torksuz (STO), Z freni devrede, ATC durmuş, kapı iş mili durmadan açılmaz |
| Bağımsızlık | Emniyet fonksiyonu kontrol sisteminden ve uzak erişimden bağımsız, kablolu çalışmalıdır. |
| Siber bağlantı | Zincir "veri çalındı" ile bitmez: saldırının hattaki insana fiziksel etkisi değerlendirilir (S=3 senaryolar öne alınır). |

## 5. SWOT ile RAMS İlişkisi

| SWOT öğesi | Etkilenen RAMS boyutu | Yaklaşım |
|---|---|---|
| Ayrı emniyet yolu (S) | Emniyet | Korunmalı, ağdan bağımsız tutulmalı |
| Uzak erişim kesişimi (W) | Emniyet, Kullanılabilirlik | Zone/conduit ile ayrıştır, erişimi sertleştir |
| Parametre yükleme laptop'u (W) | Güvenilirlik, Emniyet | Bütünlük kontrolü, yetki ve kayıt |
| Öngörücü bakım (O) | Güvenilirlik, Kullanılabilirlik, Bakım yapılabilirlik | Sensör verisiyle planlı müdahale |
| SBOM / yama yönetimi (O) | Güvenilirlik, Emniyet | Önce test, sonra planlı uygulama |
| Zararlı yazılım / USB (T) | Güvenilirlik, Kullanılabilirlik | USB/DNC kısıtı, tarama, beyaz liste |
| Uzak servis ele geçirilmesi (T) | Emniyet | Safety PLC ağa bağlanmaz, parametre uzaktan yazılamaz |

## 6. Laboratuvar Çıktıları

Bu analizlerin dayanağı olan laboratuvar föyleri (Gürmak CNC için doldurulmuş) şunlardır:

| Lab | Konu | Çıktı |
|---|---|---|
| Lab 1 | Safety Architecture | Safety Function Map |
| Lab 2 | IEC 62443 Mini Workshop | Zone / Conduit Diagram |
| Lab 3 | Threat & Risk + Risk Matrix | Risk Register |
| Lab 4 | Secure Remote Access | Secure Architecture |
| Lab 5 | SBOM / CVE / Patch | Vulnerability Decision |

Risk Register özeti (skor = L × S × E):

| Senaryo | L | S | E | Skor | Öncelik |
|---|---|---|---|---|---|
| Uzak servis ele geçirilip kapı koruması atlatılır, operatör takıma yakalanır | 3 | 3 | 2 | 18 | Kritik |
| Zararlı laptop → hatalı program → takım kırılır/fırlar | 2 | 3 | 2 | 12 | Yüksek |
| Ofis ağı → HMI/SCADA → ATC tetiklenir, el ezilir | 1 | 3 | 2 | 6 | Orta (S=3, öne alınır) |

## 7. Varsayımlar ve Sınırlar

- Makinenin gerçek ağ yapısı, uzak erişim yapılandırması ve PLr/SIL değerleri üreticiden doğrulanmamıştır; ilgili ifadeler **varsayımdır**.
- L, S, E puanları eğitim amaçlı, öğrenci değerlendirmesidir.
- Bu çalışma bir eğitim çıktısıdır; resmî risk değerlendirmesi veya uygunluk belgesi yerine geçmez.
- Tasarım kararları ve çözüm seçimi üreticiye aittir; bu belge gereksinimleri tanımlar, çözüm dayatmaz.

## 8. Kaynaklar

- IEC 62443 serisi: Endüstriyel otomasyon ve kontrol sistemlerinde siber güvenlik
- ISO 13849-1 / IEC 62061: Makinelerde emniyetle ilgili kontrol sistemleri (PL / SIL)
- EN 50126: RAMS yaşam döngüsü yaklaşımı
- AB Makine Yönetmeliği (EU) 2023/1230
- AB Siber Dayanıklılık Yasası (Cyber Resilience Act, CRA)
- Safety 4.0 Bootcamp laboratuvar föyleri (Lab 1–5)

---

**Hazırlayan:** Osman Kerim Ögütçü · **Ders / Hoca:** Yazılım Mühendisliği ve Güncek Konular/Prof. Dr.FATİH ÖZKAYNAK  
