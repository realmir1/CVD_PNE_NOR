
# COVID-19, Pneumonia ve Normal Göğüs Röntgeni Sınıflandırma

Bu projede, COVID-19, Pneumonia ve Normal göğüs röntgeni görüntülerini sınıflandıran bir derin öğrenme modeli geliştirilmiştir. Model, TensorFlow ve Keras kullanarak oluşturulmuş ve görsellerin sınıflandırılması amacıyla bir evrişimsel sinir ağı (CNN) uygulanmıştır.

## Gerekli Kütüphaneler

Proje için aşağıdaki Python kütüphaneleri gereklidir:

- TensorFlow
- NumPy
- Matplotlib
- 

Bu kütüphaneleri yüklemek için aşağıdaki komutları kullanabilirsiniz:

```bash
pip install tensorflow numpy matplotlib
```

## Veri Seti

Bu projede kullanılan veri seti, COVID-19, Pneumonia ve Normal sınıflarına ait göğüs röntgeni görüntülerinden oluşmaktadır. Veri seti Kaggle üzerinden temin edilebilir. Veri seti üç ana sınıftan oluşur:

- **COVİD**: COVID-19 hastalığına ait röntgen görüntüleri.
- **Normal**: Sağlıklı bireylerin röntgen görüntüleri.
- **Pneumonia**: Zatürre hastalığına ait röntgen görüntüleri.

### Veri Kümesi Konumu

Veri setinin bulunduğu ana dizin, `base_dir` olarak belirtilmiştir. Aşağıdaki gibi dizini değiştirerek, verinizi doğru şekilde gösterebilirsiniz.

```python
base_dir = "/path/to/dataset"
```

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

## Eğitim ve Test

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

## Sonuçlar

Eğitim ve test doğruluk oranlarını ve modelin görsel tahminlerini gösteren grafikler, modelin performansını değerlendirmek için kullanılır.



