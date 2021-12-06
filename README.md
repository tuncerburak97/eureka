# eureka
microservice-nosql-eureka
### Rest API nedir ?

REST API modern yazılım dünyasında uygulamaların birbiri ile iletişim kurmak için kullandıkları dildir. Daha teknik bir tanım yapmak gerekir ise, REST mimarisini kullanarak uygulamalar arası veri transferini mümkün kılan web service'lere REST API denir.

Bir API ın REST API olabilmesi için ihtiyacı olan 3 şey vardır.



![client server](client-server-architecture.png)

1. Server : Söz konusu API'ı servis eden yazılım.
2. Resource : Server tarafından API aracılığıyla servis edilen veri, bu text formatında bir kullanıcı bilgisi, bir dosya, bir fotoğraf yada bunlar gibi heerhangi bir veri olabilir. Burada önemli olan her resource'un unique bir idsi olması gerekir.
3. Client : Server tarafından servis edilen API'ı kullanan yazılım.

Fakat bunlar tek başına bir API'ı Restful yapmaya yetmez, uyması gereken bazı kurallar ve kısıtlar vardır. Bu kurallar API'ın kolay kullanılabilmesini mümkün kılar. REST ifadesi **RE**presentational **S**tate **T**ransfer kavramının kısaltmasıdır ve client tarafından talep edilen resource'un o anki durumunun server'dan clienta transfer edilmesi olarak Türkçe'ye çevrilebilir. Bu transfer işlemi JSON formatında olabileceği gibi XML yada binary formatında da olabilir. Peki bu kurallar ve kısıtlar nelerdir;

- Uniform interface : Tek tip bir API olmalıdır ve tüm clientlardan servera gelen istekleer hep aynı şekilde olmalıdır
- Client — server separation : Client ve server tamamen birbirinden bağımsız olarak hareket edebilmelidir
- Stateless : Server tarafında clientla ilgili herhangi bir state tutulmamalıdır, servera gelen requestler kendi başına çalışabilecek her bilgiyi içermelidir.
- Layered system : Client herzaman doğrudan servera bağlı olmayabilir, arada load balancer yada proxy gibi ara servisler olabilir. Bu servisler sistemin scalable olmasını ve performansını artırırken client ve server arasındaki iletişimi etkilememeli.
- Cacheable : Server tarafından üreetilen responselar keşlenebilir olup olmadıklarını belirtmeli.
- Code-on-demand(optional) : Server geçici olarak clientın çalışmasını genişletebilir, bunu clienta çalıştırılmak üzere javascript gönderebilir

Peki bir client bir resourcea ihtiyaç duyarsa bunu isteği servera nasıl iletir ? Bu drurumda servera ilgilendiği kaynağın unique idsini iletmesi gerekir, bu id Rest API için o resourceun URLidir. URL ifadesi Uniform Resource Locator kavramının kısaltmasıdır. Bir de servera resource üstünde gerçekleştirmek istediği operasyonu iletmelidir, bu işlemi HTTP metodlarını kullanarak gerçekleştirir. Veritabanı uygulamalarında CRUD diye isimlendirdiğimiz Create, Read, Update ve Delete işlemlerine, API düzeyinde karşılık gelen başlıca HTTP metodları resource üzerinde işlem yapmak için kullanılır. Bu metodlar aşağıda gösterildiği gibidir;

| Veritabanı işlemleri | HTTP Metodları | HTTP Metod amacı                                 |
| -------------------- | -------------- | ------------------------------------------------ |
| Create               | POST           | Yeni bir resource u serverda oluşturmak          |
| Read                 | GET            | Clientın ihtiyacı olan resourceu serverdan almak |
| Update               | PUT            | Güncellenen resourceu serverda güncellemek       |
| Delete               | DELETE         | Serverda bulunan resourceu silmek                |

Daha önce de söylediğimiz gibi REST bir mimaridir, HTTP ise bir web protokolü. Zaman zaman insanlar REST ve HTTP kafa karışıklığı yaşamakta ve bu iki kavramı kıyaslamaya çalışmaktadır. Fakat yukarıda da belirttiğimiz gibi ikisi de kıyaslanamayacak kadar farklı kavramlardır ve birbirine alternatif değillerdir.





## Spring Boot

![Top 20 Spring Boot Interview Questions with Answers.png](https://github.com/hkarabakla/hello_java/blob/master/notes/resources/images/Top%2020%20Spring%20Boot%20Interview%20Questions%20with%20Answers.png?raw=true)

Spring framework; Spring JDBC, Spring MVC, Spring Security gibi Java ile uygulama geliştirmeyi kolaylaştıran harika modüllerden oluşmaktadır. Fakat bu modülleri kullanabilmek için pek çok konfigürasyon yapmak gerekir. Bütün bu konfigürasyonları daha kolay hale getirmek için Spring Boot'a auto-configuration özelliği eklenmiştir. Spring Boot bir uygulamanın ihtiyacı olan pek çok konfigüreasyonu default değerler ile projeye kazandırır fakat gerektiği zaman bu konfigürasyonları override etmemize de izin verir.

Spring MVC ile bir web uygulaması geliştirdiğimiz zaman uygulamayı çalıştırabilmek için bir application sunucusuna ihtiyaç duyulur. Tomcat, Jetty gibi uygulama sunucuları geliştirdiğimiz uygulamaları deploy etmek ve çalıştırabilmek için konfigüre edilmesi gerekir. Spring Boot bu konfigürasyon işlemini ortadan kaldırmak için application sunucusunu, yazdığımız uygulamanın derleenmesi sonucu ortaya çıkan jar dosyasına embedded olarak konulmaktadır. Yani harici bir application sunucusuna gerek yoktur.

Spring Framework ile uygulama geliştirirken ihtiyacımız olan bağımlılıkları Maven yada Gradle aracılığıyla projeye eklenebilir. Maven/Gradle transitive bağımlılıkların çözümlenmesi ve projenin classpathine eklenmesi konusunda çok iyi iş çıkarsa da doğrudan ilişkisi olmayan ama genellikle birlikte kullanılan bağımlılıkları çözümleyemez. Bu noktada Spring Boot starter denilen ve birbiri ile ilişkili bağımlılıkları grup halinde projeye ekleme olanağı sunan özelliğiyle karşımıza çıkıyor.

Bunların yanında Spring Boot bize prod ortamına deploy etmeye hazır bir takım özellikler de sunuyor; metrikler, health checkler ve external config gibi. Prod ortamında çalışan her uygulamanın ihtiyaç duyduğu bu özellikler Spring Boot ile otomatik olarak uygulamaya eklenir.

### Spring MVC nedir ?

Spring MVC, Spring frameworkun bir parçası olan DispatcherServlet etrafında MVC patternini uygulayan modüldür. DispatcherServlet uygulamaya gelen tüm requestleeri yakalar ve geerekli çözümlemeyi yaptıktan sonra requesti uygun handlera iletir. Request handler @Controller ve @RequestMapping anotasyonlarını kullanarak gelen requestlerden kendisine uygun olanı işleyen sınıf ve metodlardan oluşan yapıdır.

![mvc](mvc.png)



### Spring MVC anotasyonları

- @Controller
- @RestController
- @RequestMapping
- @RequestBody
- @PathVariable
- @RequestParam
- @RequestHeader
- @ResponseBody
- @ResponseStatus

## Microservice Pattern

### Monolith uygulama nedir ?

Geleneksel uygulama mimarisi olan Monolith mimarisi, pek çok modülün ve çözümün bir araya gelerek tek bir uygulama oluşturmasıyla oluşur. Örnek olarak bir e ticaret uygulamasını ele alabiliriz; bu uygulamanın bir ürün modülü, kategori modülü, kullanıcı modülü, stok modülü, sipariş modülü, teslimat modülü olduğunu 	düşünelim. Bütün bu modüllere ait kodların tek bir repositoryde tutulduğunu ve uygulama derlendiği zaman ortaya tek bir war dosyasının çıktığını düşünelim.

![overcharged-vehicles__880.jpg](https://github.com/hkarabakla/hello_java/blob/master/notes/resources/images/overcharged-vehicles__880.jpg?raw=true)



Bu mimarinin getirdiği avantajlar ve dezavantajları düşündüğümüzde karşımıza şöyle bir liste çıkıyor;

**Avantajlar:**

- Tüm uygulama kodu bir arada olduğu için deployment süreci basittir.
- Uygulama geliştirmesi nispeten daha hızlı ve kolaydır, çünkü kod tekrarı azdır, modüller birbirlerinden kodları kullanabilirler.

**Dezavantajlar:**

- Monolith uygulamalarda bütün uygulama kodu aynı repositoryde olduğundan tek bir programlama dili kullanılması zorunluluğu vardır. Günümüzde pek çok programala dili farklı problemleri çözmek için ortaya çıkmıştır. Uygulama içerisinde karşılaşılabilecek farklı problemler farklı programlama dilleri ile kolayca çözülebilecekken, aynı programlama dilini kullanma zorunluluğu işleri zorlaştırmaktadır.
- Uygulamanın tek bir modülünde değişiklik yapılması durumunda bile bütün uygulamanın build edilmesi gerekir. Bu da uygulamanın tüm kısımlarının tekrar test edilmesini gerektirir.
- Uygulama içerisinde bir modül diğer modüllerden çok daha fazla istek alabilir, bu durumda uygulamanın iyi performans göstermesi için uygulamayı scale etmek (uygulamayı daha fazla uygulama sunucusuna deploy etmek) gerekir. Tüm modüller tek bir war dosyası içerisinde paketlenmiş olduğundan bütün uygulamayı scale etmek gerekir, buna horizontal scaling denilir.
- Modüller arası iletişim için kod paylaşımı yapılır, bu paylaşımından kaynaklı modüller arası bağımlılık yüksektir.
- Modüller arası kod paylaşımı aynı zamanda farklı modüller üzerinde çalışan takımlar arasında da bağımlılık yaratır.

### Microservice nedir ?

Yukarda bahsettiğimiz sorunlara çözüm olarak microservice mimarisi ortaya atılmıştır. Microservice mimarisinde yukarıdaki örneği düşündüğümüz zaman her bir modül kendi başına bir uygulama gibi ayrı bir repositorye sahiptir ve tek başına derlenip deploy edilebilir. Microservice mimarisinde bu sorunlar etkin bir şekilde çözülürken yapısındaki karmaşadan dolayı ortaya çözülmesi gereken başka sorunlar çıkmıştır.



![microservices](https://github.com/hkarabakla/hello_java/raw/master/notes/resources/images/microservices.png)



Şimdi öncelikle avantaj ve dezavantajlarına bakalım;

**Avantajları**

- Herbir microservice tek bir repositoryde bulunduğundan teknoloji bağımlılığı ortadan kalkmıştır, herbir service farklı programlama dili ve teknolojiler kullanılarak geliştirilebilir.
- Herbir microservice tek başına build edilebildiği için bir microservicede bir değişiklik olduğunda sadece o servisi build etmek ve deploye etmek yeterlidir. Devamında da sadece o servisi test etmek yeterli olacaktır.
- Çözüm içerisinde bir microservice yoğun trafik alması yada performans sorunları yaşaması durumunda sadece o servisi scale etmek yeterli olacaktır, buna vertical scaling denilir.
- Modüller arası iletişim API yada message brokerlar aracılığıyla gerçekleştirildiğinden herhangi bir kod paylaşımı iletişim için söz konusu değildir, bu da microserviceler arası bağımlılıkları yok eder.
- Her bir microservice yada bir kaçı tek bir takım tarafından geliştirildiği için ve servisler arası kod paylaşımı olmadığı için her takım birbirinden bağımsız hareket edebilir.

**Dezavantajları**

- Bu dağıtık mimarinin getirdiği bir takım veri bütünlüğü sorunları ortaya çıkabilir.
- Herbir istek birden fazla microservis tarafından farklı teknolojiler ve yaklaşımlarla işlendiği için transaction yönetimi zordur.
- Microserviceler arası iletişimi yönetmek monolith uygulamalara göre çok daha zordur. Servislerin birbirini otomatik bulması gibi ihtiyaçlar ortaya çıkar.
- Dağıtık bir log yönetimine ihtiyaç vardır, bir request exception aldığında hatanın hangi microserviste meydana geldiğini anlamak için dağıtık log yönetim araçlarına ihtiyaç olur.
- Çok fazla tekrarlı kod yazılması gerekir, özellikle servisler arası iletişim noktasında.
- Uçtan uca senaryoları debug etmek zordur.

![Architecture of microservices](https://github.com/hkarabakla/hello_java/raw/master/notes/resources/images/Architecture-Of-Microservices-Microservice-Architecture-Edureka.png)



1. API Gateway : Hernekadar arka planda pek çok microservice bulunsa da clientın bakış açısından ortada tek bir uygulama vardır, işte bu nedenle client istekleri tek bir host kullanarak yapar. Bu istekleri microservicelerin önünde bulunan API Gateway yakalar ve her isteği ilgili microservice e yönlendirir.
2. Identity Provider : Client için authentication ve authorization bilgilerini tutan, clientın login olması sonucunda clienta uygulamaya erişim için erişim tokeni veren ve microservislerin bu tokeni doğrulamasını sağlayan merkezi kimlik yönetimi uygulamasıdır.
3. Management : Uygulamaları scale etmek gibi operasyonları yöneten araç
4. Service discovery : Mıcroservislerin ileetişim için birbirlerini otomatik olarak bulmasını sağlayan, bunu da her servisin ayağa kalkınca gelip discovery servisine register olması sonu yapabilen araç.

#### Microserviceler arası iletişim

Microserviceler iletişim kurarken iki farklı yol kullanır;

1. HTTP üzerinden rest apilar kullanarak iletişim
2. AMQP gibi asenkron mesajlaşma protokolleri üzerinden iletişim

**NoSQL Nedir?**

“NoSQL” sözcüğü ilk defa 1998 yılında Carlo Strozzi tarafından kullanılmış. Geliştirdiği ilişkisel veritabanının sorgulama dili olarak SQL’i kullanmadığını belirtmek isteyen Strozzi, açık kaynak kodlu veritabanı için NoSql DB ismini kullanmıştı. Sonradan NoSql ifadesi daha çok “ilişkisel olmayan” anlamında kullanılmış, Strozzi de ilişkisel olmayan veritabanlarının NoSql yerine NoRel(no reletional) şeklinde isimlendirilmesinin daha doğru olacağını belirtmiş.

Günümüzde ise bildiğimiz sql tabanlı veritabanları dışındaki tüm veritabanları için NoSql terimi kullanılır. İilişkisel olmayan, NoRDBMS, NonRel ve Not Only Sql de diğer ifade ediliş şekilleridir.

![img](https://miro.medium.com/max/1400/1*dTjJ6bfGypRliOZkVSTMvA.png)

NoSQL teknolojiler aslında uzun yıllardır hayatımızda, ancak günümüzde veri yelpazesinin genişlemesi, bulut, mobil, iot, sosyal medya ve big data ile oluşturulan çok çeşitli ve büyük hacimli verileri işleme gereksinimi NoSQL’in yayılmasına ve popülerliğinin artmasına olanak sağladı.

**NoSQL**; ilişkisel veritabanlarına alternatif, ilişkisel olmayan, esnek yapılı, büyük verili ve çok sayıda aktif kullanıcılı sistemlerde yüksek performans ve yönetim kolaylığı sunan veritabanı çözümüdür.



![img](https://miro.medium.com/max/679/1*rinnD4hsKGFSQASB2IKMWA.png)



Nosql veritabanı türleri :

- **Key-Value (Anahtar-Değer)** : Bilgiler bir anahtar ve karşılığında bir değer şeklinde saklanır. Kullanan sistemler : MemcacheDB , Redis , Dynamo, Riak, Voldemort , WebSphere eXtreme Scale
- **Document Oriented (Döküman Tabanlı)** : Her nesneyi (id’li bir nesneyi) ayrı bir doküman olarak XML, JSON, BSON, Binary, PDF.. gibi formatlarda saklayabilir. Kullanan sistemler : MongoDB, CouchDB , RavenDB, MarkLogic
- **Column Oriented (Sütun Tabanlı)** : Her sütun için değerleri ayrı olarak saklayan ve erişen veri sistemleri. Kullanan sistemler :Hbase, Cassandra, Accumulo
- **Graph (Çizelge)** : Kayıt graph (çizelge) adı verilen bir ilişki yapısında saklanmaktadır. Kullanan sistemler : Neo4J, OrientDB, Allegro, Virtuoso, Sones, Jena, Sesame

**NoSQL Veritabanlarının Avantajları**

Genellikle yüksek sorgu hızları sunan NoSQL veritabanları, öngörülemeyen verileri işleme konusunda çok etkilidir, sürekli genişleyen veri türleri ve modellerinin yönetilmesinde başarılıdır. Esnek, ölçeklenebilir, yüksek performanslı ve yüksek oranda işlevsel veritabanları gerektiren mobil, web ve oyun gibi birçok modern uygulama için idealdirler.

- **Esneklik**: NoSQL genellikle daha hızlı ve daha fazla yinelemeli yazılım geliştirmeyi mümkün kılan esnek şemalar sağlar. Esnek veri modeli sayesinde NoSQL veritabanları yarı yapılandırılmış ve yapılandırılmamış veriler için idealdir.
- İlişkisel (RDBMS) sistemler transaction bazlıdır ve ACID kurallarına sahiptir. NoSQL sistemler **ACID** kurallarına tamamen uymaz, pek çok NoSQL sistemde transaction kavramı da yoktur. Bu esneklik performansa olumlu etki sağlar.
- **Ölçeklenebilirlik**: NoSQL veritabanları yatay olarak genişleyebilir, yani genellikle pahalı ve kalıcı sunucular eklenerek ölçeği artırılabilecek şekilde değil, dağıtılmış donanım kümeleri kullanılarak ölçeği genişletilebilecek şekilde tasarlanır. Binlerce sunucu bir arada küme olarak çalışabilir ve çok büyük veriler üzerinde işlem yapabilirler.
- **Yüksek performans:** NoSQL veritabanları, benzer işlevlerin ilişkisel veritabanlarıyla gerçekleştirilmesi ile karşılaştırıldığında daha yüksek performansı mümkün kılan belirli veri modelleri (belge, anahtar-değer, grafik gibi) ve erişim desenlerini destekler.
- **İşlevsellik**: NoSQL veritabanları, her biri ilgili veri modeli için özel olarak tasarlanmış yüksek oranda işlevsel API’ler ve veri türleri sağlar.
- NoSQL veritabanı sistemleri ilişkisel veritabanlarına göre daha yüksek **erişilebilirlik** imkanı sunarlar. Aynı anda sisteme çok sayıda kullanıcının bağlı olduğu durumlarda kontrol ve yönetim daha kolaydır.
- NoSQL veritabanı sistemleri birçok açık kaynak kodlu projelere ve bulut bilişim teknolojilerine uygun olduğu için **maliyet** olarak ilişkisel veritabanı yönetim sistemlerine göre daha avantajlıdır. Açık kaynak kodlu ve ücretsiz seçenekler mevcuttur.
- **Bakım ve yönetimi** ilişkisel db’lere kıyasla daha kolaydır.
- NoSQL sistemler sabit tablo ve sütunlara bağımlı değildir bu da **tasarımda** bir değişikliğe gidilmesi gerektiğinde işlerin çok daha kolay olacağı anlamına gelir.

**NoSQL Sistemlerin Dezavantajları Nelerdir?**

- İlişkisel veritabanı yönetim sistemlerini kullanan uygulamaların NoSQL sistemlere taşınması zaman alıcı ve zahmetli olabilir. Sorgu tabanlı veri erişimi yerine NoSQL sistemlerdeki anahtar tabanlı yapılandırmaya gidilmesi gerekir.
- ACID (Atomicity, Consistency, Isolation, Durability) kurallarını esnettiğinden, ilişkisel veritabanı yönetim sistemlerindeki gibi transaction kavramı yoktur. Bu da veri kaybı olabileceği anlamına gelir. Finansal uygulamalarda kullanımı tercih edilmez. Yani ACID’i esneterek sağlanan ölçeklenebilme gibi avantajlar veri bütünlüğü yönünden dezavantaj oluşturur denebilir.
- NoSQL veritabanı sistemleri veri güvenliği konusunda ilişkisel veritabanı yönetim sistemleri kadar gelişmiş değiller. Dökümantasyon ve destek konularında gelişmesi gereklidir.

## Couchbase

**Couchbase;** document ve key-value tabanlı, memory-first yapısına sahip bir NoSQL veritabanı çözümüdür. Verileri JSON olarak tutar ve N1QL sorgulama diline sahiptir. Linkedin, eBay ve PayPal gibi şirketler tarafından kullanılır. 

### Couchbase’in Temel Özellikleri

Couchbase’deki veriler **Bucket** adı verilen mantıksal yapılarda saklanır. Veriler, ilişkisel veritabanlarında tablolarda tutulurken, Couchbase’de ise **Bucket** içerisinde tutulur. Bucket’da veriler 3 farklı şekilde depolanır:

- **Couchbase Bucket:** Verileri hem belleğe hem de diske yazar.
- **Memcached Bucket:** Verileri yalnızca belleğe yazar.
- **Ephemeral Bucket:** Verileri geçici olarak bellekte tutar.

![alldb](alldb.png)

Couchbase’in temel özelliklerinden bahsetmek gerekirse;



- Couchbase, document-oriented, memory-first mimari yapısına sahip bir NoSQL veritabanıdır.
- Verilerimiz JSON olarak tutulur.
- Memory first mimarisine sahip olduğu için oldukça hızlıdır. (Veriler öncelikle memory’de tutulur, daha sonra işlenir).
- MongoDB’deki gibi Document index’lemesi ile sorgularımızda büyük avantajlar sağlayabilir.
- Cluster mimarisi üzerinde çalışabilen Couchbase, verileri farklı node’lar üzerine dağıtabilir.
- Geçiçi dokümanlar oluşturmak için TTL (Time-to-Live) özelliği vardır. Örnek vermek gerekirse; geçerliliği belirli bir süre olacak şekilde sms veya şifre sıfırlama linki vb.
- Replication desteği mevcuttur. (Replication veritabanlarında yük arttığı zaman uygulanır.)

![clusterarch](clusterarch.png)

Yukarıdaki resimde de görüldüğü gibi Couchbase’in her biri farklı node’larda yer alan 6 servisi vardır. Farklı node’larda yer almasının sebebi, kaynak kullanımını arttırmak ve iş yükünü dengelemektir. Kısaca, bu servislerden bahsetmek gerekirse;

- **Data Service:** Veriye ulaşmak için kullanılır.
- **Query Service:** N1QL sorgulama dilinde belirtilen sorguları ayrıştırır, çalıştırır ve sonuçları döndürür. Query servisi, hem Data hem de Index servisleri ile etkileşimli bir şekilde çalışır.
- **Index Service:** Index oluşturmak için kullanılan servistir. Query ve Analytics servisleri ile tutarlı bir şekilde çalışır.
- **Search Service:** Full Text Search için özel olarak dizin oluşturmak için kullanılır.
- **Analytics Service:** Cpu ve memory gibi kaynak kullanımını yükseltecek Join, Set, Aggregation ve Group operasyonlarını desteklemek için kullanılır.
- **Eventing Service:** Trigger veya rapor oluşturmada kullanılan servistir.
