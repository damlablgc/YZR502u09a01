# YZR502 — Robotik Görüntü İşleme ve Nesne Tanıma için Parametre Analizi

## Öğrenci Bilgileri

| **Ad Soyad** | [Damla Bilgiç] |
| **Ders** | YZR502 Robotik Sistemler ve Algoritmalar |
| **Dönem** | 2024-2025 Bahar |
| **E-posta** | [damlaazrabilgic@gmail.com] |

## Proje Açıklaması

Bu proje, Google Colab ortamında OpenCV kullanarak robotik görü senaryosu üzerinde görüntü işleme parametrelerinin algılama performansına etkisini analiz etmektedir.

İşlem hattı şu adımlardan oluşmaktadır:

1. **Gauss Yumuşatma** — Gürültü azaltma (`cv2.GaussianBlur`)
2. **CLAHE** — Kontrast iyileştirme (`cv2.createCLAHE`)
3. **Canny Kenar Tespiti** — Kenar haritası çıkarma (`cv2.Canny`)
4. **Kontur Analizi** — Alan/dairesellik/ağırlık merkezi hesaplama (`cv2.findContours`)
5. **ORB Öznitelik Çıkarma** — Anahtar nokta ve betimleyici (`cv2.ORB_create`)

Kullanılan görüntü: OpenCV örnek veri setinden `butterfly.jpg` (356×493 piksel, gri seviye aralığı: [0, 255])

---

## Repo İçeriği

```
├── YZR502u09a01.ipynb          # Tamamlanmış Colab notebook'u
├── images/
│   ├── sample.jpg                                    # Örnek giriş görüntüsü
│   ├── Deney_1_(Varsayılan).png
│   ├── Deney_2_(Agresif_Kenar).png
│   ├── Deney_3_(Güçlü_Yumuşatma).png
│   ├── Deney_4_(Yüksek_Kontrast).png
│   ├── Deney_5_(Az_Öznitelik_+_Küçük_Alan).png
│   └── karsilastirma_grafigi.png
├── paper/
│   └── YZR502_makale.docx                            # Araştırma makalesi
└── README.md
```

---

## Deney Konfigürasyonları

| Parametre | D1 Varsayılan | D2 Agresif Kenar | D3 Güçlü Yumuşatma | D4 Yüksek Kontrast | D5 Az Öznitelik |
|-----------|:---:|:---:|:---:|:---:|:---:|
| `gaussian_sigma` | 1.5 | 1.5 | **3.0** | 1.5 | 2.0 |
| `clahe_clip` | 2.0 | 2.0 | 2.0 | **5.0** | 3.0 |
| `clahe_grid` | 8 | 8 | 8 | 8 | **16** |
| `canny_low` | 50 | **30** | 50 | 50 | **40** |
| `canny_high` | 150 | **90** | 150 | 150 | **120** |
| `min_area` | 500 | 500 | 500 | 500 | **200** |
| `max_area` | 50000 | 50000 | 50000 | 50000 | **30000** |
| `n_features` | 500 | 500 | 500 | 500 | **300** |

Kalın değerler varsayılandan farklılaştırılan parametreleri göstermektedir.

---

## Sonuç Özeti

| Deney | Toplam Kontur | Geçerli Kontur | ORB Anahtar Nokta | Ort. Dairesellik | Ort. Alan |
|-------|:---:|:---:|:---:|:---:|:---:|
| Deney 1 (Varsayılan) | 334 | 2 | 500 | 0.010 | 839.8 |
| Deney 2 (Agresif Kenar) | 612 | 2 | 500 | 0.010 | 839.8 |
| Deney 3 (Güçlü Yumuşatma) | 316 | 0 | 500 | 0.000 | 0.0 |
| Deney 4 (Yüksek Kontrast) | 574 | 4 | 500 | 0.132 | 638.4 |
| Deney 5 (Az Öznitelik + Küçük Alan) | 648 | 8 | 300 | 0.550 | 340.2 |

### Temel Bulgular

- **Deney 2** (canny_low=30, canny_high=90): Toplam kontur sayısı %83 artmış (334→612) ancak geçerli kontur değişmemiş. Düşük eşik gürültüyü artırıyor, anlamlı bölgeleri değil.
- **Deney 3** (gaussian_sigma=3.0): Aşırı yumuşatma tüm geçerli konturları yok etmiş (2→0). Güçlü blur, ince nesne sınırlarını silmiş.
- **Deney 4** (clahe_clip=5.0): Yüksek kontrast geçerli kontur sayısını 2 katına çıkarmış (2→4) ve daireselliği 13× artırmış (0.010→0.132). CLAHE sınır belirginliğini iyileştiriyor.
- **Deney 5** (min_area=200, n_features=300): En yüksek geçerli kontur (8) ve en yüksek dairesellik (0.550) elde edilmiş. Küçük alan eşiği daha fazla nesneyi kapsıyor; az öznitelik gereksiz kalabalığı azaltıyor.

---

## Kullanılan Teknolojiler

- Python 3.12
- OpenCV 4.13 (`opencv-python`, `opencv-contrib-python`)
- NumPy 2.0
- Matplotlib 3.10
- Google Colab

---

## Lisans

Bu proje YZR502 Robotik Sistemler ve Algoritmalar dersi kapsamında akademik amaçla hazırlanmıştır.
