# Dart Convert Turkish

## Dart’a Giriş
Dart programlarına ve önemli kavramlara kısa bir giriş.

Bu sayfa, Dart dilinin temel özelliklerini örnekler üzerinden kısaca tanıtmaktadır.

Dart dili hakkında daha fazla bilgi için sol menüdeki **Language** altında listelenen detaylı konu sayfalarını ziyaret edebilirsiniz.

Dart’ın temel kütüphaneleriyle ilgili bilgi için temel kütüphane dokümantasyonuna göz atabilirsiniz. Daha interaktif bir giriş için Dart kısaca (cheatsheet) sayfasını da inceleyebilirsiniz.

---

## Hello World
Her uygulama, yürütmenin başladığı en üst düzey `main()` fonksiyonuna ihtiyaç duyar. Açıkça değer döndürmeyen fonksiyonların dönüş tipi `void`’dir. Konsola metin yazdırmak için üst düzey `print()` fonksiyonunu kullanabilirsiniz:

```dart
void main() {
  print('Hello, World!');
}
```

Dart’ta main() fonksiyonu hakkında daha fazla bilgi ve komut satırı argümanları için opsiyonel parametreler için ilgili dökümantasyonu okuyun.

Değişkenler

Type-safe (tip güvenli) Dart kodunda bile çoğu değişkeni var ile açıkça tip belirtmeden tanımlayabilirsiniz. Tip çıkarımı sayesinde bu değişkenlerin tipleri başlangıç değerlerine göre belirlenir:

```var name = 'Voyager I';
var year = 1977;
var antennaDiameter = 3.7;
var flybyObjects = ['Jupiter', 'Saturn', 'Uranus', 'Neptune'];
var image = {
  'tags': ['saturn'],
  'url': '//path/to/saturn.jpg',
};
```

Dart’ta değişkenler hakkında daha fazla bilgi, varsayılan değerler, final ve const anahtar kelimeleri ve statik tipler için dökümantasyona bakabilirsiniz.

Kontrol Akışı İfadeleri

Dart, yaygın kontrol akışı ifadelerini destekler:

```if (year >= 2001) {
  print('21. yüzyıl');
} else if (year >= 1901) {
  print('20. yüzyıl');
}

for (final object in flybyObjects) {
  print(object);
}

for (int month = 1; month <= 12; month++) {
  print(month);
}

while (year < 2016) {
  year += 1;
}
```
Fonksiyonlar

Her fonksiyonun argüman ve dönüş değerlerinin tiplerini belirtmek önerilir:

```int fibonacci(int n) {
  if (n == 0 || n == 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

var result = fibonacci(20);
```

Tek bir ifade içeren fonksiyonlar için => (ok) kısayolu kullanışlıdır. Özellikle anonim fonksiyonları argüman olarak geçerken işe yarar:

flybyObjects.where((name) => name.contains('turn')).forEach(print);
Yorumlar

```Dart’ta yorumlar genellikle // ile başlar:

// Bu normal, tek satırlık bir yorumdur.

/// Bu bir dokümantasyon yorumudur ve kütüphaneleri,
/// sınıfları ve üyelerini belgelemek için kullanılır.

/* Bu şekilde çok satırlı yorumlar da desteklenir. */
Importlar

Diğer kütüphanelerde tanımlı API’lara erişmek için import kullanılır:

// Temel kütüphaneleri import etme
import 'dart:math';

// Harici paketlerden kütüphane import etme
import 'package:test/test.dart';

// Dosya import etme
import 'path/to/my_other_file.dart';
```
# Sınıflar

Örnek bir sınıf:

```class Spacecraft {
  String name;
  DateTime? launchDate;

  int? get launchYear => launchDate?.year;

  Spacecraft(this.name, this.launchDate);

  Spacecraft.unlaunched(String name) : this(name, null);

  void describe() {
    print('Spacecraft: $name');
    var launchDate = this.launchDate;
    if (launchDate != null) {
      int years = DateTime.now().difference(launchDate).inDays ~/ 365;
      print('Launched: $launchYear ($years yıl önce)');
    } else {
      print('Unlaunched');
    }
  }
}
```

## Kullanım örneği:

```var voyager = Spacecraft('Voyager I', DateTime(1977, 9, 5));
voyager.describe();

var voyager3 = Spacecraft.unlaunched('Voyager III');
voyager3.describe();
Enumlar
enum PlanetType { terrestrial, gas, ice }

enum Planet {
  mercury(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  venus(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  uranus(planetType: PlanetType.ice, moons: 27, hasRings: true),
  neptune(planetType: PlanetType.ice, moons: 14, hasRings: true);

  const Planet({
    required this.planetType,
    required this.moons,
    required this.hasRings,
  });

  final PlanetType planetType;
  final int moons;
  final bool hasRings;

  bool get isGiant =>
      planetType == PlanetType.gas || planetType == PlanetType.ice;
}
Kalıtım
class Orbiter extends Spacecraft {
  double altitude;

  Orbiter(super.name, DateTime super.launchDate, this.altitude);
}
Mixins
mixin Piloted {
  int astronauts = 1;

  void describeCrew() {
    print('Number of astronauts: $astronauts');
  }
}

class PilotedCraft extends Spacecraft with Piloted {}
Async
const oneSecond = Duration(seconds: 1);

Future<void> printWithDelay(String message) async {
  await Future.delayed(oneSecond);
  print(message);
}

Stream<String> report(Spacecraft craft, Iterable<String> objects) async* {
  for (final object in objects) {
    await Future.delayed(oneSecond);
    yield '${craft.name} flies by $object';
  }
}
Exceptionlar
if (astronauts == 0) {
  throw StateError('No astronauts.');
}

try {
  // Kod
} on IOException catch (e) {
  print('Hata: $e');
}
```
# Önemli Kavramlar

- Değişkenlere atayabileceğiniz her şey bir nesnedir ve her nesne bir sınıfın örneğidir. Sayılar, fonksiyonlar ve null bile nesnedir. Null hariç (null safety etkinse) tüm nesneler Object sınıfından türetilir.
- Null güvenliği Dart 2.12 ile geldi.
- Dart güçlü tiplidir, ancak tip belirtimi opsiyoneldir.
- Değişkenler null değer alamaz, eğer almasını istiyorsanız tipin sonuna ? ekleyin: int? x.
- Object?, Object veya dynamic ile her tipte değer kabul edebilirsiniz.
- Dart, generic tipleri destekler: List<int> veya List<Object>
- Dart hem üst düzey fonksiyonları hem de sınıf/nesne fonksiyonlarını destekler.
- Dart’ta public, protected, private yoktur; _ ile başlayan isimler kütüphane özelidir.
- Identifiers harf veya _ ile başlar, rakam veya diğer karakterler ile devam edebilir.
Dart ifadeler (runtime değer döndürür) ve ifadeler içermeyen statement’ları destekler.

Dart araçları uyarı (warning) ve hata (error) raporlayabilir.

