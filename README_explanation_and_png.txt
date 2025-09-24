poky : Yoctonun temel yapısıdır. BitBake aracı ve recipes burada bulunur yani Yoctonun çekirdeği gibi. BitBake için gerekli yapılandırmaları sağlar.

meta-ti, meta-arm : TI işlemcileri ve kartları için gerekli recipe ve destek dosyaları ile ARM tabanlı işlemciler için dosyaları içerir. Bunları yükleyerek donanım için gerekli sürücüleri, çekirdek yamlarını ve yapılandırmaları içerir.

recipe(.bb) : bir yazılım paketinin derlenmesi için gerekli tanımlamaları içerir, kaynak kodun nereden indirileceğini belirtir.
Include(.inc) : Birden fazla recipe arasında ortak yapılandırmaları paylaşmak için kullanılır. Sonra recipe ye "require common.inc" diyerek bu yapılandırmayı ekleyebiliriz.

Bizim BitBake buildimiz sonucunda şematikdeki(embedded_linux_principle.png) gibi 4 çıktı oluşur.

Toolchain : ISA(İnstructure set architecture) ile ilgili işlemcinin cross copmlicant ı oluşur 
böylelikle işlemci üzerinde işlem yapabiliriz.

Kernel İmage : İşletim sistemimizde donanım ile software kısmında bağlantıyı oluşturan yapıyı sağlar.

bootloader image(s) : İşletim sistemimizi cihaz başladıktan sonra bellektedn alıp Ram a yükleyen yani başlatan yapıyı sağlar.

root filesystem image : İşletim sistemimizdeki gerekli kütüphaneleri yapıları içeren yapıdır. 

//// What I wish I'd known about Yocto Project **************************************************

OS_diagram *********************************************
Upstream Source (Kaynak Kodu): OS'un temel işlevlerini sağlayan, genellikle açık kaynak olarak dağıtılan kaynak kodudur. Bu kaynak kodları, çeşitli yazılım projelerinden veya topluluklardan alınır ve OS geliştirme sürecinde özelleştirilebilir.

Metadata/Inputs (Veri ve Girdiler): OS yapılandırması, bağımlılıklar, sürüm bilgisi gibi OS yapımında kullanılan tüm meta veriler ve girdilerdir. Bu bilgiler, OS’un bileşenlerinin birbirleriyle nasıl etkileşime gireceğini belirlemede kullanılır.

Build System (Yapı Sistemi): Kaynak kodunu derleyip çalıştırılabilir bir OS imajına dönüştüren yazılım araçları ve betiklerdir. Örneğin, Makefile ve CMake gibi araçlarla proje derlenir ve OS bileşenleri oluşturulur. Bu yapı sistemi, OS geliştirmede otomasyonu sağlar.

Output Packages (Çıktı Paketleri): Derlenen ve birleştirilen yazılımlar ya da bileşenlerdir. OS içinde yer alacak uygulamalar, kütüphaneler, modüller veya sürücüler, bu paketler olarak organize edilir ve kullanıcıya sunulur.

Process Steps (İşlem Adımları veya Görevler): OS geliştirme sürecinde izlenen belirli adımlar ve görevlerdir. Bu adımlar, kod derleme, test, hata ayıklama ve sürüm oluşturma gibi işlemleri kapsar.

Output Image Data (Çıktı İmajı Verisi): OS'un çalıştırılabilir bir imaj haline gelmiş son hâlidir. Bu imaj, genellikle bootloader, çekirdek ve diğer OS bileşenlerini içerir. Sistem bu imajı alıp doğrudan cihaza yükleyebilir.
**************************************

Upstram Source da , SCMs(Source Control Management) Vardır. Git den indiriyoruz. Local project de olabilir Upstream Project Relaeses de olabilir. Bunlar Source Materials oldu. Build Sistemde Source Fetching oldu(Fetch Kaynakları almak denilebilir) Yani Build System in içine girdi. Sonra Patch Application, Config/Compile/Autoconf as needed ve diğer adımlardan geçecek fakat bunlardan geçerken Metadata/Inputs(User Configuration,Meta(.bb+patches),Machine BSP Configuration,Policy Configuration) lar ile şekillenecek. Diğer build aşamalarından geçti ve Output Packages, Output Images Data(Images,Application Development SDK) verdi.

- generate ettiğn dosyaların grafiklerini çıkarmamızı istemiş bunu da ' -g ' ile ben yapacağım diyor. " -u " ile de magic decode yapıyor, hataları listeliyor.

10. kısımda Build hatasını anlamak için bize bir yapı sunuyor.

- Varolan bir image üzerinde işlem yapmamızı değil de kendi image imizi oluşturup onun üzerinde işlem yapmamızın daha iyi olacağını belirtiyor.

15. kısımda da, yapacağımız çalışmaya göre bilmemiz gereken dosyaları belirtiyor.

BSPs(Board Support Packages) : BSPs include a variety of software elements that work together to enable the OS to interact with the hardware of the specific board : 

	Device Drivers : These key components of BSPs provide the interface between the OS and the hardware devices on the board. Device drivers allow the OS to access the various hardware components, such as the CPU, memory, and peripherals

	Boot loaders :Small programs that run when the board is powered on, boot loaders are responsible for loading the OS and other software components. They also provide a way to update the software on the board by allowing new software images to be loaded over a network or other interface.

	Board-specific initialization routines : Responsible for configuring the hardware of the board, such as setting up memory or initializing peripherals, these routines ensure that the hardware is configured so that the OS can run correctly.

ISA(Instruction Set Architecture): arc,arm,arm64,ricy,x86, vb... bunlar ISA çeşitleridir. Bir bakıma işlemci mimarileridir. Derlerken akışlar benzer olmasına rağmen her işlemcide farklıdır.

*************************************************************************************************

////// yocto-slides *********************************************************************

Gömülü linux için herşeyi manuel olarak yapabilirsiniz fakat bu birçok detayı kendinizin yapması gerekecek ve bağımlılıklar cehennemine giriş yapacaksınız. Binary Distrubition (Debian, Ubuntu, Fedora, etc.) Yapılması ve kurulumu kolay fakat kişiselleştime, optimize için iyi değildir ve native compilation kullanır. Build systems(Buildroot,Yocto,PTXdist,etc.) kullanıcı için esneklik sağlar, geliştirilebilir, cross-compilation kullanır.

 embedded_linux_principle *************************
 
Toolchain: Gömülü Linux için kullanılan derleme araçları setidir. Bu araçlar, kaynak kodunu belirli bir işlemci mimarisi için derler ve çalıştırılabilir hâle getirir. Toolchain, derleyici (genellikle gcc), bağlayıcı (linker), ve libc gibi standart kütüphaneleri içerir. ARM veya MIPS gibi mimarilere göre özelleştirilir.

Kernel Image (Çekirdek İmajı): Linux çekirdeğinin derlenmiş ve çalıştırılabilir hâlidir. Kernel, donanımı yönetir, sürücüleri kontrol eder ve işletim sistemi ile donanım arasında bir köprü görevi görür. Çekirdek imajı, genellikle zImage veya uImage olarak adlandırılır ve doğrudan cihazın hafızasına yüklenerek çalıştırılır.

Bootloader Image(s) (Önyükleyici İmajlar): Sistemin açılışında çalışan ve Linux çekirdeğini başlatan yazılımdır. Bootloader, çekirdeği yükleyerek işletim sistemi çalıştırma sürecini başlatır. Gömülü Linux'ta genellikle U-Boot gibi bootloader’lar kullanılır. Bootloader, işlemciyi başlatır, bellek ayarlarını yapar ve çekirdek imajını yükler.

Root Filesystem Image (Kök Dosya Sistemi İmajı): Çalışan sistemin tüm dosyalarını ve uygulamalarını içeren kök dosya sistemi imajıdır. Kök dosya sistemi, kullanıcı uygulamaları, kütüphaneler, ayar dosyaları ve cihazın çalışması için gerekli diğer bileşenleri içerir. Bu imaj, cihazın dosya sistemi olarak yüklenir ve işletim sisteminin işlevselliğini sağlar.
************************************************************************

//// LAB0 ve LAB1  ******************************************************** 
bitbake : The main build engine command. En sonda yazdığımız tük klasörleri build ediyor ve bize son halini veriyor.
conf/ Configuration files, as before, not touched by the build.
downloads/ Downloaded upstream tarballs of the recipes used in the builds.
sstate-cache/ Shared state cache. Used by all builds.
tmp/ Holds all the build system outputs.  tmp/... nin altında birçok dosya açılacaktır

git checkout -b kirkstone-4.0.5 kirkstone-4.0.5, biz bir git reposunu indirdik. Bunun içinde buna geçiyoruz demek yani.

Rootfs : RAM'de yerleşik yürütülebilir koddur. Tüm dosyaların olduğu dosya sistemine kök dosya sistemi(rootfs) denir. Linux çekirdeği rootfs'i 'root=' yapılandırması aracılığıyla bağlar.

U-Boot : ARM, MIPS ve x86 olmak üzere birçok mimariyi destekleyen, açık kaynak kodlu bir önyükleyici uygulamasıdır.

HATALAR ********************************************************
İlk defa yaptığımızda git hata veriyorsa yani adımızı ve mailimizi istiyorsa 
git config --list ile bilgilerinizi listeleyin eğer birşey çıkmazsa demek ki kayıt olmamış
git config --global user.name "Abdullah Serdar"
git config --global user.email "abdullah@example.com" gibi kendi adınızı konfigure edebilirsiniz
 
 İlgili dosyayı "  find . -name "*bmap-tools*"  " böyle aradım ve
 ./Documents/yocto-labs/poky/meta/recipes-support/bmap-tools/bmap-tools_git.bb buradaki dosyada 12. satırı değiştirdim. 'branch=master' vardı onu 'branch=main' yaptım.
 
 /yocto-labs/poky/meta/recipes-connectivity/openssl dosyasındaki 7. satırdaki LICENSE="MIT" yaptım ve lisans uyumsuzluğu çözüldü. Bunu yaparken /yocto-labs/poky dosya düzenindeki LICENSE dosyasındaki "SPDX-License-Identifier: MIT" e bakarak yaptım.
***********************************************************************

Build sonrası tüm dosyalar " build/tmp/deploy/images/beaglebone " altında toplanıyor.

LAB1 de sd kart ile u-boot ve diğer dosyaların nasıl atılacağı ve sonrasında nasıl kurulacağını anlatıyor. Picocom ile yüklüyor. Bununla içine giriyor. Putty de kullanılabilir. Cihaz içindeki terminale erişiyor böylece.

*************************************************************************

///// LAB2 **************************************************************
Bu derste "İmaja yeni paketler ekleme, Sanal paketler ve paket varyantları,Bitbake taskları çalıştırma" nasıl olacağı anlatılıyor.

Recipe 'ler bizim fetch,configure,compile ve install a software component(application,library,...) bunların nasıl yapılacağını tanımlar. Tüm bu tasklar ayrı olarak yürütülebilir.

append, prepend, remove komutları benim IMAGE'imi layerlara ayırmak için kullandığım komutlar. Bunun sebebi bir layerla işlem yaparken o layer ın diğer layerlara bağlı kalmasını engellemek ve konfigure ederken bize serbestlik sağlamasıdır. 

Variables lar konfigure dosyalarında tutulur(.conf türleri veya $BUILDDIR/conf/local.conf), bunlar global dir. Variables lar ise Recipes lerde local olarak tutulur(.bb, .bbapend, .bbclass gibi). Ayrıca Recipes ler global scope lara da erişebilir. 

Variable_modify.png***********************
Bu dosyada belirtiyor komutları.(PDF de operatörleri ve diğer kullanımları tanımlıyorlar(PAGE 76))
******************************************

BitBake [target] ile image i oluşturuyoruz fakat bunu yaparken de seçili paketleri yada diğerlerini gibi belli durumları seçip bulid edebiliriz bunun için komutları vermiş. Örneğin bitbake [target] -s ile mevcut kurulu layerlardaki recipe'lerdaki tanımlı paketleri gösterir(86)

"bitbake -c menuconfig virtual/kernel" ile çekirdek configuration yaparız.

LAB2 inin pratiğinde "Configure the build, customize the output images and use NFS" yapılıyor. root filesystem i network kullanarak yapacağız(NFS). Uzakdaki cihaz değiştiriliyor.

" IMAGE_INSTALL_append_beaglebone = " dropbear" "i $BUILDDIR/conf/local.conf ya ekliyorum. Bu satırı $BUILDDIR/conf/local.conf dosyasına eklediğinizde, BeagleBone için oluşturulan Linux imajına dropbear paketi eklenir. dropbear hafif bir SSH sunucusudur. Bu sayede, BeagleBone cihazınıza SSH üzerinden erişim sağlayabilirsiniz. 

" bitbake -vn virtual/kernel " çekirdeğin derleme işlemi başlamadan nasıl derleneceğini gösteren ifadedir.

"PREFERRED_PROVIDER_virtual/kernel_beaglebone = "linux-dummy"  " ile bitbake i derlerken tercih edilen sağlayıcı ile derleme yapmamızı imkan verir. Bu değişiklik yapmayı öğrendik(Artık yeni kernel ile derlendi)

**************************************************************************


////// LAB3(Writing recipes)(Add a custom application)*******************************************

.bb uzantılı dosyalar, recipe lerin yazıldığı dosyalardır.  <application-name>_<version>.bb  Recipe dosyasının nasıl olduğunun bir ifadesidir.          (PN(Package Name)) (Package Version)
Recipe nin headerları, source larını filan söylüyor(100)

DL_DIR ?= "${TOPDIR}/downloads" ı konfigure ederek source u fetch edebilirsin.

"SRC_URI[md5sum] = "97b2c3fb082241ab5c56ab728522622b"" de md5 bir protokoldür. git den çekilen dosyanın yanlış olmamasını sağlar aynı zamanda ad da verilip name parametresi ile de ek olarak check edilmesi sağlanabilir. SRC_URI file:// ... kullanır ve dosyayı yüklemez sadece bir FILESPATH ile bağlantı kurdurur.

LICENSE dosyalarının düzenlenmesine geçti(108), DEPENDS ler, bağımlılıklarını, derleme veya çalışma zamanına göre belirleyebilir hatta versiyonuna göre bile özelleştirilebilir.

Task yazma kısmına girdi syntax yapısını belirtiyor. Sonra Patch yapısına giriyor. Genellikle üç tür patch yapılır git, patch ve quilt fakat çoğunlukla git kullanılır.

Recipe nin örneğini veriyor(PAGE 121) Summary,description,homepage,section,LICENSE, SRC_URI,SRCREV,S,LIC_FILES_CHKSUM, do_compile(){oe_runmake}, do_install(){install -d ${D}$..} gibi

SRC_URI kaynağı belirtir,fetch edilecek olanı.

LAB3 PRATİK*********************************************************
Lab3 pratik bölümünde herhangi bir uygulama için, örneğin internette source kod var onun için bir recipe hazırlıyacağız ve onu IMAGE'mize entegre edeceğiz. Labda bire ne yapmamız gerektiği söylenmiş ama nasıl yapmamız gerektiğine girmemiş. Onu video dan alıyorum. ninvaders oyununu çalıştırmaya çalışacak.

Bunun için adımlar
1)"cd yocto-labs/poky/meta/recipes-extended" ile ilgili sıraya git ve "mkdır ninvaders" oluştur ve içine gir.

Bizden bir .bb file ve comen one include file dan oluşturmamı istiyor. Ortak dosyayı oluşturalım.

2)'nano ninvaders.inc' ile include dosyayı oluştur. PAGE123 deki örneği şablon kabul edip dosyaya yapıştırdım. HOMEPAGE için ninvaders source code google dan araştırdım ve yapıştırdım. SUMMARY için sayfasındakini yazdım(kendim de belirtebilirdim). HOME yerinde bir tmp folderi açtım ve 'mkdir ninvaders' yaptım ve google kodunu indirip indiremediğime baktım "wget https://unlimited.dl.sourceforge.net/project/ninvaders/ninvaders/0.1.1/ninvaders-0.1.1.tar.gz" ve indi. Sonra 'SOURCEFORGE_MIRROR = "https://downloads.sourceforge.net"' kullanarak yani "wget https://downloads.sourceforge.net/ninvaders/ninvaders-0.1.1.tar.gz" ile indirmeye çalıştım ve oldu. O zaman formumuzu bulduk. Bunun sebebi "downloads.sourceforge.net" in paket saklama formatı var + {BPN}+ {BPN}-{PV}.tar.gz şeklinde. Buna göre SRC_URI yi düzenlediğim zaman 
SOURCE_URI="${SOURCEFORGE_MIRROR}/${BPN}/${BPN}/${PV}/${BPN}-${PV}.tar.gz" oldu.

3) tmp leri sildim ve tüm sayfalardan çıktım. Sonra Home a bir bootlin-yocto/resources oluşturdum ve 2 numaradaki ilk adımla ninvaders ı indirdim.

4).bb dosyası oluşturacağım. .bb formatında ninvaders_0.0.1.bb(aplication-name_version.bb) düzen bu şekildedir bende "nano ninvaders_0.1.1.bb" ile yocto ya bunun bir .bb(recipe) olduğunu belirtmek için 'cd yocto-labs/poky/meta/recipes-extended/ninvaders/' sonra 
'nano ninvaders_0.1.1.bb' yazıyorum ve bu dosyanın içine girdim. Buna da 124. sayfadaki kodu yapıştırıp taslak oluşturuyorum. Terminalde ninvaders tar. bulunduğu yere gidip 
'md5sum ninvaders-0.1.1.tar.gz' ile kodunu öğreniyorum.

'tar zxvf ninvaders-0.1.1.tar.gz' ile bu dosyanın içeriğine bakıyorum, onu tanıtmak için.

'nano ninvaders-0.1.1/README' içine girip tanımaya çalışıyorum. Depency(Requirements) ve LICENSE'ye filan bakıyorum. 'nano ninvaders-0.1.1/gpl.txt' ile LICENSE baktım.

'md5sum ninvaders-0.1.1/gpl.txt' ile 'LIC_FILES_CHKSUM' koduna baktım.

Sonra DEPENDS = "recipe-b (>= 1.2)" ekledim (PAGE 110)

ninvaders.inc de "do_compile() {  
        oe_runmake CC="${CC}"
 }
" ile cross compiler yaptım.

"  /yocto-labs/poky/meta/recipes-extended/ninvaders  " herşeyi düzenledim. En son çalışan bu.
 bitbake ninvaders ile build
 bitbake -c cleanall ninvaders ile build i temizle


**********************************************************************

**********************************************************************************************

//////LAB4 and LAB5(lab olmadan bir geçiş)(Create a Yocto layer and  Extend a recipe)***********

bbappend ve bbclass mekanizmalarının örneklerle incelenmesi
Ve diğer recipe yazma üzerine ileri konular({Packet name}_{version}.bbappend) "kime extend edecekseniz aynı işimde oluşturup .bbappend yapıyoruz. version yerin '%' de kullanılabilir.

- Biz extended bir recipe oluşturup dosyanın içine taşıdık fakat bu sağlıklı olmayan bir yaklasım Bunun yerine custum ile bir layer oluşturup yapmalıydık,(extend a recipe) şimdi onu yapacağız.

-FILESEXTRAPATHS ile bu extend dosyayı bildireceğiz(136). 
-Birçok hazır komut bas.bbclass da tanımlanır. Bunun gibi birçok class vardır(kernel.bbclass,...)
Bu class lar "/yocto-labs/poky/meta/classes"içinde var(python ve base script olarak yazılabiliyor)
-kernel class için Linux kernel i configure,compile ve bu tür yapıları gerçekleştirir.
-BitBake de dosya eklemek için inherit,include,require keywords leri ile recipes,classes ve diğer konfigurasyon dosyaları eklenebilir.
-'inherit' class lar için kullanılır. 'include' ve 'require' tüm dosyalar için kullanılabilir. 'include' kullanırsak dosya bulunamadıktan sonra herhangi bir hata üretmez.

- "bitbake -c devshell <recipe>" ile derlediğim zaman "bitbake -c devshell ninvaders" ile bana yeni bir terminal açtı ve bütün environmentleri hazır edilmiş bir binary dosyası var. Herşeyi burada test edebilirim. İstersem ' INHERIT += "buildhistory" BUILDHISTORY_COMMIT = "1" ' i localconfig dosyasına ekleyerek de buildleyebilirim.
- Source fetching de(157) bitbake edeceği dosyaların sırasını belirtir.

LAYERS : recipe ve koleksiyonların özel configurasyonlar ile toplanması.

(Page 166) Poky referans system ortak layerları var meta,meta-skelaton,meta-poky,meta-yocto-bsp bunlar tüm layerlar değil istendiği durumda eklenebilir. Layerları doğrudan modifiye etmek istenen birşey değil, direkt kendininkini oluştur. (Page 168)de geliştirilen ve hazır layerlar var. Herhangi bir layer yapmaya çalışmadan önce burada bak, belki vardır. Zaman kaybı yaşanmaz. Bundan sonraki ekleyeceğimiz layerları meta-ti ın yanına koyacağız(Buranın biraz gri bir alan olduğunu söyledi)

(source poky/oe-init-build-env) ile build ortamına sağla.
-BUILDDIR/conf/bblayers.conf:
• bitbake-layers show-layers
• bitbake-layers add-layer meta-custom
• bitbake-layers remove-layer meta-qt5  ile layerları görme, ekleme ve silme yapılabilir.

-(PAGE 171) bize önerdiği, işe yarayacak olan layerları söyler. Sonra nasıl layer oluşturulacağınız açıklar. Diğer layerlardan recipe leri kopyalama, append yap diyor.

LAB4 Practice(Create a Yocto layer)*******************************
- Bir yocto layeri oluşturacağız, onu yocto projesi ile ilişkilendireceğiz ve custom layers olarak kullanacağız.

bitbake-layers create-layer -p <PRIORITY> <layer> ile bir layer oluşturuyoruz. "//yocto-labs/poky$ bitbake-layers create-layer -p 7 meta-bootlinlabs" ile yeni layerımızı oluşturduk.

"/yocto-labs/build$ cat conf/bblayers.conf" ile dosyayı açtık ve "yocto-labs/build$ bitbake-layers add-layer ../poky/meta-bootlinlabs" ile build e ilgili dosyayı bulmasını sağladık.(Elle de yazabilirdik ilk başta yaptığımız gibi fakat komut ile yaptık). 'meta-bootlinlabs' içinde conf, recipes-example, COPYING.MIT ve README gelmiş.

-"bitbake-layers show-layers" ile layerları kontrol edince, layer ın yerini aldığını görmüş olduk.

-Bize uyarı olarak herhangi bir layer ı koplayayıp alma diyor extend et fakat bu bizim oluşturduğumuz layerlar için geçerli değil, sistemin oluşturdukları için geçerli. Bu sebeble daha önce oluşturmuş olduğumuz, ninvaders yapısını kendi oluşturduğumuz meta-bootlinlabs layerı içine alacağız. Bunun için :
1) "/yocto-labs/poky$ mkdir meta-bootlinlabs/recipes-extended" ile kendi layerımızın altına dosyamızı açtık.
2) "mv meta/recipes-extended/ninvaders/ meta-bootlinlabs/recipes-extended/" ile bu konumadan bu konuma aldık.
3) "bitbake-layers show-recipes ninvaders" ile gelip gelmediğine baktık ve gelmiş.

*************************************************************************

LAB5 Practice(Extend a recipe)***********************************************
Linux patching yapacağız ve ninvaders uygulaması için patch leme yapacağız ve bir önceki oluşturduğumuz layer içinde başka bir layer içindeki recipe yi kendi oluşturduğumuz laye içinde kullanmaya çalışacağız(extended recipes).

1)"yocto-labs/poky/meta-bootlinlabs$ mkdir -p recipes-kernel/Linux" ile ilgili yere dosyayı oluşturdum.
2)"/yocto-labs/poky/meta-bootlinlabs$ touch recipes-kernel/linux/linux-ti-staging_5.10.bbappend" ile ilgili klasörde dosyayı oluşturdum.
3)Oluşturduğum dosyaya " FILESEXTRAPATHS:prepend = "${THISDIR}/files:" " yazdım ve "/yocto-labs/poky/meta-bootlinlabs$ mkdir recipes-kernel/linux/files" oluşturdum. sonra "/yocto-labs/poky/meta-bootlinlabs$ cd recipes-kernel/linux/files/" ilgili yere gidip "yocto-labs/poky/meta-bootlinlabs/recipes-kernel/linux/files$ cp ~/yocto-labs/bootlin-lab-data//nunchuk/linux/* ." bu klasördeki dosyaları files altına yazdım ve files dosyam doldu.
4)files altındaki dosyaları teker teker .bbappend'e ekliyorum.
"
FILESEXTRAPATHS:prepend := "${THISDIR}/files:"

SRC_URI += "file://0001-Add-nunchuk-driver.patch \
            file://0002-Add-i2c1-and-nunchuk-nodes-in-dts.patch \
            file://defconfig"
"

(defconfig, default configurasyon dosyasıdır. Klasörün içerisinde bir .config ara eğer bulamazsan defconfig yap demek)

****************************************************************************
Sonrasında Kol ile oynamayı gösterdi.
****************************************************************************

/////////////LAB6 *****************************************************************
BSP Layers konusunu meta-ti ve meta-yocto-bsp layerlarında machine conf, u-boot ve kernel yapılandırma detaylarına bakıyoruz.

BSP layes birçok board iletişimi için hardware ve software yapılandırmalarını içerir. meta-ti-bsp, meta-arm-bsp veya meta-st-bsp gibi layerlardır.

-meta-bsp dosyalarının altında conf ve onun altında machine ler var. Bu '.conf' dosyaları bu hardware iletişimini gerçekleştirir. Dosya ismi ile machine ismi aynı ve doğru olmalıdır.
-makine mimarisi ve özellikleri ile ilgili yapıları içerir.

Bootloader : Bootloader, os nin başlarken bellekden ram a yükleme işlemi ile os yi başlatır.(PAGE 194) de yapısını açıklıyor. (SPL_BINARY,UBOOT_SUFFIX,UBOOT_MACHINE,....) herşey u-boot.inc içinde gerçekleşiyor.
Kernel :  "PREFERRED_PROVIDER_virtual/kernel", config dosyası içinde bunu yazar ve kernel ı oluşturur. Kernel ,çekirdek, bilgisayarda donanım (hardware) ve yazılım (software) arasındaki bağlantıyı sağlayan arabirime verilen isimdir. (PAGE 200 lerde gösteriyor)
(Video18 deki son 10-15 dk izlenmedi)

LAB6 Practice (Create a custom machine configuration)***************************

-BSP yapılarını(kernel,bootloader,...) nasıl çalıştığını öğreniriz ve sıralaması nasıl olur göreceğiz.(Mevcut layer içinde kendi board yapılandırmamızı oluşturuyoruz) 
Biz sanki custom bir donanımı ayağa kaldırıyormusuz gibi yaklaşacağız ve beaglebone u ayaklandıracağız.

1) '/yocto-labs/poky/meta-bootlinlabs/conf' konumuna bir 'machine' diye folder açıyoruz.
2)'/yocto-labs/poky/meta-bootlinlabs/conf/machine$ touch bootlinlabs.conf' ile dosyayı oluşturuyoruz.(Burada layer ismi ve makine ismi aynı ve bu bir karmaşıklık oluşturur. Bizmemiz gereken bunlar farklı ve karıştırmayalım)
3) "/yocto-labs/meta-ti/meta-ti-bsp/conf/machine" buradaki 'beaglebone.conf ve ti33x.inc' yapısına bakarak 'bootlinlabs.conf' dosyasını oluşturduk. Sonra Globale gidip "/yocto-labs/build/conf/local.conf" den MACHINE yi bootlinlabs yaptık ve derledik.
4) derleme sonrası .dtb(device tree), zImage, rootfs var fakat uboot ve birkaç yapı eksik.
5) 'bootlinlabs.conf' dosyasına bunlarıda ekledik. u-boot ekliyoruz ki derlensin ve derleme sonucu bizim images klasörümüzün içerisine konsun.
IMAGE_BOOT_FILES = "MLO u-boot.img"
IMAGE_FSTYPES +="tar.xz"
EXTRA_IMAGEDEPENDS += "u-boot"
6) Daha sonrasında yükleme işlemi yapıyor fakat benim olmadığı için boş geçiyorum.
***************************************************************  
*********************************************************************************

////////////LAB7(Distro layers,Custom images,Packagegroups)*************************************

Distro Layers : Bir Linux dağıtımının genel yapılandırmalarını, ayarlarını, ve politikalarını tanımlayan bir katmandır. Paket yönetimi, çalışma zamanı ayarları, kernel ayarları, sistem başlatma hizmetleri gibi sistemin genel davranışını kontrol eden unsurları içerir. İstersek kendi distro muzu yapabiliriz fakat biz poky kullanıyoruz.

Image : Gömülü cihazınıza yükleyeceğiniz tam işletim sisteminin derlenmiş bir versiyonudur. Yani, kernel, kullanıcı alanı araçları, kütüphaneler ve uygulamalar içeren boot edilebilir bir disk imajı oluşturur. Yocto'da bir image, bir cihaza yüklenebilecek tüm gerekli bileşenleri içeren tam bir Linux dosya sistemi oluşturur. İmage, belirli bir gömülü cihaz için özelleştirilebilir ve paket seçimleri, servisler, ve konfigürasyon ayarları gibi unsurları içerir. Yani en üst seviye recipe dir.

(PAGE 218) Image organizasyonunu anlatıyor. Package Groups, ilişkili birçok paketi bir arada kullanılabilmek için meydana getirilen recipe lerdir.

LAB7 Practice (Create a custom image)**************************************
Kendi imaj dosyalarımızı oluşturma
Paket grupları ile imaj dosyamıza eklemek istediğimiz paketleri yönetme
MACHINE bir configuration, Distro bir layer ve Image bir recipe dir.

1) "/yocto-labs/poky/meta/recipes-core/images" bu dosya yolunda image ler var biz "bitbake core-image-minimal" ı build yapıyorduk fakat diğer yapılar da var.
2) "/yocto-labs/poky/meta-bootlinlabs" yani benim layerıma gidiyorum ve bir Image oluşturacağım. Bunun için yapıyı tekrar ediyorum ve "mkdir -p recipes-core/images" yapıyı oluşturuyorum.
3) core-image-minimal.bb dosyasındakileri bootlin-images-minimal.bb a yapıştırıyorum. Şimdi bir packatgroup oluşturmak istiyorum.
4)'yocto-labs/poky/meta-bootlinlabs$ mkdir recipes-core/packagegroups' ve 'touch recipes-core/packagegroups/packagegroup-bootlinlabs-games.bb' yaparak oluşturuyorum ve slaytdaki example ile packetgroup u yazıyorum ve düzenliyorum.(PAGE 230). Artık bootlinlabs.image.minimal.bb dosyasındaki "IMAGE_INSTALL += "ninvaders"" kısmına packagegroup işmini yerleştirebilirim.
5) Eke olarak "/yocto-labs/poky/meta/recipes-core/packagegroups" klasöründen 'packagegroup-core-ssh-openssh' ıda yükledim. ve debug ettiğimde 'bitbake bootlinlabs-image-minimal' ext3 ve tar.xz uazantılı images lerimizde geliyor.

***************************************************************************

********************************************************************************** 

///////// LAB8 ***************************************************************
-Açıkkaynak, özgür yazılım ve ticari lisansların yönetimi
-Reçetelerin ötesi: PACKAGECONFIG, koşullu özellik atama, Python taskları, değişken ve task bayrakları, paketleri bölme ve rootfs inşa detayı
-meta-toolchain generic ve imaj tabanlı uygulama SDK'sı
-'devtool' mucizesi ile reçete oluşturma, güncelleme ve taşıma

License yapılarını anlatıyor.
'MIT' License yi istediğin gibi kullanabilirsin fakat diğerlerini lisans versiyonlarını filan da belirtmek gerekiyor.

-Python Task larına girdi, yapının python ile de yapılabileceğini belirtiyor. 
- Root filesystem (rootfs) yapısını anlattı, nasıl açılıyor ve süreci ilerliyor diye.

YOCTO PROJECT SDK ,cross compilar,linker,toolchain,libraries,headers vb yapıları bilgisayara kurar ve bunları işletir. Yocto da bir uygulama geliştirmek için kullanılır.

'bitbake meta-toolchain' ile bu sdk yı generate ediyoruz.Sonucun da "/tmp/deploy/sdk" bu yapıda olacak. SDK variables larını belirtiyor (PAGE 289)

Devtool, recipe oluşturmak, güncellemek ve bir recipe içinde ki uygulamayı update yapabilmek için kullanılır.

-devtool, Yocto Project tarafından sağlanan bir geliştirme aracıdır ve Yocto tabanlı gömülü Linux projelerinde yazılım geliştirmeyi kolaylaştırmak ve hızlandırmak amacıyla kullanılır. 
-devtool, Yocto'nun karmaşık yapısını basitleştirir ve geliştirme sürecini daha esnek ve hızlı hale getirir.
-'devtool add <recipe> <fetchuri>' yapı ile birlikte yapıyorsun ve devtool bize tüm herşeyi indirip recipe leri kendi oluşturuyor(Bizim en başta yaptığımız herşeyi yapıyor). Oluşturduğu MakeFile i biraz konfigure etmemiz gerekebilir.
-devtool edit-recipe <verdiğimiz recipe ismi> , ile recipe yi açıyor.
-/yocto-labs/build devtool modify ninvaders, ile hali hazırda olan uygulamayı da workspace içine alıyor. Uygulama eski yerinde kalıyor fakat yeni yerinde de kullanabiliyoruz.

-'devtool reset ninvaders' ile çıkarıyor, geri kalan source dosyasını senin silmen gerekiyor.
-'devtool build ctris' gibi şekilde kendisi build ediyor. Hata olursa PR ="r1" de
-'devtool finish -f ctris ../poky/meta-bootlinlabs' ile istenilen yere koyabiliyorum.(PAGE 295 gibi yerde açıklıyor devtool u)
-'devtool reset critis' ile de yapabiliyoruz.

-Quilt de devtool ile aynı yapı fakat devtool daha avantajlı deniyor. Kullanmasan da olur.

-SDK kurdu, fakat yapmadım.(meta-toolchaiin ile SDK kurulumu yapıldığı zaman tamamlanmadan yarıda kaldı ve sürekli hata alıyorsanız rm rf tmp/* ve rm rf sstate-cache/* ile temizle. Normalden daha uzun sürecek fakat temiz bir yapı olacak). Kurduğu yeri gösteriyor ve derlemeyi gösteriyor.
24. video yu birebir yapmadım. Eğer yapacaksan izleyebilirsin.

En sonda ctris i çalıştırıyor ve oynuyor.
******************************************************************************

/////////LAB9(Runtime Package Management, Using devtool)***********************************
-IPK paket formatına göre konfigurasyonumuzu yaparak cihazımızda opkg paket yöneticisini kullanıyoruz 
-Her paket derlendiğinde onun index i de güncellenmeli
Son video yu izle fakat burada devtool nasıl kullanılacağı belirtilmiyor. Lab9 u yapabilirsin.
Packet server yönetimini ve target configurationu belirtiyor. Build yapılarını
******************************************************************************


Verdin, Toradex de nasıl Image oluşturuluyor. İlgili paketleri ubuntuya indir.





