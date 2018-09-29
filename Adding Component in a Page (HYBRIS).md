  
  
  Jsp dosyasından referans alan bir component için JspIncludeComponent kullanılarak bir değişkene jsp nin pathi verilir. 
Bu değişken ismi bir jsp pathinin yerini almış olur. (Örneğin üz metinden oluşacak bir component için CMsParagraphComponent
kullanılır gibi ). Bu componentleri sayfalarda kullanabilmek için slotlar oluşturulur(inşert_update ContentSlot).
Daha sonra bu slotu kullanıcağın sayfaya ekleyebilmek için, ContentSlotForPage, oluşturduğumuz content slot ile insert_update 
edilmeli, bu işlem sırasında o slotun sayfadaki yerini ve adını temsil eden bir isim verilir. WCMs de sayfana baktığın zaman, 
slotun entegre edildiği  sayfa pozisyonunda, bu isim gözükür ve  en başta oluşturduğumuz o component, slotlar vasıtası ile, 
sayfanın o istediğimiz yerine yerleştirilmiş olur. 

______________________________________________________________________________________________________________________________
 ```SQL
 INSERT_UPDATE JspIncludeComponent;$contentCV[unique=true];uid[unique=true];name;page;actions(uid,$contentCV);&componentRef 
                                                                                                                            
 ;;PractisePage;Practise  Page;$jspPath/membership/profile/myAccountWishlistPage.jsp;; PractisePage                         
```
____________________________________________________________________________________________________________________________

myAccountWishlist.jsp sayfasını, PractisePage isimli component tutuyor artık. sonra;

_____________________________________________________________________________________________________________________________
 ```SQL
 INSERT_UPDATE ContentSlot;$contentCV[unique=true];uid[unique=true];name;active[default=true];cmsComponents(&componentRef);;;            
                                                                                                                           
 ;;Section2BSlot-Practise-CP;Section2B Slot for Practise (CP) Page;;PractisePage;;;                                        
 ```
___________________________________________________________________________________________________________________________

Hedef sayfanın, section2b (proje yapısına göre farklılık gösterebilir) kısmında kullanılmak üzere bir slot oluşturduk. Ancak 
ismini section2bslot koyduk diye sadece orada kullanmak zorunda değiliz şuan için. Bu isim, bu componenti, herhangi bir 
sayfanın herhangi bir yerinde kullanmaya yarayacak adlandırmadır. spesifik olarak bir sayfada kullanmak içinse;

_____________________________________________________________________________________________________________________________
```SQL
INSERT_UPDATE ContentSlotForPage; $contentCV[unique = true]; uid[unique = true]; position[unique = true]; page(uid, $contentCV)[unique = true]; contentSlot(uid, $contentCV)[unique = true];;;                                                           
                                                                                                                           
 ;;Section2B-Practise-CP-Page;Section2B;practise-CP;Section2BSlot-Practise-CP;;;                                           
```
___________________________________________________________________________________________________________________________

komutu eklenir. practise-CP sayfasında, section2B bölümüne, section2Bslot-Practise-CP slotu vasıtasıyla PractisePage
componenti gömdük.Bu sayfanın bu bölgesine de, bütün bu gömülen slot ve componentleriyle birlikte bir uzay olarak 
Section2B-Practise-CP-Page denmiştir. Yani o bölgenin ismi  Section2B-Practise-CP-Page'dir artık. WCMS'den sayfa ayrıntılarına
bakınca, o bölgenin bu şekilde isimlendirildiğini, Admin console undan bakınca da o sayfa altında o isimli slotun olduğu
görülecektir.



