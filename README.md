# clean-code-javascript

## Mündəricat

1. [Giriş](#Giriş)
2. [Dəyişkənlər](#Dəyişkənlər)
3. [Funksiylar](#Funksiyalar)
4. [Obyektlər və Məlumat Strukturları](#obyeklər-və-məlumat-strukturları)
5. [Siniflər (Classes)](#siniflər)
6. [SOLID](#solid)
7. [Testlər](#testlər)
8. [Paralellik](#paralellik)
9. [Xətanın idarə edilməsi](#xətanın-idarə-edilməsi)
10. [Formatlama](#formatlama)
11. [Şərhlər](#şərhlər)
12. [Tərcümə](#tərcümə)

## Giriş
	
![Proqram keyfiyyətinin qiymətləndirilməsinin yumoristik təsviri.](https://www.osnews.com/images/comics/wtfm.jpg)

Bu məqalə Robert C. Martinin proqram mühəndisliyi prinsiplərini ehtiva edən [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) kitabının JavaScript versiyasıdır. Bu üslub bələdçisi olmayıb, sadəcə JavaScript-də [Oxumaq, təkrar istifadə etmək və yenidən redaktə etmək](https://github.com/ryanmcdermott/3rs-of-software-architecture) kimi prinsipləri öyrədib və bu tiplərdə proqram təminatı yaratmaq üçün olan bir bələdçidir.

Sizdən buradakı hər bir prinsipə ciddi şəkildə əməl etmək tələb olunmur, burada olan məsləhətlər yanlnız kodunuz daha oxunaqlı və səliqəli etmək üçün _Clean Code_ müəllifləri tərəfindən uzun illik təcrübələrə əsaslanan faydalı məsləhətlərdir.

Proqram mühəndisliyi peşəmiz 50 yaşının bir az üstündədir və biz hələdə çox şey öyrənirik. Proqram təminatının arxitekturası arxitekturanın özü qədər köhnə olanda, bəlkədə əməl etməli olduğumuz daha çətin qaydalarımız olacaq. İndi isə icazə verin, bu tövsiyələr sizin və komandanızın hazırladığı JavaScript kodunun keyfiyyətini qiymətləndirmək üçün bir məhək daşı kimi sizə kömək etsin.



QEYD: Bu məsləhətləri bilmək sizi dərhal daha yaxşı bir proqram mühəndisi etmir, və bu məsləhətləri tətbiq etdiyinizdə yazdığınız kod səhvsiz olacaq mənasına gəlmir.

Hər bir kod parçası ilk olaraq qaralama kimi başlayır, məsələn: yaş gil quruyub son formasını alıb saxsı olur. Eyni ilə proqram təminatındada yazılan kodları həmkarlarımızla birlikdə nəzərdən keçirərkən qüsurları düzəldirik. Təkmilləşdirilməyə ehtiyacı olan koda ilişib qalmayın. Bunun əvəzinə kodunuza daha yaxşı yazmağa fokuslanın.

## **Dəyişkənlər**

### Mənalı və fərqli dəyişən adlarından istifadə edin

**Pis:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Yaxşı:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Eyni dəyişən növü üçün eyni sözləri istifadə edin

**Pis:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Yaxşı:**

```javascript
getUser();
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Axtarıla bilən adlardan istifadə edin

Biz kod yazmağdan daha çox kod oxuyacağıq, ona görədə kodun oxuna bilən və axtarıla bilən olması vacibdir. Dəyişənləri mənalı və başa düşülən ***adlandırmamaqla*** biz oxucuya zərər vermiş oluruq. Adlarınızı axtara bilən edin. buddy.js və ESLint kimi alətlər adsız sabitləri müəyyən etməyə kömək edəcək.


**Pis:**

```javascript
//  86400000 nə idi?
setTimeout(blastOff, 86400000);
```

**Yaxşı:**

```javascript
// Bu tip dəyişənləri böyük hərflə yazılmış sabitlər(consts) kimi təyin edin.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### İzah oluna bilən dəyişənlərdən istifadə edin.

**Pis:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Yaxşı:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Başdan getdi planlamağdan çəkinin.

Açığ olmaq, gizli olmaqdan qat qat yaxşıdır.

**Pis:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Bir dəqiqə, `l` nə idi ki?
  dispatch(l);
});
```

**Yaxşı:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Kodunuzda lazımsız əlavə məlumat yazmayın.

Sinif / obyekt adınız sizə bir şey deyirsə, onu dəyişən adınızda təkrarlamayın.

**Pis:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**Yaxşı:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Qısaqapanma və ya şərtlər yerinə əvvəlcədən təyin edilmiş arqumentlərdən istifadə edin

Əvvəlcədən təyin edilmiş arqumentlər ümumiyyətlə qısa dövrələrdən daha təmizdir, bunları istifadə etsəniz bir şeyə diqqət yetirin, funksiyanız sadəcə `qeyri-müəyyən` *(undefined)* dəyərlər üçün öncədən təyin edilmiş arqumentı istifadə edəcək, `'',` `"",` `false,` `null, `0` və `NaN` kimi "Yanlış" adlandırıla bilən dəyərlər əvvəlcədən təyin edilmiş dəyərlə əvəz edilmir.

**Pis:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Yaxşı:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

## **Funksiyalar**

### Funksiya arqumentləri (məsləhət olunan arqument sayısı 2 və ya daha az)

Funksiya parametrlərinin sayını məhdudlaşdırmaq olduqca vacibdir, çünki bu, funksiyanızı sınamağı asanlaşdırır. Üçdən artıq arqumentin olması hər bir arqumentlə tonlarla fərqli işi sınaqdan keçirməlisənə yol açır buda nəticə böyük bir kombinasiyalı partlayışa gətirib çıxarır. 


Bir və ya iki arqument ideal haldır və mümkünsə üçdən qaçınmaq lazımdır. Bundan daha çox birləşdirilməlidir. Adətən, ikidən çox arqumentdən sonra funksiyanız çoxlu əməliyyatlar etməyə çalışır. Olmadığı hallarda, çox vaxt daha yüksək səviyyəli bir obyekt arqument kimi kifayət edəcəkdir.


JavaScript sizə havada obyektlər yaratmağa imkan verdiyindən, birdən çox obyektdən istifadə etmədən, çoxsaylı sinif konstruksiyalarına ehtiyac olmadan obyektdən istifadə edə bilərsiniz.

Funksiyanın hansı xassələri gözlədiyini aydınlaşdırmaq üçün ES2015 / ES6 təhlil sintaksisindən istifadə edə bilərsiniz. Bunun bir sıra üstünlükləri var:

1. Funksiyanın imzasına baxdıqda, hansı xüsusiyyətlərin istifadə edildiyini dərhal bilir.
2. Adlandırılmış parametrləri simulyasiya etmək üçün istifadə edilə bilər.
3. Təhlil prosesi həmçinin funksiyaya ötürülən arqument obyektinin ilkin müəyyən edilmiş dəyərlərini klonlaşdırır. Bu, yan təsirlərin qarşısını almağa kömək edə bilər. Qeyd: Arqument obyektlərindən təhlil edilən obyektlər və massivlər klonlaşdırılmır.
4. Linters istifadə edilməmiş dəyərlər üçün sizi xəbərdar edə bilər ki, bunuda təhlil etmədən təyin etmək mümkün olmayacaqdır.

**Pis:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**Yaxşı:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Funksiyalar bir şeyi etməlidir

Bu anlayış proqram mühəndisliyində ən vacib anlayışdır. Funksiyalar birdən çox şeyi yerinə yetirdikdə, onları Birləşdirmək, Sınamaq və Mənalandırmağ daha çətindir. Əgər Biz bir funksiyanı yazlnız vahid bir hər iş üçün yaratsaq, onu(vahid iş üçün yaradılmış funksiyanı) asanlıqla refaktorlaşdırıla bilər və, kiçik dəyişiklikə böyük bir funksiyanın yerinə yetirdiyi işi dəyişə bilər və kodumuz daha təmiz olmuş olar. Bu məqalədən heçnə anlamasanız belə, tək bu anlayışı anlamaqla bir çox *proqramçıdan* öndə olacaqsınız.

**Pis:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Yaxşı:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Funksiya adları nə etdiklərini bildirməlidir

**Pis:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// Funksiya adından nəyin əlavə olunduğunu söyləmək çətindir
addToDate(date, 1);
```

**Yaxşı:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Funksiyalar yalnız bir mücərrəd(abstraksiya) səviyyəsidə olmalıdır

Birdən çox mücərrədləndirilməsi(abstraksiyası) səviyyəniz olduqda funksiyalarınız daha çox şey yerinə yetirir. Fonksiyonların bölünməsi, yenidən istifadə edilməsi və daha asan test edilməsini imkan verir.

**Pis:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**Yaxşı:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Təkrarlanan kodların istifadəsindən maksimum uzağ durmağa çalışın


Təkrarlanan kodların qarşısını almaq üçün əlinizdən gələni edin. Təkrarlanan kodlar pisdir, çünki təkrarlanan kodlar o deməkdir ki, siz bir funksiyadakı kodu dəyişmək istədiyinizdə bunu gedib bir neçə yerdə etməyə məcbur olursunuz.

Təsəvvür edin ki, bir restoran işlədirsiniz və bu, restoranın ehtiyyatında olan meyvələri izləyirsiniz: almalar, bananlar, armudlar, və s, və onların qiymətin dəyişmək istəyirsiniz, və bunu yanlız bir dəfə etməklə reallaşdırmaq istəyirsiniz!

Siz tez-tez təkrarlanan kodunuz olur, çünki onlar ümumi şeylərin çoxunu paylaşsalar da, çox az fərq var, lakin onların fərqləri sizi eyni şeylərin əksəriyyətini yerinə yetirən iki və ya daha çox ayrı funksiyaya malik olmağa məcbur edir. Təkrarlanan kodun silinməsi bu müxtəlif şeyləri tək bir funksiya/modul/sinif ilə idarə edə biləcək bir mücərrədləndirmə(abstraksiyalandirmaq) yaratmaq deməkdir.

Mücərrədləndirməni(abstraksiyanı) düzgün əldə etmək çox vacibdir, ona görə də siz Siniflər bölməsində qeyd olunan *SOLID* prinsiplərinə əməl etməlisiniz. Pis mücərrədlər(abstraksiyalar) təkrarlanan koddan daha **pis** ola bilər, ona görə də **diqqətli olun!** Bunu demişkən, yaxşı bir abstraksiya etməyi bacarırsınızsa, abstraksıyanızı edin! Kodunuzu təklarlamayın, əks halda özünüzü minlərlə kodun içində sətir bə sətir əl ilə kodlarınızı düzəldənədə tapa bilərsiniz.

**Pis:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Yaxşı:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Object.assign ilə varsayılan obyektləri qurmaq

**Pis:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Yaxşı:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // konfiqurasiya indi bərabərdi: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Funksiya parametrlərində işarələmədən istifadə

İşarələr göstərir ki, funksiyanız birdən çox iş gördüyü başa düşülsün. Funksiyalar yalnız bir şeyi etməlidir. Əgər aşağıdakı kimi dəyişiklikləri və məntiqi operatorları olan funksiyalarınız varsa, funksiyalarınızı ayırın.

**Pis:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Yaxşı:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Yan təsirlərdən qaçının (1-ci hissə)

Funksiya bir dəyər götürüb başqa bir dəyər və ya dəyər qaytarmaqdan başqa heç nə etmirsə, bu yan təsirdir. Yan təsir fayla yazmaq olar bilər, məsələn bəzi qlobal dəyişəni dəyişdirmək və ya təsadüfən bütün pulunuzu bir başqasına göndərmək ola bilər.

bəzən bir proqramda yan təsirlərə ehtiyacınız vardır. Əvvəlki kimi məsələn: fayla yazmağınıza lazım ola bilər. Əgər etmək istədiyiniz iş vahid bir işdirsə onu konkretləşdirib mərkəzləşdirin. Bir neçə funksiyaya və ya sinifə malik olmayan müəyyən bir fayla yazan. Bunu edən bir xidmətə sahib olun. **Yanlnız Bir və tək xidmətə sahib olsun!**.


Burada əsas məqam heç bir strukturu olmayan obyektlər arasında vəziyyəti(*state*) paylaşmaq, hər hansı bir şeylə yazıla bilən dəyişkən məlumat növlərindən istifadə etmək və yan təsirlərinizin baş verdiyi yerləri mərkəzləşdirməmək kimi ümumi tələlərdən qaçmaqdır. Əgər bunu bacarsanız, digər proqramçıların böyük əksəriyyətindən daha xoşbəxt olacaqsınız.

**Pis:**

```javascript
// Aşağıdakı funksiya Qlobal dəyişənə istinad edir
// Bu adda başqa bir funksiyamız olsaydı, indi massiv olub bu funksiyani pozardı.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Yaxşı:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Yan təsirlərdən qaçının (2-ci hissə)

JavaScript-də bəzi dəyərlər dəyişməz (dəyişməz), bəziləri isə dəyişkəndir
(dəyişkən). Obyektlər və massivlər iki növ dəyişkən dəyərdir, ona görə də vacibdir ki,
funksiyaya parametr kimi ötürüldükdə onları diqqətlə idarə olunmalıdır. A
JavaScript funksiyası obyektin xassələrini və ya məzmununu dəyişə bilər və
asanlıqla başqa yerdə səhvlərə səbəb ola bilər.

Tutaq ki, A-nı təmsil edən massiv parametrini qəbul edən bir funksiya var adı isə
Alış-veriş kartıdır. Funksiya həmin alış-veriş səbəti massivində dəyişiklik edərsə -
satın almaq üçün bir maddə əlavə etməklə, məsələn - sonra hər hansı digər funksiya
eyni 'səbət' adlı massivindən istifadə edən bu əlavədən təsirlənəcək. Bu əla ola bilər
, lakin pis də ola bilər. Gəlin pis bir vəziyyəti təsəvvür edək:

İstifadəçi “al” funksiyasını çağıran “Satınalma” düyməsini klikləyir
şəbəkə sorğusu yaradır və `səbət` massivini serverə göndərir. Çünki
pis şəbəkə bağlantısı olduqda, 'satın alma' funksiyası yenidən cəhd etməyə davam etməlidir.
İndi istifadəçi təsadüfən "Səbətə əlavə et" düyməsini klikləsə nə etməli?
Şəbəkə sorğusu başlamazdan əvvəl əslində istəmədikləri əşyaları alsın?
Əgər bu baş verərsə və şəbəkə sorğusu başlasa, o zaman satın alma funksiyası
`səbət` massivi dəyişdirildiyi üçün təsadüfən əlavə edilmiş əşiyanı göndərəcək.

`addItemToCart` funksiyasının həmişə klonlanması üçün əla həll olardı
`səbət' funksiyası. redaktə edin və klonu qaytarın. Bu hələ ki, funksiyaları təmin edəcək
köhnə alış-veriş səbətindən istifadə dəyişikliklərdən təsirlənməyəcək.

Bu yanaşma üçün iki xəbərdarlıq:

1. Daxil edilmiş obyekti həqiqətən dəyişmək istədiyiniz hallar ola bilər, 
   lakin bunu etdiyiniz zaman bu halların olduqca nadir olduğunu görəcəksiniz.
   Çox şey heç bir yan təsir olmadan yenidən təşkil oluna bilər.

2. Böyük obyektlərin klonlanması performans baxımından heçdə uyğun olmaya ola bilər,
   amma xoşbəxtlikdən, praktikada bu o qədər də böyük bir məsələ deyil, çünki orada bu tip       
   proqramlaşdırma yanaşmasını əl ilə etməkdən daha sürətli edən və böyük obyektlərin və massivlərin klonlanması zamanı daha az yaddaş istifadə edən [möhtəşəm kitabxanalar](https://facebook.github.io/immutable-js/) var.
 

**Pis:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Yaxşı:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Qlobal funksiyalara yazmayın

Qlobalları çirkləndirmək JavaScript-də pisdir, çünki siz başqa kitabxana ilə toqquşa və sizin API-lərdən istifadə edənlər problem düzələnə qədər, çətinlik çəkə və bunun nədən qaynağlandığını tapamağda çətinlik çəkərlər.

Problemi nəzərdən keçirək: tutaq ki, siz JavaScript-in doğma kitabxanasında massiv `diff` metodunu genişləndirmək və iki massivin fərqlərini göstərmək istəyirsiniz. JavaScript-in yerli Array metodunu iki massiv arasındakı fərqi göstərə bilən "diff" metoduna genişləndirmək istəyirsiniz?

Siz yeni funksiyanızı `Array.prototype`-də yaza bilərsiniz, lakin o, eyni şeyi etməyə çalışan başqa bir kitabxana ilədə toqquşa bilər. Əgər digər kitabxana massivin birinci və sonuncu elementləri arasındakı fərqi tapmaq üçün sadəcə olaraq `diff`-dən istifadə edərsə onda necə? Beləliklə, sadəcə ES2015 / ES6 siniflərindən istifadə etmək və `Array` qlobal olaraq genişləndirmək daha yaxşı olardı.

**Pis:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Yaxşı:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### İmperativ proqramlaşdırmadan daha çox funksional proqramlaşdırmaya üstünlük verin

JavaScript, Haskell kimi funksional bir dil deyil, lakin funksional bir tərzə malikdir. Funksional dillər daha təmiz və sınaqdan keçirilməsi daha asan ola bilər. Bacardığınız qədər bu tip proqramlaşdırma ilə məşğul olun.

**Pis:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Yaxşı:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Şərtlərin əhatə olunması

**Pis:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Yaxşı:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Mənfi şərtlərdən çəkinin

**Pis:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Yaxşı:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Şərtlərdən(if,else,else if) çəkinin

Bu, qeyri-mümkün bir iş kimi görünə bilər, Hətta bunu ilk dəfə eşitdikdən sonra bir çox insan "`if` ifadəsi olmadan necə kod yazım ?" Cavab budur ki, bir çox hallarda eyni tapşırığı yerinə yetirmək üçün polimorfizmdən istifadə edə bilərsiniz. İkinci sual, adətən, "yaxşı, bu əladır, amma niyə bunu etmək istərdim?" Cavab əvvəllər öyrəndiyimiz təmiz kod anlayışı idi: funksiya yalnız bir işi görməlidir. Əgər if ifadələri ilə siniflər funksiyada olduqda, bu, istifadəçilərə funksiyanızın birdən çox iş gördüyünü bildirir. Təmiz kod yazmağın əsas anlayışlarından biri olan mücərrədləndirmə(abstraksiya), funksiyada yalnız bir şey et.

**Pis:**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Yaxşı:**

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Növ yoxlamasından çəkinin (1-ci hissə)

JavaScript tipsizdir, yəni funksiyalarınız istənilən növ arqument qəbul edə bilər.
Bəzən bu bizə problem yaradıb tələyə apara bilər,
Beləki, funksiyalarınızda yoxlama yazmaq bir çox zaman cəld edici olur. Bunu etmək məcburiyyətində qalmamağın bir çox yolu var.
Nəzərə alınacaq ilk şey ardıcıl API-lərdir.

**Pis:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Yaxşı:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Növ yoxlamasından çəkinin (2-ci hissə)

Əgər sətirlər və tam ədədlər kimi əsas primitiv dəyərlərlə işləyirsinizsə,
və polimorfizmdən istifadə edə bilmirsinizsə, lakin hələ də kodun yoxlanılması ehtiyacını hiss edirsiniz,
TypeScript istifadə etməyi düşünməlisiniz. TypeScript əla alternativdir
JavaScript-də, çünki standart dinamik JavaScript-in üstünə statik yazmağı təmin edir
TypeScript. Normal JavaScript-i əl ilə(manual) debuq etmək qəliz məsələdir. 
JavaScript-inizi təmiz saxlayın,
yaxşı testlər və yaxşı kod rəyləri yazın,TypeScript (bu, dediyim kimi javascript-də, əla alternativdir təmiz kod yazmağ üçün!).

**Pis:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**Yaxşı:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Kodu həddindən artıq optimallaşdırmayın

Müasir brauzerlər özündə yetəri qədər kodu optimallaşdıran sxemlər ehtiva edir, yəni sizin kodunuzu həddindən artıq optimallaşdırma istəyiniz sadəcə vaxt itkisinə səbəb olacaq.

Optimallaşdırmanın harada lazım olduğunu [göstərən](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers yaxşı resurslar var. Siz bunları optimallaşdırana qədər qeyd edə bilərsiniz.

**Pis:**

```javascript
// Köhnə brauzerlərdə keşsiz `list.length` ilə hər yüklənmədə daha çox enerji sərf olunacaq.
// `list.length` yenidən hesablanması səbəbindən. Müasir brauzerlərdə bu optimallaşdırılıb.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Yaxşı:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### İstifadə edilməyən kodları silin.

İstifadə edilməyən kod təkrarlanan kod qədər pisdir, Əgər o kod hər hansısa yerdə çağırılmırsa və istifadə edilmirsə, o kodu silin.
hələ də ehtiyacınız varsa, versiya tarixçənizə atın

**Pis:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**Yaxşı:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

## **Obyektlər və Məlumat Strukturları**

### Alıcı və təyinedicilərdən istifadə (Getters, Setters)

Obyektlərdəki məlumatlara daxil olmaq üçün alıcılardan və təyinedicilərdən istifadə obyektdə xassə axtarmaqdan daha yaxşı ola bilər. "Niyə?" deyə soruşa bilərsiniz, səbəblərin siyahısı:

-  Bir obyektin xüsusiyyətində dəyişiklik etməkdən əlavə iş görmək istədikdə, kod bazanızdakı hər bir girişə baxmaq və dəyişmək lazım deyil.
- `set` edərkən doğrulama etmək asan olur.
-  Daxili təmsili əhatə edir.
-  Quraşdırma zamanı giriş(logging) və səhvlərin idarə edilməsini(error-handling) əlavə etmək asandır.
-  Ola bilər ki, siz məlumatları serverdən yükləyirsiniz, bu zaman siz obyektiniz xüsusiyyətlərin təmbəl yüklənmə(lazy load) ilə yükləyə bilərsiniz.

**Pis:**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**Good:**

```javascript
function makeBankAccount() {
  // yerli dəyişkən(private)
  let balance = 0;

  // alıcını(getter) ümumi(public) obyekt edib təyin et.
  function getBalance() {
    return balance;
  }

  // təyin edicini(setter) ümumi(public) obyekt edib təyin et.
  function setBalance(amount) {
    // ... balansı yeniləməzdən əvvəl doğrulayın
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Obyektlərin xüsusi üzvlərə sahib olmasını təmin etmək

Bu, bağlamalar vasitəsilə həyata keçirilə bilər (ES5 və aşağıda versiya).


**Pis:**

```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // İşçinin adı: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // İşçinin adı: undefined
```

**Yaxşı:**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // İşçinin adı: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // İşçinin adı: John Doe
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

## **Siniflər**

### ES5 sadə funksiyalarındansa ES2015/ES6 siniflərinə üstünlük verin

Klassik ES5 sinifləri üçün oxunaqlı sinif modeli, strukturu və metod təriflərini əldə etmək çox çətindir. Əgər mirasa(inheritance) ehtiyacınız varsa (və ehtiyacınız olmayada bilər), ES2015 / ES6 siniflərinə üstünlük verin. Bununla belə, daha böyük və daha mürəkkəb obyektlərə zərurət yaranana qədər kiçik funksiyalara üstünlük verin.

**Pis:**

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**Yaxşı:**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Zəncirləmə metodundan istifadə edin

Bu(chaining) nümunə(pattern) JavaScript'də çox üğurlu və istifadəyə yaralı bir nümunədir beləki, siz bu nümunənin uğurunu *JQuery*, *Lodash* kimi kitabxanalarda(library) görə bilərsiniz. Bu nümunə kodunuzu ifadəli və daha az detallı(kompleks) edir. Bu nümunəni istifadə etsəniz özünüzdə görəcəksiniz ki, kodunuz daha səliqəlidir.


**Pis:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**Yaxşı:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Mirasdan daha çox kompozisiyaya üstünlük verin


Dörd nəfərdən ibarət dəstə tərəfindən yaradılmış məşhur [_Dizayn Nümunələri_](https://en.wikipedia.org/wiki/Design_Patterns) kimi, siz mirasdansa kompozisiyanı seçməlisiniz. Mirasdan istifadə etmək üçün bir çox yaxşı səbəblər və kompozisiyadan istifadə etmək üçün dahada çoxlu yaxşı səbəblər var. Bu maksimum nöqtə üçün əsas məqam odur ki, əgər ağlınız instinktiv olaraq mirasdan istifadə etməyə gedirsə, onun kompozisiya probleminizi daha yaxşı modelləşdirə biləcəyini düşünməyə çalışın.

"Mirasdan nə vaxt istifadə etməliyəm?" sualı sizi maraqlandıra bilər. Bu, əlinizdə olan problemdən asılıdır, lakin bu, mirasın kompozisiyadan daha mənalı olduğu hallar:

1. Mirasınız "has-a" əlaqəsini deyil, "is-a" əlaqəsini təmsil edir (İnsan->Heyvan və İstifadəçi->İstifadəçi Təfərrüatları).
2. Siz əsas siniflərdən kodu təkrar istifadə edə bilərsiniz (İnsanlar bütün heyvanlar kimi hərəkət edə bilər).
3. Siz əsas sinifi dəyişdirərək törəmə siniflərə qlobal dəyişikliklər etmək istəyirsiniz. (Hərəkət edərkən bütün heyvanların kalori xərclərini dəyişdirin).

**Pis:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Pisdir, çünki işçilər(Employee) vergi məlumatlarına sahibdirlər. EmployeeTaxData(EmployeeTaxData) bir növ işçi (Employee) deyil.
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Yaxşı:**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

## **SOLID**

### Vahid Məsuliyyət Prinsipi (SRP)


Təmiz Kod'da deyildiyi kimi, "Sinifin dəyişməsi üçün heç vaxt birdən çox səbəb olmamalıdır". Çox funksiyalı bir sinfi sıxışdırmaq, təyyarə uçuşlarında özünüzlə götürə biləcəyiniz çamadan kimi cəlbedici ola bilər. Bununla bağlı problem var, sinifiniz konseptual olaraq uyğun gəlməyəcək və dəyişmək üçün bir çox səbəb yaranacaq. Bir sinfi dəyişmək üçün lazım olan sayını minimuma endirmək vacibdir. Sinifin çox funksiyası varsa və siz onun bir hissəsini dəyişdirirsinizsə, bunun kod bazanızdakı digər asılı modullara necə təsir edəcəyini anlamaq çətin ola bilər.

**Pis:**

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Yaxşı:**

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Açıq/Qapalı Prinsipi (OCP)

Bertrand Meyerin qeyd etdiyi kimi, “proqram təminatının *sinifləri, modulları, funksiyaları və s.* genişləndirilmək üçün açıq, lakin modifikasiya üçün qapalı olmalıdır”. Bunun mənası nədi? Bu prinsip onu bildirir ki, siz istifadəçilərə mövcud kodu dəyişmədən yeni funksionallıq əlavə etməyə icazə verməlisiniz.

**Pis:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // cavabı çevirmək və qaytarmaq
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // cavabı çevirmək və qaytarmaq
      });
    }
  }
}

function makeAjaxCall(url) {
  // sorğu və qaytarma
}

function makeHttpCall(url) {
  // sorğu və qaytarma
}
```

**Yaxşı:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // sorğu və qaytarma
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // sorğu və qaytarma
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Liskovun Əvəzetmə Prinsipi (LSP)


Bu çox sadə bir anlayış üçün qorxulu termindir.Rəsmi olaraq "Əgər S T'nin alt növüdürsə, o an S tipli obyektlər T tipli obyektlərlə (yəni S tipli obyektlər T proqramındakı obyektləri əvəz edə bilər) dəyişdirilmədən dəyişdirilə bilər, həmin proqramın arzu olunan xüsusiyyətləri (dəqiqlik, yerinə yetirilən işlər və s.) " Bu, daha dəhşətli bir tərifdir.


Bunun ən yaxşı izahı odur ki, əgər sizin üst sinifiniz və alt sinifiniz varsa, əsas sinif və alt sinif yanlış nəticələr əldə etmədən bir-birini əvəz edə bilər.Bu hələ də çaşqınlıq yarada bilər, ona görə də klassik Kvadrat Düzbucaqlı nümunəsinə baxaq. Riyazi olaraq kvadrat düzbucaqlıdır, lakin "is-a" münasibətindən istifadə edərək mirasla modelləşdirsəniz, tez bir zamanda problemlə üzləşəcəksiniz.


**Pis:**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // PİS: Kvadrat üçün 25 qaytarır halbuki, 20 olmalıdır.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Yaxşı:**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### İnterfeyslərin ayrılması prinsipi (ISP)

JavaScript-in interfeysləri yoxdur, ona görə də bu prinsip digərləri kimi ciddi şəkildə tətbiq edilmir. Bununla belə, JavaScript-in tip sisteminin olmamasında belə vacibdir.

ISP, "istifadəçilər istifadə etmədikləri interfeyslərdən asılı olmağa məcbur edilməməlidirlər." deyir. İnterfeyslər `Duck Typing` səbəbiylə JavaScript-də gizli razılaşmalardır.

A good example to look at that demonstrates this principle in JavaScript is for
classes that require large settings objects. Not requiring clients to setup
huge amounts of options is beneficial, because most of the time they won't need
all of the settings. Making them optional helps prevent having a
"fat interface".

**Bad:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

**Good:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Bağlılığı tərsinə çevirmə prinsipi (DIP)

Bu prinsip iki əsas şeyi ifadə edir:

1. Yüksək səviyyəli modullar aşağı səviyyəli modullardan asılı olmamalıdır. Hər ikisi abstraksiyalardan asılı olmalıdır.
2. Abstraksiyalar detallardan asılı olmamalıdır, detallar abstraksiyalardan asılı olmalıdır.


İlk zamanlarda bunu başa düşmək çətin ola bilər, lakin siz AngularJS ilə işləmisinizsə, bu prinsipin Dependency Injection (DI) şəklində həyata keçirildiyini görmüsünüz. Eyni anlayışlar olmasa da, DIP yüksək səviyyəli modullara aşağı səviyyəli modulların detallarını bilmək və tənzimləmək imkanı verir. DI ilədə buna nail oluna bilər. Bunun böyük bir faydası modullar arasındakı əlaqəni azaltmasıdır. Birləşmə(Coupling) çox pis birləşmə nümunəsidir, çünki kodunuzun yenidən qurulmasını çətinləşdirir.

Daha əvvəl qeyd edildiyi kimi, JavaScript-in interfeysləri yoxdur, ona görə də abstraksiyalar bağlılığ təşkil etmir. Yəni bir obyektin/sinifin digər obyektə/sinifə məruz qoyduğu metodlar və xüsusiyyətlər. Aşağıdakı bağlılığ olmaması ondan ibarətdir ki, `InventoryTracker` üçün hər hansı sorğu modulunda `requestItems` metodu olacaq.

**Pis:**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // PİS: Biz xüsusi sorğu tətbiqindən asılılıq yaratdıq.
    // `requestİtems` sadəcə `request`'də bağlı olmalıdır.
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**Yaxşı:**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// Asılılığımızı xaricdən konfiqurasiya edərək və yeritməklə
// Sorğu modulumuzu asanlıqla WebSockets istifadə edən yeni və müasir ilə əvəz edə bilərik.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

## **Test etmək**



Test canlı yayımdan daha vacibdir. Əgər testiniz yoxdursa və ya cüzi bir məbləğdirsə, kodu hər dəfə təqdim edəndə heç bir şeyin pozulmadığına əmin ola bilməzsiniz. 100% əhatəyə malik olmaq (bütün bəyanatlar və filiallar) sizə inam və dinclik bəxş edən kifayət qədər məbləğin nədən ibarət olduğuna qərar vermək sizin komandanızdan asılıdır. Bu o deməkdir ki, əla sınaq çərçivəsinə malik olmaqla yanaşı, [yaxşı əhatə alətindən](https://gotwarlost.github.io/istanbul/) istifadə etməlisiniz.

Testləri yazmamaq üçün heç bir bəhanə yoxdur. [Bir çox yaxşı JS framework'ləri](https://jstherightway.org/#testing-tools) var, ona görə də komandanızın üstünlük verdiyi birini tapın.

Komandanız üçün nəyin işlədiyini tapdıqdan sonra istifadə olunan hər yeni funksiya və modul üçün həmişə testlər yazmağı hədəfləyin. Seçim etdiyiniz metod Test Dəstəkli İnkişafdırsa (TDD), bu əladır, lakin əsas odur ki, hər hansı bir funksiyanı işə salmazdan və ya mövcud funksiyanı yenidən nəzərdən keçirməzdən əvvəl kodda məqsədlərinizə çatdığınızdan əmin olun.

### Hər test üçün bir konsepsiya

**Pis:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**Yaxşı:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**[⬆ Yuxarı Qalx](#Mündəricat)**


## **Paralellik**

### Promises'lərdən istifadə edin, callbacks-dən yox

Callback'lər təmiz deyillər və kodda həddindən artıq düyünə səbəb olurlar. ES/15 və ES/16 ilə birlikdə 'Promises'dan istifadəyə üstünlük verin.

**Pis:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**Yaxşı:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Async/Await istifadə etmək Promises istifadə etməkdən daha yaxşıdır.

Promises'lər CallBack'lərə nəzərən yaxşı bir alternativdir, amma ES/17 və ES/18 gətirdiyi async və await konsepti bu iki (Promises, CallBack) daha yaxşıdır. Tək edilməli olan funksiyanin başına `async` yazmaq və sonra proqramı `then` zinciri istifadə etmədən yazmaqdır. Bu gün ES2017 / ES8 xüsusiyyətlərindən yararlana bilirsinizsə, ondan istifadə edib daha təmiz kod yazın!


**Pis:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**Yaxşı:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

## **Xətanın idarə edilməsi**

Atılan səhvlər yaxşı bir şeydir! Bu o deməkdir ki, proqramınızda hər hansı bir xəta baş verdikdə icra müddətinin müvəffəqiyyətlə müəyyən edilməsi və onlar sizi cari yığında(stack)-də funksiyanı işə salacaq, funksiyanı dayandıracaq və konsolda xəta görüləcəkdir.

### Yaxalanan səhvlərə məhəl qoymayın

Tutulan xəta ilə heç nə etməmək sizə bu səhvi düzəltmək və ya ona reaksiya vermək imkanı vermir. Səhvləri konsolda qeyd etmək (`console.log`) konsolda çap edilmiş əşyalar dənizində itə biləcəyi qədər yaxşı deyil.

Əgər siz try/catch'də hər hansı bir kod parçasından istifadə edirsinizsə, bu o deməkdir ki, siz orada səhv ola biləcəyini düşünürsünüz və buna görə də bunun baş verdiyi zaman üçün planınız olmalı və ya kod yolu yaratmalısınız.

**Pis:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Yaxşı:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // Bir seçim (console.log-dan daha nəzərə çarpan)
  console.error(error);
  // Başqa bir seçim:
  notifyUserOfError(error);
  // Başqa bir seçim::
  reportErrorToService(error);
  // edə bilirsinizsə bu üçüdə bir edilə bilər
}
```

### Rədd edilmiş promises'lərə məhəl qoymayın

Eyni səbəblərə görə siz try/catch-də baş verən xətaları nəzərə almamalısınız.

**Pis:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**Yaxşı:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

## **Formatlama**

Formatlaşdırma subyektivdir. Buradakı bir çox qaydalar kimi, çətin və sürətli yoxdur
riayət etməli olduğunuz qayda. Əsas odur ki, formatla bağlı MÜBAHİSƏ ETMƏYİN.
Bunu avtomatlaşdırmaq üçün [tonlarla alətlər](https://standardjs.com/rules.html) var.
Birini istifadə edin! Mühəndislərin formatlama üzərində mübahisə etməsi vaxt və pul itkisidir.

Avtomatik formatlaşdırmanın səlahiyyətlərinə aid olmayan şeylər üçün
(gizinti, nişanlar və boşluqlar, ikiqat və tək dırnaqlar və s.) bura baxın
bəzi rəhbərlik üçün.

### Mənalı böyük hərflərdən istifadə edin

JavaScript tipsizdir, ona görə də kapitallaşma dəyişənləriniz, funksiyalarınız və s. Onun haqqında çox şey deyir. Komandanızın istədiklərini seçə bilməsi üçün bu qaydalar subyektivdir. Əsas odur ki, nə seçsəniz, ardıcıl olun.

**Pis:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Yaxşı:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Funksiya çağıranları ve çağrılanları yaxın olmalıdır

Əgər funksiya digərini çağırırsa, bu funksiyaları mənbədə şaquli olaraq yaxın saxlayın
fayl. İdeal olaraq, zəng edəni zəng edənin üstündə saxlayın. Biz kodu oxumağa meyl edirik
qəzet kimi yuxarıdan aşağıya. Buna görə kodunuzun oxunmasını təmin edin.

**Pis:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**Yaxşı:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

## **Şərhlər**

### Yalnızca iş məntiqi qarışıq olan şeyləri şərhləyin

Şərhlər yazmaq vacib deyil, yəni kodda mütləq şərh olmalıdır deyə bir qayda yoxdur. Yaxşı kod yazıldıqda onsuzda özünü şərhləyir.

**Pis:**

```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // string'in uzunluğu
  const length = data.length;

  // Datadaki hər simvolu döngüdən keçir
  for (let i = 0; i < length; i++) {
    // Simvol kodunu al
    const char = data.charCodeAt(i);
    // Qarışdır(Hashla)
    hash = (hash << 5) - hash + char;
    // 32-bit tam ədədə çevir
    hash &= hash;
  }
}
```

**Yaxşı:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // 32-bit tam ədədə çevir
    hash &= hash;
  }
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Kodlarınızı proqramda şərh olaraq buraxmayın

Versiya kontrol sisteminin (məs: GitHub) olmasının bir səbəbi var, Köhnə kodlariniz tarixin tozlu səhifələrinə həvalə edin.

**Pis:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Yaxşı:**

```javascript
doStuff();
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Jurnal şərhləri yazmayın

Unutmayın, versiya kontrol sistemindən istifadə edin! İstifadə edilməmiş koda, şərh edilmiş kodlara və xüsusən də log kodlarına ehtiyac yoxdur. Tarix üçün `git log` istifadə edin.

**Pis:**

```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**Yaxşı:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

### Mövqe işarələrindən çəkinin

Bunlar çox vaxt çirklənmə yaradır. Düzgün girinti və formatlaşdırma, həmçinin funksiyalar və dəyişən adlar kodunuza vizual quruluş versin.

**Pis:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**Yaxşı:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ Yuxarı Qalx](#Mündəricat)**

## Tərcümələr

Həmçinin başqa dillərdə:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenian**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![az](https://user-images.githubusercontent.com/94697857/155020237-d9cf3149-a08d-4dbb-8991-9cdd23e3b379.png) **Azerbaijani (Azərbaycanca)**: [ff-10/clean-code-javascript/](https://github.com/ff-10/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **Serbian**: [doskovicmilos/clean-code-javascript/](https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)

**[⬆ back to top](#table-of-contents)**
