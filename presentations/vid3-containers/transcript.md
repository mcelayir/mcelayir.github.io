# Giriş

Docker ile popülerleşen container teknolojisi, artık geliştiricilerin vazgelimez araçlarından biri olmuş durumda. Uygulamaları containerların içerisine koyup çalıştırabilmek, endüstrinin yazılımları çalıştırma, paketleme ve dağıtma biçimini değiştirdi. Containerlar konusu ilk duyduğu zaman çok karmaşık gelebilir, bir dönem öyleydi de. Fakat docker, işletim sistemi üzerinde izole bir filesystem ve process çalıştırmak kullanıcıdan soyutlanmış ve kullanıcının basit bir arayüzle bunu başarmasını sağlamıştır

Bu videoda bizde container konusu üzerindeki sis bulutlarını dağıtarak, teknolojiyi anlatıp, ve bir yazılım geliştirici olarak dockerdan nasıl faydalanabileceğinizi göstermeye çalışacam.

# İÇERİK

# Container nedir?

Teknoloji tarafına girmeden önce konsepti anlamaya çalışalım. Günlük hayatta çalışma masamızda rutin olarak kullandığınız eşyaları bir çekmeceye sığdırmaya çalışalım. Boyutları uygun bir çekmece ayarladıktan sonra kafamıza yatan bir gruplama yapıp, bu grupları çekmeceye yerleştirdik. Bunu yaparak eşyalara çekmece içerisinde bulunması gereken alanı verdik fakat çekmeceyi kullanmaya başlayıp etkileşime girdikçe içindeki eşyaların birbirine karışmaya bazılarının devrilip, bazılarının yuvarlandıklarını görmeye başlayacağız. Düzendeki bu bozulmaları düzeltmedikçe çekmece bizim için kullanması imkansız bir hale gelecek. Bu sorunu eşya gruplarımızı onlara uygun kaplara koyarak onların birbirlerine karışmadan, verimli bir şekilde bir arada var olmalarını sağlayabiliriz.

Containerlar ise yazılım dünyasında bu kaplara karşılık gelir. Yazılım dünyasında bir konteyner, bir yazılımın standartlaştırılmış paketidir. Uygulamalar genellikle birçok bileşenden bir araya getirilir ve bir program, bir veya daha fazla başka programın veya sistem hizmetlerinin kullanılmasını gerektirebilir. Ayrıca, yazılımın çalışması için çalışma ortamında bazı dosyaların bulunması gerekebilir. Bilgisayarların, çalıştıracağı her uygulama için bu "bağımlılıkları" içerecek şekilde yapılandırılması gerekir. Bir bilgisayarın birçok uygulamayı çalıştırabildiği ve her uygulamanın benzersiz bir bağımlılığa sahip olduğu düşünüldüğünde, bu çalışma ortamlarının yapılandırmanın ve sürdürmenin bir güçlük olduğu şaşırtıcı değildir.

Burada çok kısa kendi kişisel bilgisayarınızı düşünün. İnternette karşınıza çıkan her uygulamayı bilgisayarınıza kurduğunuzu ve işiniz bitse bile silmediğinizi düşünün. Bu uygulamalar kendileriyle birlikte çalışmaları için gereken herşeyi bilgisayara yükleyecektir. Böyle devam ettikçe bilgisayarınız şişecek ve verim alamamaya başlayacaksınız. Bu durum şirketlerin canlı sunucuları için de geçerli. 
Bu yüzdende şirketler sunucuların bakımı ve uygulamaların ayakta tutulması için para ve harcar.


Bu noktada container, uygulamaları çalıştıkları ortamdan soyutlayarak devreye girer. Uygulama, çalışma ortamı ile container içine paketlenir. Container, uygulamayı çalıştırmak için gereken hizmetleri, kütüphaneleri ve ayarları barındıran basit bir işletim sistemi sağlar. Artık bilgisayarı birden çok çalışma ortamını destekleyecek şekilde yapılandırılmak yerine containerları çalıştıracak şekilde yapılandırmak yeterli olacaktır. Bu nedenle bir konteyner, uygulamanın bir bilgi işlem ortamından diğerine hızlı ve güvenilir bir şekilde dağıtılabilmesini sağlayan standartlarşmış bir birimidir. Container tabanlı uygulamalar, hedef ortamın özel bir veri merkezi, genel bulut veya hatta bir geliştiricinin kişisel dizüstü bilgisayarı olmasına bakılmaksızın, container desteğine sahip herhangi bir ortama tutarlı bir şekilde dağıtılabilir.

Bir sanal makine (VM), bir bilgisayar sisteminin bir emülasyonudur. VM'ler, birden çok sanal bilgisayarın tek bir bilgisayar içinde çalışmasını mümkün kılar. Bir hypervisor, VM'leri çalıştırmak için gereken donanımı sanallaştırır. Konuk işletim sistemi, Sanal Makine'nin sanallaştırılmış donanım üzerinde çalışabilmesi için makineye kopyalanır. Konuk işletim sistemleri ve uygulamaları, donanım kaynaklarını tek bir ana sunucudan veya bir ana sunucu havuzundan paylaşır.

Sanal makinelerin aksine, containerlar alttaki donanımı sanallaştırmak yerine işletim sistemini sanallaştırır. Containerlar, fiziksel sunucunun işletim sisteminin üstüne oturur ve her container, ana bilgisayarın işletim sistemi kernelini paylaşır. Kütüphaneler ve binary dosyalar gibi kaynakların paylaşılması, işletim sistemi kodunu yeniden oluşturma ihtiyacını önemli ölçüde azaltır böylece ortaya performanslı bir şekilde çalıştırılabilen mikro sanal makineler ortaya çıkabilir

# CONTAINER vs VM Slaydını oku

# NEDEN CONTAINERLAR SLAYTINI OKU

# DOCKER

Containerler, container adı ile olmasada dockerdan önce de hayatımızda bulunuyordu. Fakat kullanması bizim gibi sıradan kullanıcılar için çok komplikeydi.

Docker container yaratmayı ve çalıştırmayı, sadece bir kaç komutun çalıştırılması meselesi haline getirdi ve endüstride bir devrim yarattı. Containerlar sadece konuyu bilenlerin değil basit bir bilgisayar kullanıcısının bile neredeyse hiç vakit harcamadan kullanabileceği bir seviyeye geldi ve kullanımı çok yaygınlaştı. İlk büyük açık kaynak projesi olarak ortaya çıkan Docker, bu başarısı ile, hemen sektörün standardı haline geldi.

Şimdi gelin dockerın dinamiklerine bakalım. Ana hatlarıyla Dockerın çalışmasını sağlayan 4 tane bacağı vardır.

Bunlardan ilki sistemin temel yapıtaşı olan dockerfilelardır. Dockerfile'ları bir contanierın nasıl yaratılacağını tarif eden tarif dosyaları gibi düşünebiliriz. Dockerfiledaki komutlar satır satır çalıştırılarak container imageları oluşturulur. 

İkinci önemli bacak ise İmagelardır. İmagelar bu sistemin para birimi gibidir. bir container imajın çalışan nesnesidir ve imagelar dağıtılıp, tekrar tekrar kullanılabilinir. Image oluşuturulduktan sonra public repositorylere yüklenerek paylaşılabilinir, yani benim oluşturduğum bir image benim hiç tanımadığım biri tarafından kullanılabilinir.

Üçüncü bacak ise dockerfile dan image yaratan ve bu imagelardan containerlar oluşuturup onların bir arada çalışmasını sağlayan docker enginedır. Basitçe docker engine olmazsa geri kalanın çok bi önemi yok. Komut satırından girdiğimiz docker komutları docker engine tarafından karşılanır ve yerine getirilir. Çalışacak olan containerlara ram ve cpu kaynakları docker engine tarafından sağlanır. Containerın diğer kontainerlarla konuşabilmesi ve kullanıcın container ile etkileşime geçmesi gibi işlemler de yine container engine tarafından sağlanır. Son olarak da dediğimiz gibi dockerfile dan image oluşturulmasını sağlayan docker enginedır. Yaratılan imagelar docker engine üzerinde saklanır ve ne zaman kullanmak isterseniz bu image ı kullanabilirsiniz. 

Sonuncu bacak ise bu imageların paylaşıldığı en büyük public repository olan dockerhub. Bizde başkaları da yarattıkları imageları başkalarının kullanması için dockerhuba yükler ve bunlardan iyi kalpli olanları bir dökümantasyon bile koyar. İhtiyacınız olan kontainerları dockerhubda bulup kullanmaya başlayabilirsiniz. Örnek olarak bilgisayarınıza hiç mysql yüklemeden, mysqlin dockerhubda resmi olarak paylaşılmış olan imageını alıp bilgisayarınızda çalıştırarak sanki yüklüymüş gibi kullanabilirsiniz.


