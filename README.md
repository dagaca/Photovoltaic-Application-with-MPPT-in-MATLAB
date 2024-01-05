# Photovoltaic-Application-with-MPPT-in-MATLAB
Bu repo, Simulink kullanarak Maximum Power Point Tracking (MPPT) özellikli bir Fotovoltaik (PV) sistemini uygular. Parametreler, **'Machine Learning and Deep Learning for Photovoltaic Applications'** adlı kaynaktan elde edilen karakteristiklere dayanmaktadır.

## MATLAB Simulink Şeması

![simulink_sema](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/d2c3fd64-954d-433e-94c9-30cb698245f3)


### PV Array (MSX 83) Blok ve Parametreler

![pvarray](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/db55c459-5657-4812-8893-23ca7f2ea728)


#### Tek Modül için I-V ve P-V Özellikleri (1000W/m2)

![tekmodul](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/6e71ea6d-3204-4994-b210-a63067c92105)


#### Dört Seri Bağlı Modül için I-V ve P-V Özellikleri (1000W/m2)

![dortserimodul](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/72069807-ab42-40ba-86c2-d97ae8f9bdf4)



#### Simülasyon Durumları
PV sistem iki durum altında simüle edildi. Bu durumlardan ilki sabit irradiation, ikincisi değişken irradiation.

⦁	Durum -> G = 1000 W/m2 and T = 25 °C

![sabit_irr](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/f14cd1f5-c1d7-47be-aa82-5281315c5574)


⦁	Durum -> G = 1000 W/m2 ’den 600 W/m2 ‘ye kademeli değişim, ardından 400 W/m2‘ye ani değişim.

![degisken_irr](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/1924fa96-0071-46d5-b85b-0c7d5d275ad7)



## Boost Konvertörler
Sistemde yer alan Diyot ve IGBT Diyot ideal olarak kabul edilerek simülasyon gerçekleştirildi. Aşağıda detayları ve parametreleri yer almaktadır.


### IGBT
Ideal bir IGBT diode için, internal resistance sıfır, snubber resistance ve snubber capacitance değerleri de sıfır olmalıdır. Ancak internal resistance, snubber resistance ve snubber capacitance değerleri için ideal bir IGBT diode kullanırken, discrete zamanlama türünde hata mesajı verir. Discrete zamanlama türünü kullanmaya devam etmek için, bu değerler çok küçük ancak sıfırdan farklı değerler olarak ayarlandı. Bu, ideal bir IGBT diodeyi yaklaşık olarak taklit ederken, hala discrete zamanlama türünün kullanılabilmesini sağladı.

![igbt_parameters](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/822233cf-f67f-4cb1-8245-018248c5c2aa)


### Diyot
Ideal bir diyot için Resistance Ron sıfır (0) olmalıdır. Inductance Lon da aynı şekilde sıfır (0) olmalıdır. Forward voltage Vf değeri diyotun pozitif yönden negatif yöne geçiş sırasında ürettiği gerilim değeridir ve ideal bir diyot için sıfır (0) olmalıdır. Snubber resistance Rs ve Snubber capacitance Cs değerleri ise, diyotun geçiş sırasında ürettiği dalgalanmaları sönümlemek için kullanılan direnç ve kapasitördür. Ideal bir diyot için bu değerler de sıfır (0) olmalıdır. Ancak, discrete zamanlama türünü kullanmaya devam etmek için, bu değerler çok küçük ancak sıfırdan farklı değerler olarak ayarlandı. Bu, ideal bir IGBT diodeyi yaklaşık olarak taklit ederken, hala discrete zamanlama türünü kullanabilmemizi sağladı.

![diode_parameters](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/145c7053-a96b-48d8-aca4-64747b723917)



## MPPT Devre Tasarımı (Kontrol Yapısı)

![mppt](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/266c07ce-f20c-43d0-bb72-7d43f98441f4)


Bu projede kullanılan MPPT algoritması, yaygın olarak kullanılan 3 MPPT algoritmasından bir tanesi olan ‘**Perturbation and observation (P&O)**’ algoritmasıdır.

![mppt_algoritma](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/8d01d88f-5b28-426e-b483-5db4e1ce4a90)



### MPPT MATLAB Kod

![mppt_kod](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/007e4e3c-2602-4903-b6b1-5220dd500cc9)

Bu MPPT algoritması, bir PV panelinin gerilim değerini, panelin verdiği maksimum güç değerine (Vrefmax) ulaştırmak için yönetir. Algoritma, PV panelinin gerilim değerini (V) ve akım değerini (I) girdi olarak alır ve PV panelinin güç değerini (P) hesaplar. Daha sonra, PV panelinin mevcut gerilim değeri ile bir önceki ölçülen gerilim değeri arasındaki fark (dV) ve mevcut güç değeri ile bir önceki ölçülen güç değeri arasındaki fark (dP) hesaplanır.
Eğer dP değeri sıfırdan farklı ise, bu durumda mevcut güç değeri bir önceki güç değerinden farklıdır ve MPPT algoritmasının çalışmasına devam edilir. Eğer dP değeri sıfırdan farklı ise, ancak dP değeri negatif ise bu durumda mevcut güç değeri bir önceki güç değerinden düşük olduğu anlamına gelir ve Vref değeri azaltılır. Eğer dP değeri sıfırdan farklı ise, ancak dP değeri pozitif ise bu durumda mevcut güç değeri bir önceki güç değerinden yüksek olduğu anlamına gelir ve Vref değeri arttırılır. Eğer dP değeri sıfır ise, bu durumda mevcut güç değeri bir önceki güç değeri ile aynı olduğu anlamına gelir ve Vref değeri değiştirilmez.
Son olarak, Vref değeri, Vrefmax ve Vrefmin değerlerinin aralığının dışına çıkmamak üzere kontrol edilir ve eğer Vref değeri aralık dışına çıkmış ise, Vref değeri Vrefold değerine eşitlenir. Bu işlemler sonrasında, Vref değeri Vrefold değeri olarak saklanır ve Vold, Pold ve Vrefold değerleri güncellenir.



### Başlangıç Parametreleri Seçimi
MPPT algoritmasında kullanılan Vrefinit, Vrefmax ve deltaVref değerleri, sistem üzerinde çalıştırılacak olan MPPT algoritmasının performansını etkileyebilecek önemli parametrelerdir. Bu nedenle, bu değerlerin seçimi önemlidir ve sistem özelliklerine göre doğru bir şekilde belirlenmelidir.
Vrefinit, MPPT algoritması çalıştırılmadan önce, PV panelinin gerilim değerinin başlangıç değeridir. Vrefmax ise MPPT algoritması sırasında PV panelinin gerilim değerinin maksimum değeridir. DeltaVref ise MPPT algoritması sırasında, PV panelinin gerilim değerinde yapılacak olan değişim miktarıdır. Örneğin, eğer sistem üzerinde çok hızlı bir değişim yapılması gerekiyorsa, deltaVref değeri büyük seçilebilir. Ancak, eğer sistem üzerinde yavaş bir değişim yapılması gerekiyorsa, deltaVref değeri küçük seçilebilir.

Vrefinit değeri; sistemin maksimum güç değerinin biraz altında olmalıdır. 
Vrefinit -> 68.4 V - (68.4 V* %10) =  61.56 V

Vrefmax değeri; sistemin maksimum güç değerinin biraz üstünde olmalıdır. 
Vrefmax -> 68.4 V + (68.4 V* %10) = 75.24 V

Vrefmin değeri; Panelin güç üretebildiği gerilimdir ve genellikle "açık devre gerilimi" olarak belirtilir.
Bu şekilde, MPPT algoritmasında kullanılan Vrefinit, Vrefmax ve deltaVref değerleri, sistem özelliklerine göre doğru bir şekilde belirlenir ve MPPT algoritması üzerinde testler yapılırken bu değerler kullanılır.



## PI Kontrolör
Kp (Oransal Kazanç) ve Ki (İntegral Kazanç) değerleri doğru seçilerek istenilen yanıt elde edilebilir.

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/73be6e01-93d2-4461-a507-31814d6ddde5)



## Grafikler
### Sabit Irradiation (1000 W/m^2) Grafikler

**Sabit Irradiation PV (MSX 83) Akım Voltaj Grafikleri**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/f05bc850-bfc2-4daa-a4dd-ff31f0bac831)


**Sabit Irradiation PV (MSX 83) Güç Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/6b9b3091-ba6e-4558-b97c-ec33efbe51b0)


**Sabit Irradiation IGBT Akım Voltaj Grafikleri**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/27d589ab-a102-44f0-9afe-b9e680c95991)


**Sabit Irradiation Diyot Akım Voltaj Grafikleri**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/b8a0c108-7d92-4b63-8d60-7e0b8fa3fb31)


**Sabit Irradiation Load (Yük) Akım Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/4688b656-65ad-4da8-8c68-f149321aa1cb)


**Sabit Irradiation Load (Yük) Voltaj Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/63b5bdab-3ad5-4bbd-94e5-b9f982c34d14)


**Sabit Irradiation Load (Yük) Güç Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/724c79a3-513c-4992-97df-fa49ae23bbc5)

### Değişken Irradiation Grafikler
**Değişken Irradiation PV (MSX 83) Akım Voltaj Grafikleri**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/3c43016b-e5f8-4d67-bb2e-e3c0961d604f)


**Değişken Irradiation PV (MSX 83) Güç Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/eb658ffc-eaea-4b1d-8b0f-3b11b9050e56)


**Değişken Irradiation IGBT Akım Voltaj Grafikleri**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/cccf4872-5f5e-432b-979f-5b0d6416d756)


**Değişken Irradiation Diyot Akım Voltaj Grafikleri**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/a273d762-faf2-4c36-8baa-8e12a204f1f3)


**Değişken Irradiation Load (Yük) Akım Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/f0c61704-5305-41ed-9fb1-1f46e96096ff)


**Değişken Irradiation Load (Yük) Voltaj Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/60849edc-2324-4cff-a388-9f1ba074be4d)


**Değişken Irradiation Load (Yük) Güç Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/6774f816-1ca7-4b4a-aa16-597abdada801)



## Sonuçlar
**PV Array Karakteristik İncelemesi**
Dört seri bağlı PV panel kullanılıyor ve her panelin maksimum güç noktası 82.935 W olduğundan, sistemin toplam güç çıkışı 4 x 82.935 W = 331.7 W olacaktır. Bu durum başlangıçta modellenmiş olunan 4 seri modülden oluşan PV Array karakteristik eğrilerine bakıldığında  maximum power noktasının Şekil1’de data cursor ile işaretlenmiş olan 331.7 W olduğu gözlemlendi.

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/2e0376af-daae-4745-a1e2-0cc398dd8dbf)


**Değişken Irradiation Sonuçları**
PV panelinin güneş ışınlarının yoğunluğunun (W/m^2) değişmesi sonucu, sistemin verimliliğinde değişiklikler meydana gelebilir. Bu değişiklikler, sistem tasarımı ve kullanılan yükler gibi faktörlere göre farklılık gösterebilir. Düşük yoğunlukta güneş ışınlarının olması durumunda, PV panelinin gerilim ve akım değerleri düşecektir ve sistem toplam gücü de azalacaktır. Bunun tersi de geçerlidir, yüksek yoğunlukta güneş ışınlarının olması durumunda, PV panelinin gerilim ve akım değerleri artacak ve sistem toplam gücü de artacaktır. Bu değişikliklerin etkisi Değişken Irradiation Grafikler kısmında gösterildi, PV panelinin gerilim ve akım değerleri zaman içinde izlendi, yüklerin gerilim ve akım değerlerine göre sistemin toplam gücünün nasıl değiştiği gözlemlendi. 

**Değişken Irradiation PV panel ve Yük Güç Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/528e6b60-554b-4fbb-a2e7-d76851a981a6)


**Sabit Irradiation Sonuçları**
PV sistem çıkışından ölçülen maximum güç değeri 331.658 W ve yük (load) çıkışından ölçülen maximum güç değeri 331.658 W (Şekil2).  PV Array karakteristik eğrisinde bulunan maximum power noktası da 331.7 W olarak gözlemlenmiştir (Şekil1). Bu veriler, tasarım hedeflerine uygun bir şekilde bir PV sistemi tasarlandığını göstermektedir. Burada panel güç değeri ile yük güç değerinin birbiri ile neredeyse aynı çıktığı gözlemlendi, bu demektir ki sistem doğru bir şekilde çalışmaktadır ve hedeflenen güç değerine ulaşılmıştır. Bu durumun sürekliliğinin kontrol edilmesi gerekmektedir, ancak genel olarak sistem performansının iyi olduğu söylenebilir. Model ayrık zamanlı olarak simüle edilerek örnekleme zamanı (5e-07) olabildiğince küçük seçilerek simülasyon hataları azaltıldı ve sistem tasarımı sırasında dalgalanmaların (gürültülerin) minimize edilmesine özen gösterildi.

**Sabit Irradiation PV panel ve Yük Güç Grafiği**

![pi](https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB/assets/80363244/745bcea8-8036-4092-94c2-acba18e4ac7f)



## Referanslar
⦁	https://www.youtube.com/watch?v=vOjy3OwmL4w 

⦁	https://www.youtube.com/watch?v=6TTx68g0wIQ&t=497s 

⦁	https://www.youtube.com/watch?v=l7YogFyhZUk&t=187s 

⦁	https://www.mathworks.com/solutions/power-electronics-control/mppt-algorithm.html 

⦁	Proportional plus Integral (PI) Control for Maximum Power Point Tracking in Photovoltaic Systems, Arun Raj ve Anu Gopinath

⦁	Machine Learning and Deep Learning for Photovoltaic Applications



## Nasıl Başlamalı?

1. Bu repo'yu bilgisayarınıza klonlayın.
   ```bash
   git clone https://github.com/dagaca/Photovoltaic-Application-with-MPPT-in-MATLAB.git
