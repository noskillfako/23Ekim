## Sözleşmeler, SOLID prensipleri &Oop mimarisi
#### 1.1 'interface'(arayüz/sözleşme)
**Sözlük Anlamı** İki bağımsız sistem veya sınıfın birbirleriyle nasıl konuşacağını belirleyen içi boş metot imzalarından oluşan kurallar bütünü
***Neden kullanılır*** bir sınıfa göbekten bağımlı kalmamak (örneğin gerçek şarj istasyonu cihazı olmadan sahte/mock şarj servisiyle testi çalıştırabilmek) için
''dart
abstract interface class StationContreact{
    Future <void> startCharging(String socketId);
    Streamdouble> watchLivePower();
}

### 1.2 İmplements vs Extends vs with
### 1.2.1 implements (Arayüzü sözleşmesi)
****Sözlük anlamı**** bir sınıfın bir şablonun belirlediği kuralları kabul edip içini sıfırdan kendisinin doldurması zorunluluğudur
**Neden Kullanılır** farklı sınıflara ortak bir standart sözleşme dayatmak çoklu rayüz desteği sağlamak ve koda olan bağımlılıkları azaltmak için
### 1.2.2 extends (kalıtım/miras)
*** sözlük anlamı ** bir sınıfın başka bir sınıfın özelliklerini ve metotlarını doğrudan devralması 
***Neden kullanılır*** kod tekrarını önlemek (DRY) mantıksal bir sınıf hiyerarşisi oluşturmak için
### 1.2.3. with(mixin/yetenek aşılama)
***Neden kullanılır** sınıf hiyerarşisini bozmadan bağımsız bir sınıfa hazır kod blokları (metotlar ve değişkenler) enjekte etme mekanizmasıdır.
***Neden kullanılır*** akraba olmayan sınıflara ortak yetenekler kazandırmak çoklu kalıtım kimliğini önlemek ve modulerliği arttırmak için 

## 1.3 SOLID Prensibleri 

### 1.3.1 Single Responsibility Principle(Tek Sorumluluk Prensibi)
**kural**: bir sınıfın veya dosyanın değişmesi için **yalnızca 1 sebebi** olmalıdır.

### 1.3.2 Open/Closed Principle(Açık/Kapalı Prensibi)
**kural**: yazılım varlıkları (sınıflar, metotlar, modüller) **genellemeye açık (open)** ama **değiştirmeye kapalı (closed)** olmalıdır.

### 1.3.3 Liskov Substitution Principle (LSP)
***kural*** bir üst sınıfın yerine yerine, onun alt sınıflarından herhangi biri geçirildiğinde sistemin davranışı bozulmamalı ve hata fırlatmamalıdır

### 4.I- Interface Segregation Principle(Arayüz Ayrıştırma Prensibi)
***kural** istemciler(sınıflar),
kullanmadıkları metotları içermeye zorlanmamalıdır. şişman(fat) interfaceler yerine küçük amaca yönelik arayüzler kurulmalıdır


### 5. Dependency Inversion Principle (DIP)
***kural*** yüksek seviyeli moduüller (UI, iş mantığı), düşük seviyeli modüllere (veritabanı, HTTP  istemcisi) doğrudan bağlı olmamalıdır her ikisi de soyutlamalra ('interface') bağlı olmalıdır.

### 1.4. 'Coupling' (Bağlaşılık) & 'Cohesion' (Uyum)
***Tightly Coupled(sıkı bağımlı):** sınıfların birbirinin her detayının bilmesi birinde yapılan bir harflil değişiklik diğer 5 dosyayı bozuyorsa sistem sıkı bağımlıdır.
***Loosey Coupled(gevşek bağlı):** sınıfların sadece interface üzerinden konuşması. biri değiştiğinde diğeri etkilenmez 
**high cohesion(yüksek uyum)** bir sınıfın sadece ve sadece kendi uzmanlık alanını yapması.

## Stream Temelleri,Cubit & Enterprıse Bloc
### 2.1. 'Stream' Nedir? (BloC'un kalbindeki Motor)
**Sözlük anlamı:** zaman içinde parça parça Asenkron veri akışını temsil eden soyut bir kavramdır.   
*** BLOC sırrı*** BLOC kütüphanesi sihirli bir şey yapmaz! BloC'un içinde gizli bir 'StreamController' vardır Event'ler borunun bir ucundan girer ('sink.add') BloC onları işler ve diğer ucundan ekrana state akıtır ('stream.listen').

## 2.3 BloC'un 3 Temel Elemanı: 'Event','State','Emitter'
***Event(olay)*** kullanıcının veya sistemin BloC'a attığı emir fişidir
**Mini sözDİZİMİ:**
```dart
class StartChargingRequested extends Charging Event {final String socketId;}
```

***State(Durum)*** ekranın o milisaniyedeki dondurulmuş fotoğrafı(snapshot)
*Mini Sözdizim* class ChargingActiveState extends ChargingState {final double kwh}
özel durumlar:'Inıtial','Loading','Loaded','Error'
***Emitter** BloC'un ürettiği yeni state'i ekrana fırlattığı megafon borusu 
*Kural:* Senkron çalışır; 'emit(NewState())'
dendiği anda UI'daki builderlar tetiklenir 
### 3.1. 'Side Effect' (Yan Etki) Neden 'BlocBuilder' içine yazılamaz?
* 'BlocBuilder' tek görevi **Widget döndürmektir** 
*Flutter motoru ekranı saniyede 60 kez çizebilir eğer siz 'BlockBuilder' içine 'ScaffoldMessenger.showSnackBar' veya 'Navigator.push' yazarsanız; kullanıcı cihazı yan çevirdiğinde veya kalvye açıldığında kullanıcıya **üst üst 10 defa SnackBar patlar veya 10 kez yeni sayfa açılır!**
*Kural: Yan etkiler sadece 'BlockListener' içine yazılır çğnkğ .2Listener' sayfa rebuikd olduğunda deği yalnızca state değiştiğinde 1 kez tetiklenir
### 3.2. 'BlocSelector'(Mikro performans & pil tasarrufu)
* Kavram: Statein içindeki onlarca alandan sadece **1 tanesini süzüp izlemek**

---
## 4. Eşilik ,Değişmezlik &BlocobServer
### 4.1.1 'Refential Equality' vs 'Value Equality
* **Dart Varsayılanı (referans eşitliğ):** İki nesnenin bellekteki Ram adresi aynı mı
'Soket('A')== Soket('A')-> false ÇÜNKÜ bellek adresi farklıdır. 
**Değer Eşitliği (Value Equality):
nesnelerin içindki veriler aynı mı
'equtable' paketi tam olarak bunu çözer nesneleri RAM adresine göre değil 'props' listesindeki alanların değerine göre karşılaştırlır

### 4.2. 'props' Listesi yazılmazsa ne olur?
BloC ekran kartını yormamak için şu denetimi yapar: 'if(yeniState==eskiState=) Ekranıçizme();'
* Eğer siz: 'Stationloaded' stateine yeni bir 'filterType' alanı eklerseni ama ' props' içine yazmayı unutursanız; BloC iki state'i eşit zanneder ve **ekranı güncellemez , arayüz donuk kalır!**

### 4.3. 'Immutablity' (Değişmezlik) & 'copyWith
**Kural:** State sınıfların tüm alanları 
'final' olmalıdır (değişmez)
** 'copywith' Mangtığı:** Eski nesnenin 5 alanını koruyup sadece değişen 1 alanını güncelleyerek taze bir nesne üretme yöntemidir

final guncel =state.copyWith(isCharging:true)

### 4.4. 'BlocObserver' (Tüm uygulamayı Röntgenleme)
**Kavram:** Uygulamadaki tüm bloc ve cubitlerin arkasında çalışan merkezi birkara kutu tleemtri kaydedici 
ne işe yarar hangi ekranda hangi vent fırlatıldı State nereden nereye geçti  ontransiyton ve nerede hata fırlatıldı
on Error tek merkezden konsola loglanır kURUMSAL PROJELERDE ANALATİKŞ ÇÖKME TAKİBİ ZORUNLUDUR

## 5.clean architecture katmanları & sorumluluk duvarları
### 5.1.1 'Entity' vs 'Model/DTO'
**Entitiy(Varlık): *** Saf iş nesnesidir bir şarj soketinin gerçek hayatttaki özelliklerini id kw durum tutar. fromjson veya toJson içermez
** model/dto Apıden gelen ham json verisinin karşılayan taşıyıcıdır entitiyden türer fromjson ve tojson metodlarını barındırırı

neden entitiynin içine fromjson yazmıyoruz?

cevap:Yarın backend json yerine XML veya Protobuf formatına geçerse Domain katmanındaki iş kuralllarımız ve uı sayfalarımız zerre kadar etkilenmesin sadece DATA katmaındaki mdeol etkilensin diye

##5.2. UseCase (Kullanım Snearyosu)
kavram: kullanıcının veya sistemin gerçekleştirebileceği tek ve atomatik bir iş kuralı 
Örnekler StartChargingSessionUseCase
GetStationSocketUseCase
RefundWalletBalanceUsecase
kural: hER UseCase sınıfının içinde sadece tek bşr call() veya execute() fonksiyonu blunur

## 5.3. Repository contract vs Repository Implementation
* **Contract (Domain Katmanında): ** Sadece sözleşmedir (abstract interface class StationRepository). Verinin nereden geleceğini bilmez.
* **Implementation (Data Katmanında): ** Sözleşmeyi uygulayan somut sınıftır (class StationRepositoryImpl implements StationRepository). Verinin önce yerel diskten mi yoksa internetten mi çekileceğine karar verir.
##Bağımlılık Yönetimi Dependency ınjection & 'get_it'
## 6.1. neden newdio() yazmak yasaktır
Eğer 5 ayrı sayfada final dio = Dio(); derseniz:

Bellekte 5 ayrı bağlantı havuzu açılır, telefonun soket sınırını zorlar.
Base URL veya Timeout değiştiğinde 5 ayrı dosyayı elle değiştirmek zorunda kalırsınız.
Test yazarken sahte (mock) veri enjekte edemezsiniz
## 6.2. Getit Kayıt Tipleri Singleton vs LazySingleton vs Factory
Singleton: Uygulama açıldığı milisaniyede nesneyi üretir ve belleğe çiviler.
LazySingleton: Uygulama açıldığında üretmez! Ne zaman ilk kez birisi sl<Dio>() derse o an üretir ve sonrasında hep aynı nesneyi verir. (Bellek ve açılış hızı dostudur).
Factory: Her çağrıldığında yepyeni, sıfır bir kopya üretir.

## 7.Kurumsal Ağ Mimarisi Dıo Interceptor & Http Status kodları
Sözlük Anlamı Ağ isteği telefondan çıkıp internete gitmeden önc bveya sunucudan cevap gelip koda düşmeden önce araya girip paketi inceleyen değiştiren güvenlik noktası

## 7.2 Bearer Token & Payload 
Bearer Token: "Bu anahtarı taşıyan kişi giriş yapmaya yetkilidir" anlamına gelen şifrelenmiş kimlik kartı metni (Authorization: Bearer eyJhbGci...).
Payload: Bir API isteğiyle sunucuya gönderilen veya sunucudan bize dönen asıl veri gövdesi (Body).
7.3 HTTP Durum kodları sözlüğü
200 OK: İstek başarılı, veri gövdede.
201 Created: Yeni kayıt (örneğin yeni şarj oturumu) başarıyla oluşturuldu.
400 Bad Request: İstemci hatası; eksik veya hatalı parametre gönderdin.
401 Unauthorized: Token yok veya süresi dolmuş; hemen Login'e yönlendir!
403 Forbidden: Giriş yaptın ama bu veriyi görmeye yetkin yok (Admin değilsin).
404 Not Found: Aradığın soket veya istasyon sunucuda bulunamadı.
500 Internal Server Error: Backend sunucusu patladı; istemcilik bir hata yok.

8. veri Dayanıklılığı cache-FİRST & savunmacı Kodlama

8.1. Cache-First (Önce Önbellek) Stratejisi
Mantık: Kullanıcı şarj istasyonu listesini açtığında 1 saniye bile beyaz ekranda spinner bekletme! Diskteki dünkü veriyi 0.01 milisaniyede ekrana bas. Arka planda sunucudan taze veriyi çek; gelince ekranı ve diski sessizce güncelle

## 8.2 Defensive Promgraming 
Mantık: Sunucudan gelen veriye asla %100 güvenme prensibi
