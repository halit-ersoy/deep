# Gelecekteki Elektrik Tüketimi Tahmin Sistemi (LSTM Tabanlı)

Ders: Derin Öğrenme (Deep Learning)
Yazar: Halit Ersoy
Kurum: İstanbul Medeniyet Üniversitesi

## 1) Proje Konusu ve Gerekçesi
### 1.1 Proje Konusu
Bu proje, geçmiş saatlik elektrik tüketim verilerinden yararlanarak bir sonraki saatin elektrik tüketimini tahmin eden
zaman serisi tabanlı bir Derin Öğrenme uygulamasıdır. Amaç; enerji talebinin kısa vadeli öngörüsünü yaparak,
kaynak planlaması ve şebeke yönetimi gibi süreçlere katkı sağlayabilecek bir tahmin sistemi geliştirmektir.

### 1.2 Seçilme Gerekçesi
- Elektrik tüketimi, günlük/haftalık/mevsimsel döngüler ve ani değişimler içeren karmaşık bir zaman serisidir.
- Kısa vadeli yük tahmini; üretim planlaması, maliyet optimizasyonu ve arz-talep dengesinin korunması açısından kritiktir.
- LSTM gibi tekrarlayan sinir ağları, ardışık verilerdeki bağımlılıkları öğrenmede güçlü olduğu için bu probleme uygundur.

### 1.3 Daha Önce Yapılan Uygulamalar
Literatürde kısa vadeli yük tahmini için yaygın yaklaşımlar:
- Klasik yöntemler: ARIMA/SARIMA, üstel düzeltme (ETS), regresyon tabanlı yaklaşımlar
- Makine öğrenmesi: SVR, Random Forest, XGBoost/LightGBM gibi gradyan artırma yöntemleri
- Derin öğrenme: RNN, LSTM/GRU, CNN-LSTM hibritleri, Transformer tabanlı modeller

Bu projede hedef, saatlik tüketim verisi üzerinde LSTM’in zaman bağımlılıklarını yakalama kabiliyetini kullanarak
anlaşılır ve gösterilebilir (demo yapılabilir) bir uçtan uca sistem ortaya koymaktır.

### 1.4 İlgili Alanın Önemi
- Şebeke işletmeciliğinde dengesizliklerin azaltılması
- Üretim/dağıtım planlamasında verimlilik
- Enerji maliyetlerinde optimizasyon
- Yenilenebilir entegrasyonunda belirsizliğin yönetimi

## 2) Veri Setinin Belirlenmesi
### 2.1 Veri Seti
Bu projede PJM Interconnection bölgesine ait saatlik elektrik tüketim verisi kullanılmıştır:
- Dosya adı: PJME_hourly.csv
- Veri tipi: Saatlik zaman serisi
- Odak: PJME (PJM East) bölgesi saatlik tüketim değerleri

### 2.2 Veri Seçim Gerekçesi
- Saatlik çözünürlük, kısa vadeli tahmin için uygundur.
- Zaman serisi tahmin çalışmalarında yaygın kullanılan, düzenli ve öğrenmeye elverişli bir veri yapısına sahiptir.
- Sezonsallık, trend ve kısa dönem dalgalanmalar gibi gerçek dünya özelliklerini barındırır.

### 2.3 Ön İşleme
- Eksik değer kontrolü ve temel temizlik adımları
- Zaman serisinin sıralı olarak hazırlanması
- Model girişi için kayan pencere (sliding window) yaklaşımı:
  - Girdi: Son 24 saatlik tüketim (sequence length = 24)
  - Çıktı: Bir sonraki saat tüketimi (t+1)
- Ölçekleme: MinMaxScaler ile normalizasyon (scaler.gz olarak kaydedilir)

## 3) Yöntem/Algoritma/Yaklaşım Seçimi ve Gerekçesi
### 3.1 Neden LSTM?
Zaman serilerinde geçmiş gözlemler, geleceği etkiler. LSTM, klasik RNN’lere göre uzun bağımlılıkları daha iyi öğrenebilir
(gradient kaybolması problemini azaltan kapı (gate) yapıları sayesinde). Saatlik tüketim gibi döngüsel ve bağımlı serilerde
LSTM pratikte güçlü bir temel yaklaşımdır.

### 3.2 Alternatif Yöntemlerle Karşılaştırma
- ARIMA/SARIMA:
  - Artı: Yorumlanabilir, küçük veriyle çalışabilir
  - Eksi: Doğrusal varsayımlar, karmaşık desenlerde sınırlı performans
- SVR / Random Forest / XGBoost:
  - Artı: Güçlü kestirim, iyi ayarlamayla yüksek performans
  - Eksi: Zaman bağımlılığını modellemek için özellik mühendisliği ihtiyacı artar
- GRU:
  - Artı: LSTM’e göre daha hafif, hızlı eğitim
  - Eksi: Bazı senaryolarda uzun bağımlılıklarda LSTM kadar güçlü olmayabilir
- Transformer tabanlı modeller:
  - Artı: Uzun menzilli bağımlılıklarda güçlü, paralel eğitim
  - Eksi: Daha fazla veri/hesaplama ve daha karmaşık kurulum

Bu projede LSTM; performans, uygulanabilirlik, anlaşılabilirlik ve demo kolaylığı açısından dengeli bir tercih olarak seçilmiştir.

## 4) Model Eğitimi ve Model Değerlendirmesi
### 4.1 Model Mimarisi (Özet)
- Girdi: 24 zaman adımı (son 24 saat)
- Katmanlar:
  - 2 adet LSTM katmanı (stacked)
  - Hidden size: 64
  - Dropout: 0.2
- Çıkış:
  - Linear katman ile 1 adet sürekli değer

### 4.2 Eğitim Süreci
- Eğitim dosyası: DeepLearning_HalitErsoy.ipynb
- Eğitim modu kontrolü:
  - TRAIN_MODE = True  -> modeli sıfırdan eğitir (varsayılan: 50 epoch)
  - TRAIN_MODE = False -> eğitilmiş modeli yükler
- Kayıtlar:
  - Model ağırlıkları: advanced_lstm_model.pth
  - Ölçekleyici: scaler.gz

### 4.3 Değerlendirme Metrikleri
Model performansı aşağıdaki metriklerle ölçülür:
- RMSE (Root Mean Squared Error)
- MAE (Mean Absolute Error)
- R² (Determinasyon Katsayısı)

### 4.4 Görselleştirmeler
- Eğitim kaybı (loss) grafiği
- Tahmin vs. gerçek tüketim karşılaştırma grafikleri

Not: Elde edilen metrikler veri ayrımı, epoch sayısı, hiperparametreler ve rastgelelik (seed) gibi faktörlere göre değişebilir.

## 5) Proje Dokümantasyonu ve Repo Düzeni
Bu proje; kod, veri, eğitim çıktıları ve dokümantasyonun düzenli bir şekilde saklanabilmesi için GitHub/Bitbucket gibi bir
platforma uygun bir yapı ile hazırlanmıştır.

Önerilen dosya yapısı:
- DeepLearning_HalitErsoy.ipynb  -> Eğitim + değerlendirme + demo
- PJME_hourly.csv                -> Veri seti
- advanced_lstm_model.pth        -> Eğitilmiş model ağırlıkları (eğitim sonrası oluşur)
- scaler.gz                      -> Ölçekleyici nesnesi (eğitim sonrası oluşur)
- README.md                      -> Bu doküman

## Kurulum
Gerekli kütüphaneleri yüklemek için:

pip install torch pandas numpy matplotlib scikit-learn gradio joblib

## Çalıştırma
1) DeepLearning_HalitErsoy.ipynb dosyasını açın.
2) TRAIN_MODE ayarını ihtiyacınıza göre True/False yapın.
3) Tüm hücreleri sırayla çalıştırın.
4) Gradio arayüzü için son hücreyi çalıştırın ve verilen lokal bağlantıyı açın.

## İletişim
Halit Ersoy
İstanbul Medeniyet Üniversitesi - Bilgisayar Mühendisliği
