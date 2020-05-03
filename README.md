# Odev_DerinOgrenme_Cifar100_Proje
Sakarya üniversitesi-Bilgisayar Mühendisliği

Bu projede Derin öğrenmenin özelleşmiş yapılarından evrişimsel sinir ağları(CNN) ve uygulama olarak Jupyter Notebook kullanıldı. CNN, katmanlı yapısıyla gizli öznitelikleri çıkararak işlem yapmaktadır. Gizli katman sayısının artışıbir noktaya kadar olumlu etki yaparken sonrasında parametre artışından dolayı bulma hızını düşürdüğü için performansa olumsuz yönde etki etmektedir. 6 katman (4 konvolüsyon katmanı + 2 tam bağlantı katmanı (Dense Layer) ) her bir katmandaki hücre sayısı 16-256 arasında olacak şekilde kullanıldı. CIFAR-100 veri setinden öğrenci numarama göre belirlenen 6 farklı sınıf: 0=apple 3=bear 44=lizard 59=pine_tree 89=tractor 91=trout . Her sınıf için toplamda eğitim işlemleri için 500 ve test için 100 adet mevcut.

Keras kullanıldığı için Keras'ta bu keras.preprocessing.image.ImageDataGenerator sınıf aracılığıyla Train sırasında görüntü verilerinde yapılacak rastgele dönüşümleri ve normalleştirme işlemi yapmaktadır. Not: komutların detaylı açıklaması resimlerde kodların açıklaması olarak yazılmaktadır.
Önce Normal bir model ile Ezberlemeyi azaltmak için sadece Dropout ile model oluşturuldu ve ağ eğitildi.
Not: Düğüm Seyreltme (Dropout Layer): hidden ya da input layerdan belli kuralla göre (eşik değeri kullanarak ya da rastgele) belli nodelar kaldırılması tekniğidir. Bu projede son katman için 0.5 dropout(seyretme) ağ modelimize uygulandığında dropout uygulanan gizli katmandaki node sayısı bir sonraki katmanda yarıya düşürüldü. 
Dropout tekniği genelde tam bağlı katmanlarda (fully-connected layer) sonra kullanılır. Dropout kullanılarak fully-connected layerlardaki bağlar koparılır. her bir katmanda farklı hidden unit kombinasyonları birbiriyle çalıştığı için daha iyi bir öğrenme gerçekleşecektir ve derin öğrenme yöntemlerinde en sık kullanılan iyileştirme (regularization) yöntemlerinden biridir.

Grafikte de görüleceğiniz loss değerinde dalgalanmalar mevcut, nedeni her iterasyonda farklı veri işlendiği için bazı iterasyondaki veriler için seçilen parametreler tam uygun iken bazısında uygun olmayabilir.iterasyon arttıkça bu zikzaklar azalacaktır. 

Veri ön işleme ve veri zenginleştirme (Augmentation)
Verinin fazla olması model başarımı (accuracy) artımının yanı sıra ezberlemeyi (overfitting) de önleyecektir.Görüldüğü üzere eğitim setinden en iyi şekilde yararlanmak için, onları bir dizi rastgele dönüşüm yoluyla arttırılınca modelde asla aynı resmin iki katını olmaz ve bu, aşırı uydurmayı önlemeye ve modelin daha iyi genellemesine yardımcı olur. Bu sefer ağımızı dropout kuullanmadan (make_model(dropout=False) ), sadece Augmentation kullanarak eğitildi. Veri artırma, aşırı sığdırma ile mücadele etmenin bir yoludur, ancak artırılmış örneklerimiz hala yüksek derecede korelasyonlu olduğundan yeterli değildir. 
Not: Aşırı sığdırma ile mücadeleye odaklanmanız, modelinizin entropik kapasitesi olmalıdır.
Veri zenginleştirmeden sonra: Veri artırımı uygulandıktan sonra yeni eğitim veri setinde benzer(relevant) örneklerin sayısı artmış oldu ve sonuç olarak veri artırımı, modelin ezberleme(overfit) yani aşırı uydurma probleminin de önüne geçmiş oldu. Aşırı uydurma, çok az örneğe maruz kalan bir model yeni verilere genelleme yapmayan desenleri öğrendiğinde, yani model tahmin yapmak için alakasız özellikler kullanmaya başladığında gerçekleşir.Grafikte görüldüğü üzere veri artırımı yöntemleri uygulandıktan sonra, test veri seti sınıflandırma doğruluğu(accuracy) artmış ve loss azalmıştır.
 Bu modeli 34 devirde (epoch) eğitildi, Eğitim sonucunda eğitim verisi için 0.743  doğruluğuna (accuracy), test verisi için ise 0.805 doğruluğa ulaşıldı. Modeli daha çok devirde eğiterek model doğruluğunu artırabilir. 



