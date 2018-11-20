
Cookie adında tablo/model oluşturuyoruz. Tablo yeni oluşturulacağı için autocreate=”true” çektik. 
Autocreate=”true” olduğundan deployment tagı da kullanılmalı. Table ismini burada verdik. Attributes/attribute 
tagı ile de tabloda column oluşturuyoruz. Qualifier ile column nameini, type ile String olacağını belirttik. 
Caseimiz de cookie mesajı sunmak ve bu mesajı kullanıcının gördüğüne dair log tutmak. Bunun için IP adresi ve o
anın dateine ihtiyacımız var. Date, createddate olarak her tablonun her enterysi için varsayılan olarak tutulduğundan
ona column açmadık.

```XML
<itemtype code="CookieTypeName" autocreate="true" generate="false">
  <deployment table="cookieTableName" typecode="18000"/>			
    <attributes>
      <attribute qualifier="IpColumnName" type="java.lang.String">
        <persistence type="property"/>	
      </attribute>				
    </attributes>
</itemtype>
```

İstekleri karşılayacak ve işleme sokacak controller;

```Java
@Controller
@RequestMapping(value = "/cookiepolicy")
public class CookiePolicyAgreement extends AbstractController {

    @Autowired
    private ModelService modelService;

    protected ModelService getModelService() {
        return modelService;
    }

    @Required
    protected void setModelService(ModelService modelService) {
        this.modelService = modelService;
    }

    @RequestMapping(value = "/agreement", method = RequestMethod.POST)
    public void setCookieUser(HttpServletRequest req)
    {
        final CookieUsersModel cookieUsersModel = getModelService().create(CookieUsersModel.class);
        cookieUsersModel.setUserIpAdress(findIpAddress(req));

        getModelService().save(cookieUsersModel);

    }

    private String findIpAddress(final HttpServletRequest request)
    {
        String ipAddress = request.getHeader("X-FORWARDED-FOR");
        if (ipAddress == null)
        {
            ipAddress = request.getHeader("X-Real-IP");
            if (ipAddress == null)
            {
                ipAddress = request.getRemoteAddr();
            }
        }
        return ipAddress;
    }

}

```

Demin oluşturduğumuz tabloyu şekildeki gibi dolduruyoruz. 
Ön tarafta cookie bildirimi için gösterdiğimiz divdeki kapatma butonuna 
tıklandığında bu controllera bir http request atmamız ve tarayıcıya bir cookie 
set etmemiz ve divin displayini none yapmamız lazım. Bu cookie durduğu sürece bir daha hiç bu div görünür olmamalı.

```Javascript
$(document).on(“ready”,function(){
        if(getCookie("CookieNotification")!="true"){
        $(".cookieSection").css("display","block");
    } 
});

$(document).on("click",".cookieCloseButton",function () {
    $(".cookieSection").css("display","none");
    document.cookie= "CookieNotification= true ; path=/ ";
    $.ajax({
        url: '/cookiepolicy/agreement',
        type: 'POST',
    });
});

```

Şuan Önyüzde gösterdiğimiz divden X butonuna tıkladığımda, ip adresim ve o anın date’i oluşturduğumuz tabloda tutuluyor. HMC’de de bu tabloyu gösterebiliyor, gerektiğinde gereken IPnin işlem geçmişini listeleyebiliyor olmam lazım.

```XML
<typeref type="CookieTypeName"/>

Önce type’ın referansını veriyoruz aşağıda dolduracağız.

    <type name="CookieTypeName" mode="append">
        <organizer>
            <search mode="replace">
                <condition attribute="IpColumnName"/>
                <condition attribute="createdTime"/>
            </search>
            <result>
                <listview mode="replace">
                    <itemlayout>
                        <attribute name="IpColumnName" width="350"/>
                        <attribute name="createdTime" width="150"/>
                    </itemlayout>
                </listview>
            </result>
            <editor mode="append">
                <essentials mode="replace">
                    <listlayout>
                        <table>
                            <tr>
                                <td><attribute name="IpColumnName" /></td>
                                <td><attribute name="createdTime" /></td>
                            </tr>
                        </table>
                    </listlayout>
                </essentials>
            </editor>
        </organizer>
        <defaultreference mode="replace" searchattribute="IpColumnName">
            <itemlayout>
                <attribute name="IpColumnName" />
            </itemlayout>
        </defaultreference>
    </type>
```

HMC’ye ekledik, şimdi variable localization gerekli;

```properties
type_tree_cookietypename                                                         = Cookie Users
```
Burdan, HMC’de Types’ın altındaki ağaç görünümünde [cookieTypeName] şeklinde görülen başlığı lokalize edip, Cookie Users olarak görünmesini sağladık. Ama bu sadece sol navbar’daki link için lokalizasyon. İçerisine girdiğimizdeki tablo column name’leri de düzenlemek gerekli.
```properties
type.cookietypename.IpColumnName.name                                            = User IP adress
```
içeride verileri listelerkenki column name artık User IP adress set edildi. (eskiden tablodaki IpColumnName gibi bir şey idi).
