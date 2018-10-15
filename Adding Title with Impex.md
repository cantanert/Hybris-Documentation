Title is a thing which you can get information about current page. It located browsers tab part and also will locate at
breadcrum if site has breadcrumb. If our website has more languages from one, we must to add for each language. 


for English website:
```sql
$contentCatalog=storeContentCatalog
$contentCV=catalogVersion(CatalogVersion.catalog(Catalog.id[default=$contentCatalog]),CatalogVersion.version[default=Staged])[default=$contentCatalog:Staged]
$langTR=en
UPDATE ContentPage    ;$contentCV[unique=true]  ;uid[unique=true]                         ;title[lang=$langTR]
                      ;                         ;profile-page                     		  ;"My profile"
```
for Turkish website:
```SQL
$contentCatalog=storeContentCatalog
$contentCV=catalogVersion(CatalogVersion.catalog(Catalog.id[default=$contentCatalog]),CatalogVersion.version[default=Staged])[default=$contentCatalog:Staged]
$langTR=tr
UPDATE ContentPage    ;$contentCV[unique=true]  ;uid[unique=true]                         ;title[lang=$langTR]
                      ;                         ;profile-page                     		    ;"Profilim"
```
This processes must be done for every avaliable languages in our website. For example if we have TR and EN languages, 
so our impex declarations are like above.

Also we can give title to a page in WCMS. Sometimes it should be come more easy but we must to have an impex about it. 
It's so important. 
