# Anadolu Hayat Emeklilik Datathon 2024

This repo contains the EDA-Preprocessing-Model notebooks that we developed as the Confidence Interval team for the Anadolu Hayat Sigorta Datathon competition.

+ **[EDA Notebook](https://github.com/enesmanan/anadolu-hayat-emeklilik-datathon/blob/main/anadolu-hayat-eda.ipynb)**
+ **[Model Notebook](https://github.com/enesmanan/anadolu-hayat-emeklilik-datathon/blob/main/anadolu-hayat-model.ipynb)**

**Furthermore**
For an Medium article and Kaggle notebook on dealing with imbalanced datasets, you can find them below.

+ [Medium Article](https://medium.com/@enesmanan768/dengesiz-veri-setlerinde-modelleme-a2830e05e0ff)
+ [Kaggle Notebook](https://www.kaggle.com/code/smailyarknkavalc/classification-in-imbalanced-data)

####  Owners:✨

+ [Enes Fehmi Manan](https://github.com/enesmanan)
+ [İsmail Yarkın Kavalcı](https://github.com/Yarkin02)


## Proje Açıklaması:

Bu proje, Hayat Sigorta Emeklilik Datathon'u için geliştirilmiştir. Bu projenin amacı, 
6 Aylık, mayıs - ekim ayları arası kişilerin tercih ettikleri hayat sigortası ürünlerini(Label) kullanarak, kasım ve aralık aylarında tercih edecekleri ürünleri tahmin etmek.

## Kullanılan Teknolojiler:

- Kaggle Notebook
- Numpy
- Pandas
- Seaborn
- Matplotlib
- XGBoost
- Scikit-learn
- Imblearn
- Collections

## Veri Kümesi:

Hayat Sigorta Emeklilik Firmasının bize sunduğu veri seti, %80'e %20 Train, test şeklinde verilmiştir. Train seti 852719 satır, 97 sütundan oluşmaktadır. Test seti 195631 satır, 96 sütundan oluşmaktadır. Train ve test setine ek olarak
Excel dosyası içerisinde Label değişkenindeki kategorilerin özellikleri verildi. Verilen değişkenler; Kişilerin demografik, sigorta tercihleri, finansal durumları ile ilgili özelliklerinden bahsetmektedir. 

## Denenen bazı teknikler:

- Farklı boosting algoritmaları
- Farklı sampling algoritmaları
- Farklı imputer teknikleri
- Excel dosyasını mergelemek
- Robust encoder
- Aykırı değer temizliği
- Parametre tuning
- Eğtitim datasını validasyon için bölmemek
- Yeni feature oluşturma
Tüm yaklaşımlar farklı farklı çıktılar verse de aralarında gözle görülür bir fark elde edemedik.

## Uygulanılan Adımlar:

### 1. Veri Keşfi ve Temizliği:
- Veri setini inceleyerek eksik veya anormal değerlerin tespit edilmesi.
- Tespit edilen eksik değerlerin IterativeImputer ile tahmin edilerek doldurulması. Bazı değişkenlerde ise rastgele şekilde doldurulması.
- Veri setindeki eksik değer oranı fazla olan değişkenlerin doldurulması yerine silinmesi.

### 2. Veri Görselleştirme:
- Veri setindeki ilişkilerin, dağılımların ve desenlerin görselleştirilmesi.
- Özellikler arasındaki ilişkilerin ve etkileşimlerin anlaşılması için grafikler oluşturulması.
- Bağımlı değişkendeki(Label değişkeni) dengesizliğin görselleştirilmesi.

### 3. Özellik Mühendisliği:
- Verilen Excel dosyasındaki özelliklerin mergelenerek modele katkı sağlayıp sağlamadığının gözlemlenmesi.(Modele katkısı olmadığı için silindi.)
- Var olan değişkenlerden yeni değişkenler türetilmesi.
- Değişkenlerin encodelenmesi.
- Kategorik değişkenlerin alt kategorilerinde frekansı düşük olan kategorilerin "Diğer", "Other" olarak gruplandırılması veya Mod değere eklenmesi.
- Label değişkenindeki kategorilerin frekansları şu şekildedir;

  [UA = 832770, HU14 = 10754, HU07 = 3728, HU06 = 3178, HU19 = 759, HU12 = 676, HU11 = 439, HU15 = 415]

- Bu dengesizliğin giderilmesi için ilk önce UA kategorisinden 75000 adet rastgele olarak örneklem seçilmiştir.
- Daha sonrasında, sampling_strategy = {0: 67461, 1: 15000, 2: 5050, 3: 4000, 4: 1250, 5: 1250, 6: 850, 7: 750} deneme-yanılma yöntemi ile belirlenmiştir.
- Belirlenen bu strateji ile birlikte SMOTE yöntemi kullanılarak UA harici kategorilerde sentetik veriler türetilmiştir.
- Oversampling sonrası kategori frekansları şu şekildedir;
  
  Before oversampling:  Counter({0: 67461, 1: 9704, 2: 3357, 3: 2877, 4: 678, 5: 608, 6: 401, 7: 368}), After oversampling:  Counter({0: 67461, 1: 15000, 2: 5050, 3: 4000, 4: 1250, 5: 1250, 6: 850, 7: 750})

### 4. Model Geliştirme
- XGBoost, Lightgbm, Catboost gibi Farklı algoritmalar denenmiştir. XGBoost diğer algoritmalara göre daha iyi performans gösterdiği için tercih edilmiştir.
- Hipertarametre optimizasyonu ve çapraz doğrulama gibi teknikler kullanarak modellerin iyileştirilmesi ve overfitting durumu incelenmiştir.
- Overfitting durumunu engellemek için XGBoost algoritmasındaki, reg_lambda parametresi kullanılmış yani L2 Regularizationu uygulanmıştır.
- Fakat modelin karmaşıklığından dolayı parametre tuning işlemlerinde sağlıklı bir sonuç yakalanamadığından sadece reg_lambda değeri belirtilmiş basit XGBoost modeli tercih edilmiştir.

### 5. Sonuçları Değerlendirme
- Bu aşamada Hayat sigorta emeklilik tarafından belirlenen katsayılar nezlinde Ağırlıklı F1-Skor metriği ile modeller değerlendirilmiştir. Belirtilen katsayılar şu şekildedir;

  [UA = 0.0001, HU14 = 0.0113, HU07 = 0.0328, HU06 = 0.0385, HU19 = 0.1614, HU12 = 0.1812, HU11 = 0.2791, HU15 = 0.2952]

- Bu katsayılar kullanılarak elde edilen Ağırlıklı F1-Skor hesaplandığında LB'deki Public score ile uyuşmadığından modelin asıl performansını atılan Submissionlar sonrasında ölçebilme imkanı bulabildik.

## Sonuç:

Public LB'de elde ettiğimiz 0.11360 Ağırlıklı F1-Skoru ile 185 Grup arasından 15. olduğumuz yarışmayı, Private LB'de seçilen yanlış Submission sonucu 26. tamamladık.
