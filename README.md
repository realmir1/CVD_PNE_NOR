
# COVID-19, Pneumonia ve Normal Göğüs Röntgeni Sınıflandırma

Bu projede, COVID-19, Pneumonia ve Normal göğüs röntgeni görüntülerini sınıflandıran bir derin öğrenme modeli geliştirilmiştir. Model, TensorFlow ve Keras kullanarak oluşturulmuş ve görsellerin sınıflandırılması amacıyla bir evrişimsel sinir ağı (CNN) uygulanmıştır.

## Gerekli Kütüphaneler

Proje için aşağıdaki Python kütüphaneleri gereklidir:

- TensorFlow
- NumPy
- Matplotlib
- Keras
<br>
<div align="Center">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/numpy/numpy-original.svg" height="50" alt="numpy logo"  />
  <img width="12" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/tensorflow/tensorflow-original.svg" height="50" alt="tensorflow logo"  />
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/01/Created_with_Matplotlib-logo.svg/2048px-Created_with_Matplotlib-logo.svg.png" height="50" alt="plotlib logo"  />
  <img width="12" />
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Keras_logo.svg/1200px-Keras_logo.svg.png" height="50" alt="plotlib logo"/>
  <img width="12" />
</div>

## Keras

Keras, derin öğrenme (deep learning) uygulamaları geliştirmek için kullanılan açık kaynaklı bir Python kütüphanesidir. Başlangıçta Theano ve TensorFlow gibi arka uç kütüphanelerine dayanarak çalışıyordu, ancak günümüzde TensorFlow'un yüksek seviyeli API'si olarak entegre edilmiştir. Keras, kullanıcı dostu ve modüler bir yapıya sahip olup, hızlı prototipleme, eğitim ve derin öğrenme modellerinin geliştirilmesi için idealdir.

## Numpy 

NumPy, Python dilinde büyük sayılar ve çok boyutlu diziler üzerinde hızlı ve etkili matematiksel işlemler gerçekleştirmeye olanak sağlayan bir python kütüphanedir. NumPy, aynı zamanda Python'da matematiksel işlemler yapmak için kullanılan diğer kütüphanelerle uyumlu bir şekilde çalışır.

## Tensorflow

TensorFlow, makine öğrenimi için ücretsiz ve açık kaynaklı bir yazılım kütüphanesidir . Bir dizi görevde kullanılabilir, ancak derin sinir ağlarının eğitimi ve çıkarımına özel olarak odaklanmaktadır

##Matplotlib




##  Örnek Doğruluk Sonucu
<br>
<div align="Center">
  <img src="https://github.com/realmir1/CVD_PNE_NOR/blob/main/Ekran%20Resmi%202025-01-13%2017.23.39.png?raw=true" height="350" alt="numpy logo"  />
</div>

<br>

## Veri Seti

Bu projede kullanılan veri seti, COVID-19, Pneumonia ve Normal sınıflarına ait göğüs röntgeni görüntülerinden oluşmaktadır. Veri seti Kaggle üzerinden temin edilebilir. Veri seti üç ana sınıftan oluşur:

- **COVİD**: COVID-19 hastalığına ait röntgen görüntüleri.
- **Normal**: Sağlıklı bireylerin röntgen görüntüleri.
- **Pneumonia**: Zatürre hastalığına ait röntgen görüntüleri.

## Kütüphelerin Kullanıldığı Yerler

| Kütüphaneler       | Kullanım Alanı       |
|--------------------|----------------------|
| TensorFlow         | Derin Öğrenme        |
| Keras              | Derin Öğrenme        |
| Matlpotlib         | Grafik İşleme        |
| Numpy              | İşleme               | 

## Model Mimarisi

Model, aşağıdaki evrişimsel katmanları içeren basit bir CNN yapısı ile oluşturulmuştur:

1. **Conv2D**: İlk evrişimsel katman (32 filtre, 3x3 çekirdek boyutu).
2. **MaxPooling2D**: Maksimum havuzlama katmanı.
3. **Conv2D**: İkinci evrişimsel katman (64 filtre, 3x3 çekirdek boyutu).
4. **MaxPooling2D**: Maksimum havuzlama katmanı.
5. **Conv2D**: Üçüncü evrişimsel katman (128 filtre, 3x3 çekirdek boyutu).
6. **MaxPooling2D**: Maksimum havuzlama katmanı.
7. **Flatten**: Çok boyutlu veriyi tek boyutlu vektöre dönüştürme katmanı.
8. **Dense**: Tam bağlantılı katman (128 nöron).
9. **Dense**: Çıkış katmanı, softmax aktivasyon fonksiyonu ile çok sınıflı sınıflandırma.

### Model Derinliği

- **Optimizasyon**: Adam optimizasyon algoritması.
- **Kayıp Fonksiyonu**: Categorical Crossentropy.
- **Metriği**: Doğruluk (`accuracy`).

## Eğitim ve Test Hakkında Bilgi
Dataların %10 u test amaçl, %90 ı eğitim amaçlı kullanılmıştır.
Model, aşağıdaki gibi eğitilmiştir:

```python
history = model.fit(train_data, validation_data=test_data, epochs=10)
```

Eğitim tamamlandıktan sonra, doğruluk grafiği çizilir ve modelin başarımı görselleştirilir.

```python
plt.plot(history.history['accuracy'], label='Eğitim Doğruluk Oranı')
plt.plot(history.history['val_accuracy'], label='Test Doğruluk Oranı')
plt.title('Model Doğruluk Oranı')
plt.xlabel('Epok (Epoch)')
plt.ylabel('Doğruluk')
plt.legend()
plt.show()
```

### Görsel Tahmin

Eğitim sonrasında, test verisi üzerinde tahminler yapılır ve tahmin edilen sınıf etiketleri ile gerçek sınıflar karşılaştırılır:

```python
test_images, test_labels = next(test_data)
predictions = model.predict(test_images)
```

Sonuçlar, aşağıdaki gibi görselleştirilir:

```python
plt.figure(figsize=(15, 5))
for index in range(5):
    plt.subplot(1, 5, index + 1)
    plt.imshow(test_images[index])
    plt.title(f"Tahmin: {classes[np.argmax(predictions[index])]} \nGerçek: {classes[np.argmax(test_labels[index])]}")
    plt.axis('off')
plt.show()
```
