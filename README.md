# OData
OData (Open Data Protocol), URL üzərindən data mənbələrini sorğulamaq və əldə ediləcək nəticələri özəlləşdirmək üçün istifadə edilən REST və ya HTTP əsaslı bir protokoldur. HTTP əsaslı olduğu üçün bütün sorğular URL üzərindən həyata keçirilir. API-lar da əldə ediləcək məlumatların özəlləşdirilməsi və ehtiyaclara uyğun fərqli formatlara çevrilməsi üçün normalda fərqli endpointlərin yazılması ehtiyacını aradan qaldıraraq birbaşa olaraq entitylər üzərində sorğuları həyata keçirməyə imkan verir və developerların yalnız buisness logic ilə məşğul olmasını təmin edir. Beləliklə, çox sürətli şəkildə məlumat modellərinə aid xidmətlərin yaradılmasına şərait yaradır.

#OData Məlumat Modeli Necədir?
Struktural olaraq, client tərəfindən göndərilən sorğu tələbi server tərəfində işlənərək nəticə yaradılır və client-a göndərilir. Əlavə olaraq, client tərəfindən tələb olunan metadata sənədi ilə OData, istifadə etdiyi entitylərin təfərrüatları barədə client-ı məlumatlandıra bilir.

OData ilə Eyni Funksionallığa Malik Populyar GraphQL Arasındakı Fərqlər Nələrdir?

Əslində, hər ikisi də eyni əməliyyatı yerinə yetirir. Lakin burada biz fərqlərə toxunarkən, hər iki yanaşmanın üstünlüklərini və mənfi cəhətlərini nəzərə alaraq qiymətləndirmə aparacaq və bu qiymətləndirmənin nəticəsində hansının seçilməli olduğunu müzakirə edəcəyik.

Asp.NET Core Web API developerləri üçün URL üzərindən sorğulama etmək istəndikdə, GraphQL və ya OData strukturlarından hansının seçilməsi lazım olduğunu aşağıdakı meyarlarla qiymətləndirə bilərik:

Developer usability baxımından:

- GraphQL-ə nisbətən OData tətbiqdə daha sürətli və sadə bir inteqrasiya tələb edir.
- OData, filtering, sorting, selecting və ya expanding əmrlərini GraphQL-ə nisbətən daha sürətli şəkildə dəstəkləyir.
- GraphQL, Asp.NET Core tətbiqlərində hər query üçün type, schema, query və implement resolver (həll edici tətbiq etmə) quruluşlarının inkişaf etdirilməsini tələb edir, bu da çox iş yükü 
  yaradır.
Performance (Performans) baxımından:

- Performans, istifadə edilən yanaşmadan daha çox ediləcək query məntiqinə bağlıdır. Həm GraphQL, həm də OData, artıq hər iki halda da $select operatoru istifadə edilərək istənilən query 
  yaradılabilir və nəticə əldə edilə bilər.
- Əslində, struktural olaraq GraphQL nə qədər güclü olsa da, hər platforma və ya arxitektura uyğun kitabxanası olmadığı üçün Asp.NET Core tətbiqlərində OData daha istifadə edilə bilən və 
  inkişaf etdirilə biləndir.
  
Əlavə olaraq, aşağıdakı cədvəllərdə hər açıdan edilən müqayisələri nəzərdən keçirə bilərsiniz.
Standard API
