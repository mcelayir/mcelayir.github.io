# CircleCI ile CI Pipeline Oluşturma

# Giriş ve amaç

Bu videoda, circleci ürününü tanıyacaz ve githubdaki projemiz için circleci ile bir continous integration pipelıne mızı oluşturup projemizi derleyeceğiz, testleri çalıştırdıktan sonra çıktıları oluşturup yayınlayacağız.

# Neden CircleCI



# CircleCI Building Blocks

## Pipeline

Çalışacak olan bütün iş akışlarını ve bu iş akışlarını çalıştırmak için gerekli olan konfigurasyon, güvenlik anahtarları ve şifreler gibi verilerin hepsini temsil eder. Farkı bir deyişle bütün çalıştırma ortamını temsil eder. Projemiz için yaptığımız konfigürasyon, CircleCI dünyasında o proje için oluşturduğumuz pipeline yani boru hattıdır. Projemize yaptığımız her değişiklikte bu konfigürasyon dosyasında tanımlı olan iş akışları çalıştırılacaktır.

## Workflow

CircleCi dünyasında iş akışı, hangi işin hangi sırayla ve hangi koşullarda çalışacağını belirleyen yapılandırma parçasıdır. Bir iş akışı başladığında, iş akışı altında tanımlanan işler çalıştırılacaktır. Bir iş başarısız olursa iş akışı sona erecek veya son iş bittiğinde tamamlanacaktır. Bir ardışık düzen içinde birden fazla iş akışı tanımlanabilse de, en iyi uygulama birini tanımlamaktır.

## Job

Bir iş altında tanımlanmış olan adımlar bütünüdür. İş çalıştırıldığında altındaki adımlar sırasıyla çalıştırılacaktır.

## Step

Adımlar ise çalışacak olan komutlardır. Kodu versiyon kontrolden checkout etmek, cacheden veri yüklemek ve testleri çalıştırmak komutlara örnekler. CircleCI üzerinde shell komutları kullanılabileceği gibi circleci apisinde bulunan komutlarda kullanılabilir.

## Executors

Bir iş akışı altında çalışacak bütün işler yeni bir executor yani çalıştırıcı tarafından çalıştırılacaktır. Burada her iş için yeniden bir çalıştırıcı oluşturulacağının altını çizmekte fayda var. CircleCI içerisinde 4 tip çalıştırıcı barındırır. Bunlardan üç tanesi Linux, Windows ve MacOs olmak üzere işletim sistemi sanal makineleridir. Diğeri ise Docker containerlarıdır, yani işler docker containerlarında çalıştırılır. Containerlar pipelineda tanımlanmış olan konfigürasyonları ve verileri kullanabilirler ve buna ek olarak çalışacak containerlarda yükleme ve konfigürasyon yapabilirler. Böylelikle çalıştırılması gerekenen komutları çalıştırır ve iş tamamlanır tamamlar. İş bitince container da sonlandırılır. Son olarak belirmekte fayda var ki docker imajı oluşturma gibi docker komutu çalıştırma gereken işlerde remote containerda çalıştırılır bunun için `- setup_remote_docker` adımının işe eklenmesi gerekir.

## Data Persistence (Veri Kalıcılığı)

Her iş için sıfırdan bir çalıştırıcı oluşturulduğu zaman yeni çalıştırıcı tertemiz bir memory ile başlatılır ve içerisinde bir önceki işten kalan veri bulunmaz. Veriyi işler arasında paylaşmak ve gelecek iş akışlarında kullanabilmek üzere kalıcı hale getirmek için, ayrıca işlerin tekrarlanabilir ve verimli olması için CircleCI 3 tane strateji sunar.

Bunlar
- Workspace
- Cache
- Artifact

## Workspace

Aynı iş akışı içerisindeki işlerin arasında veri paylaşmak için workspaceler kullanılır. İş çalıştırılırken containerın üzerinde oluşan veri daha sonradan kullanılmak için workspace'e kaydedilir. Bu veriye ihtiyacı olan bir iş workspaceden yükleyebilir. Kaydetme ve yükleme işlemleri `- persist_to_workspace` ve `-attach_workspace` adımlarının işlere eklenmesiyle olur. 
Workspacede veri 15 gün saklanır. Ama unutmamak gerekirki bu veri sadece o çalışma tekrar çalışırsa geçerli olacaktır. Aksi takdirde pipeline her çalıştırıldığında workflow için yeni bir workspace oluşturulacak yani o çalıştırma için kullanılabiliecek herhangi bir veri olmayacaktır.

## Cache

Bir pipeline'nın birden fazla çalıştırılması arasında paylaşmak için cache kullanılır. Cache genelde maliyetli, çok fazla dosyanın indirilmesini gerektiren işleri tekrarlamamak için kullanılır. Bunun önemli örneklerinden bir tanesi projenin dependencyleri için kullanılır. Projelerin dependencylerinin hangi versiyona bağlı olduğu bellidir ve çok sık değişmezler. En azından her committe değişmezler ve değişse bile hepsi bir anda değişmez ve sadece güncellenen dependencynin yeni versiyonunu indirmek yeterli olacaktır.. Bu yüzden de her çalıştırmada yeniden indirmemek için cache de saklanabilirler. Cache yazma ve yükleme `- save_cache` ve `- restore_cache` adımlarının işe eklenmesi ile olur. Cache te veri 15 gün saklanır.

## Artifact 

Artefact daha uzun süreli kullanım içindir. Örnek olarak eğer CI pipelinenınız da bir Java projeniz varsa buildiniz sonucunda büyük ihtimalle ortaya bir jar dosyası çıkıyodur veya bir android projesi için build işlemi bir apk dosyası üretir. Bu dosyalar artifact olarak tanımlanır ve CircleCI bu artifactleri bizim için saklayabilir. Artifactler `- store_artifacts` adımı ile kaydedilir. Bir çalıştırmanın sonucunda oluşan artifactleri indirebilmek için circleci apisine rest call yapmak gerekir. Artifactler 30 gün saklanır

# Kod Örneği

## Config.yml dosyası



## Job tanımlaması



