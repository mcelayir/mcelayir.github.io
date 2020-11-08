# CircleCI ile CI Pipeline Oluşturma

# Giriş ve amaç

# Neden CircleCI

# CircleCI Building Blocks

## Pipeline

Çalışacak olan bütün iş akışlarını ve bu iş akışlarını çalıştırmak için gerekli olan konfigurasyon ve güvenlik anahtarları ve şifreler gibi verileri temsil eder. Projemiz için yaptığımız konfigürasyon, circleci dünyasında o proje için oluşturduğumuz pipeline yani boru hattıdır. Projemize yaptığımız her değişiklikte bu konfigürasyon dosyasında tanımlı olan workflowlar çalıştırılacaktır.

## Workflow

Bir iş akışı çalıştırıldığında onun altına tanımlanmış olan işler konfigürasyona göre sıralı veya 
paralel bir şekilde çalıştırılacaktır. CircleCi dünyasında iş yakışı hangi işlerin hangi sıraya ve koşullara göre çalışacağını belirten konfigürasyon parçasıdır. Her ne kadar bir pipeline içerisinde birden fazla workflow tanımlanabilecek olsa da best practice 1 tane tanımlamak yönündedir

## Job

Bir iş altında tanımlanmış olan adımlar bütünüdür. İş çalıştırıldığında altındaki adımlar sırasıyla çalıştırılacaktır.

## Step

Adımlar ise çalışacak olan komutlardır. Kodu versiyon kontrolden checkout etmek, cacheden veri yüklemek ve testleri çalıştırmak komutlara örnekler. CircleCI üzerinde shell komutları kullanılabileceği gibi circleci apisinde bulunan komutlarda kullanılabilir.

## Executors

Bir iş akışı altında çalışacak bütün işler yeni executor yani çalıştırıcı tarafından çalıştırılacak. Burada sıfırdan bir çalıştırıcının her iş için yeniden oluşturulacağının altını çizmekte fayda var. 
CircleCi içerisinde 4 tip çalıştırıcı barındırır. Bunlardan 3 ü linux, windows ve mac os olmak üzere işletim sistemi sanal makineleridir. Diğeri ise docker containerlarıdır, yani işler docker containerlarında çalıştırılır. Containerlar pipelineda tanımlanmış olan konfigürasyonları ve verileri kullanabilirler ve buna ek olarak üstünde çalıştıkları containerlarda yükleme ve konfigürasyon yapabilirler. Böylelikle çalıştırılması gerekenen komutları çalıştırır ve iş tamamlanır tamamlar. İş bitince container da sonlandırılır.

## Data Persistence (Veri Kalıcılığı)

Her iş için sıfırdan bir çalıştırıcı oluşturulduğu zaman yeni çalıştırıcı tertemiz bir memory ile başlatılır ve içerisinde bir önceki işten kalan veri bulunmaz. Veriyi işler arasında paylaşmak ve gelecek işlerde kullanabilmek üzere kalıcı hale getirmek için ayrıcada işlerin tekrarlanabilir ve verimli olması için CircleCI 3 tane strateji sunar.

Bunlar
- Workspace
- Cache
- Artifact

## Workspace

Aynı iş akışı içerisindeki işlerin arasında veri paylmak için workspaceler kullanılır. İş çalıştırılırken
containerın üzerinde oluşan veri daha sonradan kullanılmak için workspace'e kaydedilir. Bu veriye ihtiyacı olan bir iş workspaceden yükleyebilir. Kaydetme ve yükleme işlemleri `- persist_to_workspace` ve `-attach_workspace` adımlarının işlere eklenmesiyle olur. 
Workspacede veri 15 gün saklanır. Ama unutmamak gerekirki bu veri sadece o çalışma tekrar çalışırsa geçerli olacaktır. Aksi takdirde pipeline her çalıştırıldığında workflow için yeni bir workspace oluşturulacak yani o çalıştırma için kullanılabiliecek herhangi bir veri olmayacaktır.

## Cache

Bir pipeline'nın birden fazla çalıştırılması arasında paylaşmak için cache kullanılır. Cache genelde maliyetli, çok fazla dosyanın indirilmesini gerektiren işleri tekrarlamamak için kullanılır. Bunun önemli örneklerinden bir tanesi projenin dependencyleri için kullanılır. Projelerin dependencylerinin hangi versiyona bağlı olduğu bellidir ve çok sık değişmezler. En azından her committe değişmezler ve değişse bile hepsi bir anda değişmez ve sadece güncellenen dependencynin yeni versiyonunu indirmek yeterli olacaktır.. Bu yüzden de her çalıştırmada yeniden indirmemek için cache de saklanabilirler. Cache yazma ve yükleme `- save_cache` ve `- restore_cache` adımlarının işe eklenmesi ile olur. Cache te veri 15 gün saklanır.

## Artifact 

Artefact daha uzun süreli kullanım içindir. Örnek olarak eğer CI pipelinenınız da bir Java projeniz varsa buildiniz sonucunda büyük ihtimalle ortaya bir jar dosyası çıkıyodur veya bir android projesi için build işlemi bir apk dosyası üretir. Bu dosyalar artifact olarak tanımlanır. Artifactler `- store_artifacts` adımı ile kaydedilir. Bir çalıştırmanın sonucunda oluşan artifactleri indirebilmek için circleci apisine rest call yapmak gerekir. Artifactler 30 gün saklanır

# Kod Örneği

## Config.yml dosyası



## Job tanımlaması



