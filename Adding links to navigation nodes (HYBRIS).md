
![alt text](https://github.com/cantanert/Hybris-Documentation/blob/master/w.png "")

  CMSLinksComponent'den custom bir link component yarat, varsa projende kullanmak istediğin bir  navigation node'un altına, 
yoksa yeni yaratacağın bir node'un altına ekle. Ben profilim sayfasında solda bulunan AccountNavigationNode'una, favorilerimin
gösterileceği sayfayı çağıran bir wishlistNavLink oluşturmak istiyorum. Örnek olarak bir işlem yapalım:

___________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
 ```SQL
INSERT_UPDATE CMSLinkComponent;$contentCV[unique=true];uid[unique=true];name;url;&linkRef; &componentRef;styleAttributes;target(code)[default='sameWindow'];visible[default=true];external[default=false];desktopOnly[default=true]; HaveImageAndTitle[default=false] 
              ;;PractiseNavLink;Practise Nav Link;/my-account/practise;PractiseNavLink;PractiseNavLink;pratik;;true;;;                                                                                                                                                
                                                                                                                                                                                                                                                         
 INSERT_UPDATE CMSNavigationNode;$contentCV[unique=true];uid[unique=true];name;links(&linkRef);parent(uid, $contentCV);visible[default=true];&nodeRef                                                                                               
              ;;AccountNavigationNode;Account Navigation Node;UpdateProfileNavLink,OrderHistoryNavLink,AddressBookNavLink,CardNavLink,UpdatePasswordNavLink,PromotionsNavLink,FAQNavLink,PractiseNavLink;SiteRootNode;;MaviAccountNavigationNode;                                                                

```
_________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

  Burada PractiseNavLink isminde bir link component tanımladık ve ona some attributes verdik. Ve bu componenti nerede kullanmak 
istiyorsak onun altına eklemeliyiz. Burada biz profil sayfasındaki sol navigation node'unun içerisine, diğer mevcut olan 
componentlerin en sonuna ekledim. Bunlar alt alta listeliyse en altına, soldan sağa inline listeli ise en sağına eklenecektir. 

  __Örneğin;__ PromotionsNavLink, AccountNavigationNode'un altında eklenmiş olmasına rağmen, önyüzde listede gözükmüyorsa bunun 
sebebi, o linkin ilk oluşturulma aşamasında visibilitysi false olarak atanmıştır. Biz de şimdi oluşturduğumuz PractiseNavLink'i,
göstermek istemediğimiz zaman, visibility'sini none yaparız. Örneğin o kısımda bir geliştirme yapılacak ve o geliştirme canlıya 
alınana kadar gözükmesini istemiyorsak, tek yapmamız gereken o tablodaki linkin bulunduğu enteryden visibility kısmını update 
etmemiz...



