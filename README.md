# ⚡ Gelecekteki Elektrik Tüketimi Tahmin Sistemi

**Ders:** Derin Öğrenme (Deep Learning)
**Yazar:** Halit Ersoy
**Kurum:** İstanbul Medeniyet Üniversitesi
**Görev:** Bilim ve Mühendislik Topluluğu (BMT) Başkanı

---

## 📖 Proje Özeti

Bu proje, geçmiş **zaman serisi** verilerini kullanarak gelecekteki elektrik tüketimini tahmin etmeyi amaçlayan bir Derin Öğrenme uygulamasıdır. Sistem, saatlik enerji kullanım örüntülerini öğrenmek ve **bir sonraki saatin tüketimini** öngörmek için **Gelişmiş LSTM (Long Short-Term Memory)** mimarisini kullanır.

Proje; uçtan uca eğitim (training) hattını, performans ölçüm metriklerini ve gerçek zamanlı denemeler için **Gradio tabanlı web arayüzünü** içerir.

---

## 🚀 Temel Özellikler

* **Zaman Serisi Analizi:** Saatlik enerji tüketim verilerini işler (PJME bölgesi).
* **Derin Öğrenme Modeli:** PyTorch tabanlı LSTM mimarisi ve aşırı öğrenmeyi azaltmak için Dropout kullanır.
* **Veri Normalizasyonu:** Modelin daha iyi yakınsaması için MinMax ölçekleme uygular.
* **Performans Ölçümü:** RMSE, MAE ve R² skorları ile model doğruluğunu değerlendirir.
* **Etkileşimli Demo:** Gradio arayüzü ile kullanıcı girişine göre anlık tahmin üretir ve görselleştirir.
* **Görselleştirme:** Eğitim kaybı (loss) eğrileri ile gerçek vs. tahmin grafikleri üretir.

---

## 🛠️ Kullanılan Teknolojiler ve Kütüphaneler

* **Python 3.x**
* **PyTorch** (Derin öğrenme çatısı)
* **Pandas & NumPy** (Veri işleme)
* **Scikit-Learn** (Ön işleme ve metrikler)
* **Matplotlib** (Görselleştirme)
* **Gradio** (Web arayüz / demo)
* **Joblib** (Model/Scaler kayıt)

---

## 📂 Proje Yapısı

```
├── DeepLearning_HalitErsoy.ipynb  # Ana Jupyter Notebook (Eğitim + Demo)
├── PJME_hourly.csv                # Veri seti (PJM East Region Hourly MW)
├── advanced_lstm_model.pth        # Eğitilmiş PyTorch model ağırlıkları (eğitim sonrası oluşur)
├── scaler.gz                      # Kaydedilmiş MinMaxScaler nesnesi (eğitim sonrası oluşur)
└── README.md                      # Proje dokümantasyonu
```

---

## ⚙️ Kurulum ve Çalıştırma

1. **Projeyi edinin (klonlayın / dosyaları çıkarın):**
   Tüm dosyaların aynı klasörde olduğundan emin olun.

2. **Gerekli bağımlılıkları yükleyin:**
   Aşağıdaki komut ile gerekli kütüphaneleri kurun:

```
pip install torch pandas numpy matplotlib scikit-learn gradio joblib
```

3. **Veri seti kontrolü:**
   `PJME_hourly.csv` dosyasının proje kök dizininde olduğundan emin olun.

---

## 🧠 Model Mimarisi

Model, sıralı (sequential) veriler için tasarlanmıştır:

* **Girdi (Input):** Normalleştirilmiş son 24 saatlik enerji tüketim dizisi.
* **LSTM Katmanları:**

  * 2 adet ardışık LSTM katmanı
  * Gizli boyut (Hidden Size): 64
  * Dropout: 0.2 (genelleme performansını artırmak için)
* **Çıkış (Output):** Tek bir sürekli değer üreten Linear katman (MW tahmini)

---

## 📊 Kullanım Rehberi

### 1) Modeli Eğitme

`DeepLearning_HalitErsoy.ipynb` dosyasını Jupyter Notebook veya Google Colab ile açın.

* Notebook içinde `TRAIN_MODE` adlı bir bayrak bulunur.
* `TRAIN_MODE = True` yaparsanız model sıfırdan yeniden eğitilir (50 epoch).
* `TRAIN_MODE = False` yaparsanız hazır eğitilmiş `advanced_lstm_model.pth` yüklenir.

### 2) Değerlendirme (Evaluation)

Notebook otomatik olarak aşağıdaki metrikleri hesaplar ve gösterir:

* **RMSE (Root Mean Squared Error):** Tahmin hatalarının standart sapması.
* **MAE (Mean Absolute Error):** Mutlak hata ortalaması.
* **R² Skoru:** Regresyon uyumunu gösteren istatistiksel ölçüm.

### 3) Canlı Demo (Gradio)

Notebook’un son hücresini çalıştırarak Gradio arayüzünü başlatabilirsiniz.

* **Girdi:** Son 24 saate ait tüketim değerleri (virgülle ayrılmış).
* **Çıktı:** Bir sonraki saat için tahmini tüketim (MW) ve trend grafiği.
* **URL:** Çıktıda verilen lokal bağlantıya tıklayın (örn: [http://127.0.0.1:7860](http://127.0.0.1:7860))

---

## 📈 Örnek Sonuçlar

(Test sürecinde tipik olarak gözlemlenen değerler)

* **RMSE:** ~400–600 MW
* **R² Skoru:** > 0.95 (yüksek doğruluk)
* **Eğitim Kaybı (Loss):** 20 epoch sonrasında belirgin şekilde düşer.

---

## 📝 İletişim

**Halit Ersoy** — Bilgisayar Mühendisliği Öğrencisi
İstanbul Medeniyet Üniversitesi
