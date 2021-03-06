
Proje'de mevcut bir sayfa üzerinde değişiklik yapmak, "Adding Component in a Page (HYBRIS)" ve "Adding links to navigation 
nodes (HYBRIS)" dosyalarındaki gibi işlemler,  yeni component ekle/çıkar, yönet, yeniden konumlandır, içerik değiştir vb. 
işlemlerin gerçekleşmesi için gereken düzenlemeleri yapmak kadar basitken, mevcut olmayan ve yaratılacak yeni bir sayfa için
bir takım extra işlemlere gereksinim duyulur.

Özetle yapılması gerekenler;

__1)__ Yeni bir page template'i oluşturmak: Oluşturulacak yeni sayfanın önce ne için oluşturulacağı belirlenmeli ve sonrasında 
"Page Type" belirlenmeli. Örneğin, seçilen bir ürünün detaylarını gösteren sayfanın tipi ProductPage'dir. Tipi bu seçilen
sayfa, daha sonra template olarak şekillendirilir. Bütün ürünlerin listelendiği, product details sayfaları, tek bir formda 
olup, componentlerinin konumları belirli ve standart olmalıyken, farklılığı yaratan şey, componentlerin içerikleri olmalıdır.

Örneğin bu product details sayfalarındaki ürünün fotoğraflarının olduğu bölgeyi temsil eden component slotu bütün ürünlerde 
olmalıyken, componentin kendisi ürüne göre değişir. Farklılık bu şekilde sağlanır. Eğer daha önce kullanılan benzer bir sayfa 
var ise, onun template ini kullanıp, yeniden template hazırlamaya gerek duymayız. Örneğin Profil sayfasında adres,siparişler
vb. elementlerin listelendiğini düşünürsek ve oraya örneğin şifre işlemleri elementini eklemek istersek, yeni bir template 
oluşturmak yerine, diğer elementlerin kullandığı ve farklılaştırdığı template i biz de kullanıp özelleştirebiliriz. Hazır bir
template i kullandığımızı düşünelim.
 
 
__2)__ Yeni bir content page oluşturmak: Bu işlem, oluşturmak istediğimiz sayfanın, templatten ayrılıp özelleştirildiği fazdır. 
Projeye eklenmek istenen her sayfa için bir content page oluşturmak gereklidir. 

_____________________________________________________________________________________________________________________________

```SQL
INSERT_UPDATE ContentPage;$contentCV[unique=true];uid[unique=true];name;masterTemplate(uid,$contentCV);label;             
    defaultPage[default='true'];approvalStatus(code)[default='approved'];homepage[default='false']                             
    ;;practiseContentPage;Practise Content Page;PractisePageTemplate;practiseCMSPage                                          
```
___________________________________________________________________________________________________________________________

Şeklinde, PractisePageTemplate templateinden özelleştirilecek olan practiseContentPage oluşturulur.

__3)__ Yeni bir slot ve component oluşturmak:  Sayfamızda kullanmak istediğimiz işlevsellik içeren modüllere componentler
diyebiliriz. Örneğin sayfanın footer'ında bulunan sertifika ve hukuki hakların belirtildiği copyright bölümünü text olarak bir
paragraph component düşünebiliriz. Bu componentin içeriğini impexlerle değiştirileceği gibi arayüzlerden de değiştirebiliriz 
(wcms,hmc).  Componentlerin ve slotların sayfalarda nasıl kullanıldığını  "Adding Component in a Page (HYBRIS)" dosyasında 
anlatmıştım, inceleyebilirsin.

__4)__ Sayfamıza link oluşturmak  ve onu Navigation node'una eklemek:  ----> "Adding links to navigation nodes (HYBRIS)" 

__5)__ Controller:  Oluşturduğumuz ContentPage'i, içerisine gömmüş olduğumuz component ve slotlarla birlikte önyüz halini wcmsden
önizleyebiliriz. Navigation node'a eklediğimiz linkimiz bizi o URL'ye götürecektir fakat o URL'de wcmsde önizlediğimiz sayfanın
çağrılması için controller yazılması gerekir. En basic haliyle örnek bir controller:

_____________________________________________________________________________________________________________________________
```JAVA
@RequestMapping(value = "/practise", method = RequestMethod.GET)                                                          
 	public String enterPractisePage(final Model model) throws CMSItemNotFoundException                                      
 	{                                                                                                                       
 		final ContentPageModel cpm = getContentPageForLabelOrId(PRACTISE_CMS_PAGE);                                           
 		storeCmsPageInModel(model, cpm);                                                                                      
 		setUpMetaDataForContentPage(model, getContentPageForLabelOrId(PRACTISE_CMS_PAGE));                                    
 		editPageType(model, cpm);                                                                                             
 		return getViewForPage(cpm);                                                                                           
	}   
```
___________________________________________________________________________________________________________________________

__6)__ Jsp Oluşturmak: Biz  jsp sayfasını, oluşturduğumuz content page in belirli bir kısmında kullanmak için o sayfayı bir Jsp 
componentine atatık ve component management işlemleriyle kullanılacak sayfaya slot ile gömdük.  Eğer bütün işlemler doğru
yapıldıysa belirlediğimiz URL'ye gidince controller vasıtasıyla, hazırladığımız content page gelir. jsp componentini 
gömdüğümüz yerde de Jsp dosyasının içeriği ekrana basılır. Profil sayfası için, her bir linkin yönlendirdiği sayfalardaki 
farklılığı sağlayan bu jsplerdir.
