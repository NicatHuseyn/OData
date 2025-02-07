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
    https://localhost:5001/odata/personal?$filter=ulke eq 'USA'
    ```
  - ne (Not Equal)
    Ölkə adı USA olmayanları gətir
    ```
    https://localhost:5001/odata/personal?$filter=ulke ne 'USA'
    ```
  - gt (Greater Than)
    PersonalId dəyəri 5-dən böyük olanları gətir
    ```
    https://localhost:5001/odata/personal?$filter=personalid gt 5
    ```
  - ge(Greater Equal)
    PersonalId dəyəri 5 və 5-dən böyük olanları gətir.
    ```
    https://localhost:5001/odata/personal?$filter=personalid ge 5
    ```
  - lt(Less Than)
    PersonalId dəyəri 5-dən kiçik olanları gətir.
    ```
    https://localhost:5001/odata/personal?$filter=personalid lt 5
    ```
  - le(Less Equal)
    PersonalId dəyəri 5-dən kiçik və 5-ə bərabər olanları gətir.
    ```
    https://localhost:5001/odata/personal?$filter=personalid le 5
    ```
<br>

<b>Məntiqi Operatorlar</b> <br>  
  - and
    Ölkəsi USA olanlar və bölgəsi WA olan personalları gətir.
    ```
    https://localhost:5001/odata/personal?$filter=country eq 'USA' and region eq 'WA'
    ```
  - or
    Ölkəsi USA olanlar və ya bölgəsi WA olan personalları gətir.
    ```
    https://localhost:5001/odata/personal?$filter=country eq 'USA' or region eq 'WA'
    ```

<b>Aritmetik Operatorlar</b> <br>
  - add (Addition)
    Maaşa 500 əlavə olunduqda 1500-ə bərabər olan personalları gətir.
    ```
    https://localhost:5001/odata/personal?$filter=salary add 500 eq 1500
    ```
  - sub (Subtraction)
    Maaşından 500 manat çıxdıqda 1500-ə bərabər olan personalları gətir.
    ```
    https://localhost:5001/odata/personal?$filter=salary sub 500 eq 1500
    ```
  - mul (Multiplication)
    Maaşı 1.5 ilə vuranda 1500-ə bərabər olan personalları gətir.
    ```
    https://localhost:5001/odata/personal?$filter=salary mul 500 eq 1500
    ```
  - div (Division)
    Maaşı 3-ə böləndə 1500-ə bərabər olan personalları gətir.
    ```
        https://localhost:5001/odata/personal?$filter=salary div 500 eq 1500
    ```
    <br>
    <br>

<b>Funksional Operatorlar</b>
- endswith
  Adı 'y' ilə bitənləri gətir.
  ```
  https://localhost:5001/odata/personal?$filter=endswith(Name, 'y')
  ```
- startswith
  Adı 'D' ilə başlayanları gətir.
  ```
  https://localhost:5001/odata/personal?$filter=startswith(Name, 'D')
  ```
- length
  Char sayı 3-dən böyük olan dataları gətir.
  ```
  https://localhost:5001/odata/personal?$filter=length(Name) gt 3
  ```
- indexof
  Adı N hərfi ilə başlayan personalları gətir.
  ```
  https://localhost:5001/odata/personal?$filter=indexof(Name, 'N') eq 0
  ```
- trim
  Dəyəri boşluqlardan təmizlənmiş Name dəyəri 'Nicat' olanları gətir.
  ```
  https://localhost:5001/odata/personal?$filter=trim(Name) eq 'Nicat'
  ```
- year
  Doğum tarixinə görə dataları gətir.
  ```
  https://localhost:5001/odata/personal?$filter=year(BithOfYear) eq 1830
  ```
- month
  6-cı ayda anadan olanları gətir.
  ```
  https://localhost:5001/odata/personal?$filter=month(BirthOfDate) eq 6
  ```
- day
  Ayın 14-cü günündə anadan olanları gətir.
  ```
  https://localhost:5001/odata/personal?$filter=day(BirthOfDate) eq 14
  ```

Digər bütün $filter operatorlarını görmək üçün buradakı <a href="https://docs.oasis-open.org/odata/odata/v4.01/cs01/part2-url-conventions/odata-v4.01-cs01-part2-url-conventions.html#sec_SystemQueryOptionfilter">Documantion-u</a> incələyə bilərsiniz.
<br>
<hr>

<strong>$select:</strong><br>
Bir entity-dən qaytarılacaq property-lərin müəyyən edilməsini təmin edir.
Bu seçimi istifadə edə bilmək üçün endpoint üzərindən aşağıdakı kimi təyin olunmalıdır. <br>
```
app.UseEndpoints(endpoints =>
{
    endpoints.MapODataRoute("odata", "odata", builder.GetEdmModel());
    endpoints.Select();
});
```
<br>
Nümnunə üçün aşağıya baxın

```
https://localhost:5001/odata/personal?$select=Name, Surname
```
Beləliklə, client ilə server arasında ötürülən data həcmi və miqdarı azaldılaraq daha performanslı bir ötürmə təmin ediləcəkdir.
<br>
<hr>
<strong>$orderby:</strong><br>
Data üzərində sıralama aparılmasını təmin edir.
Bu seçimi istifadə edə bilmək üçün endpoint üzərindən aşağıdakı kimi təyin olunmalıdır.
<br>

```
app.UseEndpoints(endpoints =>
{
    endpoints.MapODataRoute("odata", "odata", builder.GetEdmModel());
    endpoints.Select().OrderBy();
});
```
<storng>$orderby:</strong> Kiçikdən böyüyə ‘asc’ və böyüklükdən kiçiyə ‘desc’ olmaq üzrə iki növ sıralama həyata keçirir. Bunun üçün aşağıdakı nümunələri incələyə bilərsiniz:
<br>
- asc
  Böyükdən kiçiyə doğru sıralama
  ```
  https://localhost:5001/odata/personal?$select=name, surname&$orderby=name asc
  ```
- desc
  Kiçikdən böyüyə doğru sırala
  ```
  https://localhost:5001/odata/personal?$select=name, surname&$orderby=name des
  ```
<br>
<br>

<storng>$orderby:</strong><br>
Gələn data miqdarının sayını verir.Bu seçimi istifadə edə bilmək üçün endpoint üzərindən aşağıdakı kimi təyin olunmalıdır.:<br>
```
app.UseEndpoints(endpoints =>
{
    endpoints.MapODataRoute("odata", "odata", builder.GetEdmModel());
    endpoints.Filter().Count();
});
```

Nümunə olaraq aşağıdakı sorğuları incələyə bilərsiniz:
Toplam qeyd sayını gətirir.
```
https://localhost:5001/odata/personal?$filter=year(StartWorkDate) eq 1992&$count=true
```
<br>
<strong>QEYD:</strong> tutaq ki, bizim 100 dənə Personal datamız var və ekranda 5 dənəsi gəlir. Bizlər Pagination edə bilmək üçün datanın 100 dənə olduğunu, amma 100 dənə datadan 2 dənəsinin gəldiyini bilməliyik. Bu səbəbdən 100 rəqəmini, yəni data sayını da əldə etməliyik. Bunun üçün də EDM modulları ilə edirik. EDM, bizim data modellərimizi daha yaxşı tanıyıb və daha ətraflı məlumat vermək üçün bir strukturdur. Bunun üçün Controllerimizdə aşağıdakı kimi bir kod əlavə edəcəyik.
<br>

```
   public static IEdmModel GetEdmModel()
   {
       ODataConventionModelBuilder builder = new();

       // əgər dataları EDM hissəsinə əlavə etməsək, bu zaman Count-u əldə edə bilmərik.
       builder.EntitySet<Personals>("personals");
       builder.EntitySet<Category>("categories");
       builder.EntitySet<Product>("products");
       return builder.GetEdmModel();
   }

```
<br>
Program.cs də isə aşağıdakı kimi edirik:


```
builder.Services.AddControllers().AddOData(options =>
{
    // bu feature endpoinitimizdə ODatanın icazə verdiyi query params-ları parametr olaraq göndərməyə icazə verir.
    options
    .Select()
    .Filter()
    .Expand()
    .Count()
    .OrderBy()
    .SetMaxTop(null)
    .AddRouteComponents("odata",TestController.GetEdmModel());
});
```
<br>
<br>

<storng>$top:</strong><br>
İlk n ədəd veriləri gətirir.
Bu seçimi istifadə edə bilmək üçün endpoint üzərindən aşağıdakı kimi təyin olunmalıdır.
<br>
personalId dəyərinə görə böyüklükdən kiçiyə sıralanmış siyahıdan ilk 3 verilənin adını və soyadını gətirir.
```
https://localhost:5001/odata/personal?$select=name, surname&$top=3&$orderby=personalid desc
```
<br>
<br>
<storng>$skip:</strong><br>
n ədəd dataları ötürür.
Bu seçimi istifadə edə bilmək üçün endpoint üzərinə əlavə metod yazmağa ehtiyac yoxdur. Müvafiq operativ xüsusiyyət daxili olaraq gələcəkdir.

personalId dəyərinə görə sıralanmış personallərdən 3-cü və 4-cü personali gətirir.
<br>
```
https://localhost:5001/odata/personal?$select=name, surname&$skip=2&$top=2
```
<br>
<br>

<storng>$expand:</strong><br>
Sorğu edilən entity-də mövcud olan və fiziki olaraq əlaqəyə malik olan navigation property-lərin sorğulanmasını təmin edir və bununla da əlaqəli verilərin əldə olunmasına imkan yaradır.
Bu seçimi istifadə edə bilmək üçün endpoint üzərindən aşağıdakı kimi təyin olunmalıdır.
<br>
Aşağıdakı nümunələrə baxaq
<br>
- personal entity-si daxilindəki ‘Sales’ property-si expand edilərək, əlaqəli şəkildə personallərin həyata keçirdiyi bütün satışlar gətirilir.
  ```
  https://localhost:5001/odata/personal?$expand=sales
  ```
- personalların etdiyi bütün satışlarla birlikdə yalnız ‘ShipmentName’ və ‘CustomerName’ gətirilir.
  ```
  https://localhost:5001/odata/personal?$select=name, surname&$expand=sales($select=shipmentName, CustomerId)
  ```
- personallardan ilkinin bütün satışları gətirilir.
  ```
  https://localhost:5001/odata/personal(1)?$select=name, surname&$expand=sales ($select=shipName, CustomerName)&$count=true
  ```
- personalların həyata keçirdiyi satışların miqdarını gətirir.
  ```
  https://localhost:5001/odata/personal?$select=name&$expand=sales($count=true;$select=saleId)
  ```



