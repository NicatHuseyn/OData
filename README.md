# OData
OData (Open Data Protocol), URL üzərindən data mənbələrini sorğulamaq və əldə ediləcək nəticələri özəlləşdirmək üçün istifadə edilən REST və ya HTTP əsaslı bir protokoldur. HTTP əsaslı olduğu üçün bütün sorğular URL üzərindən həyata keçirilir. API-lar da əldə ediləcək məlumatların özəlləşdirilməsi və ehtiyaclara uyğun fərqli formatlara çevrilməsi üçün normalda fərqli endpointlərin yazılması ehtiyacını aradan qaldıraraq birbaşa olaraq entitylər üzərində sorğuları həyata keçirməyə imkan verir və developerların yalnız buisness logic ilə məşğul olmasını təmin edir. Beləliklə, çox sürətli şəkildə məlumat modellərinə aid xidmətlərin yaradılmasına şərait yaradır.

# OData Məlumat Modeli Necədir?
Struktural olaraq, client tərəfindən göndərilən sorğu tələbi server tərəfində işlənərək nəticə yaradılır və client-a göndərilir. Əlavə olaraq, client tərəfindən tələb olunan metadata sənədi ilə OData, istifadə etdiyi entitylərin təfərrüatları barədə client-ı məlumatlandıra bilir.

# OData ilə Eyni Funksionallığa Malik Populyar GraphQL Arasındakı Fərqlər Nələrdir?

Əslində, hər ikisi də eyni əməliyyatı yerinə yetirir. Lakin burada biz fərqlərə toxunarkən, hər iki yanaşmanın üstünlüklərini və mənfi cəhətlərini nəzərə alaraq qiymətləndirmə aparacaq və bu qiymətləndirmənin nəticəsində hansının seçilməli olduğunu müzakirə edəcəyik.

Asp.NET Core Web API developerləri üçün URL üzərindən sorğulama etmək istəndikdə, GraphQL və ya OData strukturlarından hansının seçilməsi lazım olduğunu aşağıdakı meyarlarla qiymətləndirə bilərik:

Developer usability baxımından:

- GraphQL-ə nisbətən OData tətbiqdə daha sürətli və sadə bir inteqrasiya tələb edir.
- OData, filtering, sorting, selecting və ya expanding əmrlərini GraphQL-ə nisbətən daha sürətli şəkildə dəstəkləyir.
- GraphQL, Asp.NET Core tətbiqlərində hər query üçün type, schema, query və implement resolver (həll edici tətbiq etmə) quruluşlarının inkişaf etdirilməsini tələb edir, bu da çox iş yükü 
  yaradır.
  
Performance baxımından:

- Performans, istifadə edilən yanaşmadan daha çox ediləcək query məntiqinə bağlıdır. Həm GraphQL, həm də OData, artıq hər iki halda da $select operatoru istifadə edilərək istənilən query 
  yaradılabilir və nəticə əldə edilə bilər.
- Əslində, struktural olaraq GraphQL nə qədər güclü olsa da, hər platforma və ya arxitektura uyğun kitabxanası olmadığı üçün Asp.NET Core tətbiqlərində OData daha istifadə edilə bilən və 
  inkişaf etdirilə biləndir.
  
Əlavə olaraq, aşağıdakı cədvəllərdə hər açıdan edilən müqayisələri nəzərdən keçirə bilərsiniz.
Standard API
<br>

<img src="https://www.gencayyildiz.com/blog/wp-content/uploads/2020/05/OData-Nedir-GraphQLden-Fark%C4%B1-Nedir..jpg" />

Sorğu Bacarığı
<br>
<img src="https://www.gencayyildiz.com/blog/wp-content/uploads/2020/05/OData-Nedir-GraphQLden-Fark%C4%B1-Nedir.-1.jpg" />

Surface Capability
<br>
<img src="https://www.gencayyildiz.com/blog/wp-content/uploads/2020/05/OData-Nedir-GraphQLden-Fark%C4%B1-Nedir.-2-768x239.jpg" />
<hr>
Bu qarşılaşdırmalardan əldə edilən nəticəyə görə, Asp.NET Core tətbiqlərində CRUD əməliyyatları üçün minimum zəhmət ilə məlumat mənbəyinə giriş təmin etmək istəndikdə, OData seçilə bilən protokol olacaqdır.
<br>
<hr>

#OData Query Options nədir?
OData Query Options əsasən, istifadəçi tərəfindən URL üzərindən aparılan data sorğularında (delete istisna olmaqla) yaradılan datanın istifadəçiyə göndərilməzdən əvvəl filtrdən keçirilməsi, sıralanması və s. kimi əməliyyatlarla strukturlaşdırılmasını təmin edən seçimlərdir.

OData Query Options üç kateqoriyaya ayrılır: 
- ‘System Query Options’
- ‘Custom Query Options’
- ‘Parameter Aliases’
Hansı kateqoriyaya aid olmasından asılı olmayaraq, seçimlər aşağıdakı prototipdə olduğu kimi URL üzərində sorğuya əlavə olunur.

```
https://localhost:5001/odata/{controller_name}?${options}={value}
```
İndi isə bu seçimləri ardıcıl olaraq incələyək.
<br>
<strong>System Query Options</strong>
URL üzərindən aparılan sorğular nəticəsində qaytarılacaq data üzərində filtrasiya, seçmə, sıralama kimi əməliyyatlar aparan, element sayını hesablayan, müəyyən sayda elementi ayıran və ya nəzərə almayan, həmçinin navigation property-lər vasitəsilə fiziki əlaqəyə malik olan digər cədvəlləri də sorğulamağa imkan verən seçimlərdir.
<br>
- <strong>filter</strong>
Verilerin filtrelenmesini sağlar. ‘Karşılaştırma’, ‘Mantıksal’, ‘Aritmatik’ ve ‘Fonksiyonel’ operatörlerden oluşmaktadır. Belirtilen ifadelerle yapılan işlemler neticesinde sonuç olarak true dönen öğeler sonuca eklenecektir.
Bu seçeneği kullanabilmek için endpoint üzerinden aşağıdaki gibi tanımlanması gerekmektedir.
```
app.UseEndpoints(endpoints =>
{
    endpoints.MapODataRoute("odata", "odata", builder.GetEdmModel());
    endpoints.Filter();
});
```

<b>Qarşılaşdırma operatorları</b>
<br>

- eq (Equal)
Ölkə Adı USA olanları gətir.
  ```
  https://localhost:5001/odata/personel?$filter=ulke eq 'USA'
  ```
- ne (Not Equal)
  Ölkə adı USA olmayanları gətir
  ```
  https://localhost:5001/odata/personel?$filter=ulke ne 'USA'
  ```
- gt (Greater Than)
PersonalId dəyəri 5-dən böyük olanları gətir
```
https://localhost:5001/odata/personel?$filter=personelid gt 5
```
- ge(Greater Equal)
PersonalId dəyəri 5 və 5-dən böyük olanları gətir.
```
https://localhost:5001/odata/personel?$filter=personelid ge 5
```
- lt(Less Than)
PersonalId dəyəri 5-dən kiçik olanları gətir.
```
https://localhost:5001/odata/personel?$filter=personelid lt 5
```
- le(Less Equal)
PersonalId dəyəri 5-dən kiçik və 5-ə bərabər olanları gətir.
```
https://localhost:5001/odata/personel?$filter=personelid le 5
```
<br>

<b>Məntiqi Operatorlar</b> <br>
