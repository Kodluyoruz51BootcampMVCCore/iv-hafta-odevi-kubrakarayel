# iv-hafta-odevi
*  Geri dönen bilgiye göre yeşil renkte onay mesajı gösterilmesi. (örneğin:view bag)

*  AddMVC - AddMVCCore - AddDateAnnotations nedir? Nerelerde eklenmelidir?

 * Shanpshot nedir? nasıl değişir? neden alınır?

*  Jquery Calender--> [Datapicker](https://jqueryui.com/datepicker/) --> DueAt'i takvim tipinde eklemek nasıl yapılır? Araştırınız. (DateTimeUffset tipinden atamalar oluşucak)

*  First- FirstOrDefault ve Single- SingleOrDefault nedir? Aralarındaki farkı araştırınız.

*  En kısa null check nasıl yapılır?

* partial view nedir?

* Farklı authentication bulup,aynı işleri farklı yollar ile yapanları araştırınız.

* Ödev- Razor Pages/MVC Projects karşılaştırmasını yapınız.


 ### 1) Geri dönen bilgiye göre yeşil renkte onay mesajı gösterilmesi. (örneğin:view bag)
 #### ViewBag
 
 `@ViewBag.SuccessMessage` // _View_
 
 `ViewBag.SuccessMessage = "Success!";` // _Controller_

 
 
 #### TempData
 `@TempData["Message"]` // _View_
 
 `TempData["Message"] = "Success!";` // _Controller_
 
 
### 2) AddMVC - AddMVCCore - AddDateAnnotations nedir? Nerelerde eklenmelidir?
#### AddMvc()
Finally, we have `AddMvc()`, which simply registers the entire kitchen sink of all the features. It gives you:

everything that `AddControllersWithViews()` does
everything that `AddRazorPages()` does
```
// ready for everything
services.AddMvc()
```
While I'd imagine you know what you are trying to build – if you ever have any doubts in which direction your project will evolve, or if you are afraid that some MVC feature would be missing (or in fact, if you already ran into a missing feature), calling  `AddMvc()` would be the safest bet to resolve any of those worries or issues.
  
#### AddMvcCore()
`AddMvcCore()` registers all the core services required for the MVC application to work at all. We do not need to list them all, but pretty much everything related to the controller invocation pipeline gets activated there. These are low(er) level services, that only get customized when you are doing something quite complex or unusual (i.e. building a CMS). Some examples of them are: the controller activation services, the MVC options pipeline, application model provider infrastructure, action constraints, filter pipeline, model binder infrastructure, action result executors and a few more.

At the same time, the initialized framework configuration is completely “bare bones”. It is functional from the perspective of being able to handle an incoming HTTP call, but it is missing several core features. For example, the model validation via data annotations is no activated, same with authorization.

In this set-up, you are in control of (or, if you will, you are responsible for) what is plugged in and used at runtime. In other words, if you need anything beyond the most basic framework feature, you have to add it manually. In fact, in .NET Core 2.x and earlier, not even JSON support was there; this has now changed and the `System.Text.Json` formatter is actually already included in the call to `AddMvcCore()`.

**_For example:_**

```
// pick what you need
services.AddMvcCore()
    .AddDataAnnotations() // for model validation
    .AddApiExplorer(); // for Swagger
```
This should be the default choice for you if you really like to bootstrap the minimal amount of things at runtime and only activate the individual features you really use.
 
**_AddMVC vs AddMVCCore_** -> https://dzone.com/articles/the-difference-between-addmvc-and-addmvccore

#### AddDateAnnotations

MVC uygulamasında veri tabanı tablolarını Code First yöntemi ile oluşturmaya başladığımızda yapılan validasyon işlemlerine Data Annotations denir.

**_MVC Data Annotions’lar Ve Açıklamarıyla Bol Örnekler_** -> https://www.muratoner.net/aspnet/aspnet-mvc/mvc-data-annotionslar-ve-aciklamariyla-bol-ornekler

### 3) Shanpshot nedir? nasıl değişir? neden alınır?
+ https://softdevpractice.com/blog/entity-framework-core-snapshot/
+ https://www.learnentityframeworkcore.com/migrations/model-snapshot#:~:text=Entity%20Framework%20Core.-,The%20Model%20Snapshot%20In%20Entity%20Framework%20Core,updated%20with%20each%20subsequent%20migration

### 4) Jquery Calender
+ https://docs.microsoft.com/tr-tr/aspnet/mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4

+ https://www.c-sharpcorner.com/UploadFile/0c1bb2/calendar-control-using-jquery-ui-in-Asp-Net-mvc-5/


### 5)First- FirstOrDefault ve Single- SingleOrDefault nedir? Aralarındaki farkı araştırınız.

Tek rakamları içeren int tipinde bir dizimiz olduğunu varsayalım. Bu diziden herhangi bir teksayıyı seçmek istediğimizde hangi durumda hangi sorguyu kullanmalıyız?
```
int[] oddNumbers = { 1, 3, 5, 7, 9 };
```
**SingleOrDefault:** Eğer dizi içinden sadece bir tane sayı seçmek istiyorsak ve seçim şartımız sağlanmıyorsa, bu durumda int tipinin varsayılan değeri olan 0(sıfır) döndürülsün istiyorsak **SingleOrDefault** seçimini kullanmalıyız.

```
int[] oddNumbers = { 1, 3, 5, 7, 9 };
 
int number = oddNumbers.SingleOrDefault(n => n.Equals(4));
 
Console.WriteLine(number);
```
Yukarıdaki kod parçası hata fırlatılacaktır. Çünkü 1’den büyük olan, birden fazla eleman vardır.

**Single:** Eğer seçimimiz sonucunda sadece bir tane eleman geleceği garanti ise, bu durumda **Single** seçimini kullanabiliriz. Eğer şartımızı sağlayan hiçbir eleman dönmezse veya şartımızı sağlayan birden fazla eleman dönerse, bu iki durumda da istisnalar fırlatılacak ve hata ile karşılaşmış olacağız.

```
int[] oddNumbers = { 1, 3, 5, 7, 9 };
 
int number = oddNumbers.Single(n => n.Equals(3));
 
Console.WriteLine(number);
```
Yukarıdaki örnekte şartımız sağlandığı için 3 sonucu çıktı olarak gösterilecektir.

```
int[] oddNumbers = { 1, 3, 5, 7, 9 };
 
int number = oddNumbers.Single(n => n.Equals(2));
 
Console.WriteLine(number);

```
Yukarıdaki örnekte ise 2’ye eşit olan herhangi bir eleman olmadığı için **InvalidOperationException** istisnası fırlatılacaktır.

**FirstOrDefault:** Bu seçimde de mantık **SingleOrDefault** ile aynıdır. Ancak bu seçimde istenen şartta ilk eleman seçilir. Örneğin dizinin ilk elemanı, dizinin 2’den büyük ilk elemanı gibi.

**First:** Mantık **Single** ile aynıdır, ancak seçilen ilk elemandır.

```
int[] oddNumbers = { 1, 3, 5, 7, 9 };
 
int number = 0;
 
number = oddNumbers.FirstOrDefault(n => n > 9);  // Sonuç: 0
number = oddNumbers.FirstOrDefault(n => n == 3); // Sonuç: 3
number = oddNumbers.First(n => n == 5);          // Sonuç: 5
number = oddNumbers.First(n => n > 2);           // Sonuç: 3
number = oddNumbers.First(n => n > 9);           // Sonuç: HATA
 
Console.WriteLine(number);
```
### 6) En kısa null check nasıl yapılır?
#### ?? Operatörü
Bu operatör ile kısa ve net bir gramerle null değerleri kontrol edebiliyor ve müdahale edebiliyoruz. Şimdi aşağıdaki kod bloğunu inceleyiniz.

```
static void Islem(int? Sayi)
{
    Sayi = Sayi ?? -1;
    Console.WriteLine(Sayi.Value);
}
```
Gördüğünüz gibi Sayi parametresine null değer girilirse ?? operatörü ile kontrol edilmektedir. Okunuşu şöyledir. Eğer Sayı değeri null ise -1 değerini ata. İşlemimiz bu kadar kolay…

Tabi genellikle Asp.NET MVC mimarisinde kullandığımız bu operatörü birde o mimaride inceleyelim.
```
public ContentResult Topla(int? Sayi1, int? Sayi2)
{
    Sayi1 = Sayi1 ?? 0;
    Sayi2 = Sayi2 ?? 0;
    return Content((Sayi1 + Sayi2).ToString());
}
```
### 7) Partial view nedir?

PartialView parçalı görünüm anlamına gelmektedir. Yani partial view ile belli başlı alanlarımızı ayırabiliriz. Örneğin menü için bir partial view oluşturursak bunu layout içinde çağırabiliriz. Böylece menü ile ilgili tüm işlemler o partial view içinde bulunur. Bazı durumlarda layout içindeki bölgelerimizi her sayfada göstermek ihtiyacı duyabiliriz. Örneğin bir blog sitesi düşünürsek, bu sitede sağda veya solda bulunan kısımlar genellikle sabit kalmaktadır ve bu kısımdaki birçok şey veritabanından gelmektedir. Mesela kategori listesi gibi ya da son eklenen yorumlar, son eklenen yazılar gibi…

Layout oluşturduğumuzda bunun bir controller tarafı olmayacağı için Layout içinde belirlediğimiz bir tanımlama ile ilgili Controller’ın ilgili Action’ına git bu listeyi veya değerleri bize getir şeklinde belirtebiliriz. 

### 8) Farklı authentication bulup,aynı işleri farklı yollar ile yapanları araştırınız.

+ https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers
+ https://github.com/temilaj/ASP.NET-core-role-based-authentication
+ https://github.com/mukeshkumartech/CookieAuthenticationInAsp.NetCore


### 9) Razor Pages/MVC Projects karşılaştırmasını yapınız.

#### Razor Pages vs MVC
ASP.Net Core Razor Page is very similar to ASP.NET MVC's view pages. It has a similarity in syntax and functionality as MVC. The major difference is that in an MVC application model and controller code is included or bonded with View pages.

Razor pages are the next evolution of traditional ASP.Net WebForms. MVC is good for those web application which has lots of dynamic server pages, REST APIs and AJAX call.
For basic web pages and simple user input forms, we should go for Razor pages.

Razor pages are similar to ASP.Net web forms in the context of structure, except page extension.
In ASP.Net web forms, we had .aspx for UI and .aspx.cs file for code behind, in ASP.Net Razor pages we have .cshtml for UI and .cshtml.cs for code behind.

So, we can say that Razor pages are more organized compared to MVC. In MVC lots of routing get involved with Model, Controller, and Actions within Controller. But this is not the case with Razor pages.

We have a Razor view and code-behind file for this view to manage all actions and events on the View page.

#### Structure of Razor Pages and MVC

Razor pages do not have any kind of structure like MVC.  All Razor pages reside under the Pages folder with a very basic structure. You may see the below screenshot. Further, you can organize your project structure based on your functionality.

The below screenshot will explain to you about differences between the project structure of .Net Core Razor pages and MVC.

Understand Razor pages vs MVC structure from below image.


![del](https://user-images.githubusercontent.com/61011022/86684982-b873ee00-c00b-11ea-9bdc-de7c4761efc9.png)



#### Code Comparison

As I have already mentioned above that Razor pages and MVC look almost similar i.e. both having .cshtml as the file extension.

If you will notice the 2 pages, one from Razor and another from MVC then you will notice @page at the top of Razor pages.

@page is a directive used in Razor pages.
Another difference is the model declaration in Razor pages.

In Razor pages model declaration is like

For index.cshtml - @model IndexModel
For about.cshtml - @model AboutModel

#### Adding a new Razor Page in application

Right-click on Pages folder in Solution Explorer and Select Add->Razor Pages

Select Razor Page from below screen and click Add

![del1](https://user-images.githubusercontent.com/61011022/86685161-e6f1c900-c00b-11ea-8bab-2ec91f150179.PNG)

Next, Give a name to the view and select the required options and click Add.

![del2](https://user-images.githubusercontent.com/61011022/86685242-f96c0280-c00b-11ea-8f84-7bac6d5a17fd.PNG)


Default Code-behind for index.cshtml

```
public class IndexModel : PageModel
    {
        public void OnGet()
        {

        }
    }

```
There is no code-behind in MVC. In MVC, the developer has to use the controller file to create an Action method to link a particular View.

#### Page Routing

In Razor pages, default routing will find a Razor page from the Pages folder for a specific request. Means routing configuration will try to find the same name in the Pages folder for which a request was generated.

So, if a user requests/home then routing will try to find home in the pages folder.

But this is not the case in MVC.
In MVC default routing configuration will try to find a matching controller and action to load a view.

#### Advantages of using Razor Pages

There are few advantages of using Razor pages over MVC. Razor pages are easy to understand in the context of structure. Razor pages do not have a controller and action to load.
Razor pages contain code behind for individual pages.

#### Summary:

It is too early to analyse Razor pages as they are new. Razor pages are more structured and easy to understand compare to MVC. Razor pages are similar to traditional ASPX pages as Razor pages contain code-behind files too. Razor pages have @page directive at the top of the page.

There could be lots of debate for deciding which one is better.
But, if you want to create a website with static pages or with a few dynamic pages, I think the Razor page is a good choice.

But If you are creating a REST-based API project or lots and lots of dynamic pages, you should go for MVC.





### Kaynakça
+ https://www.strathweb.com/2020/02/asp-net-core-mvc-3-x-addmvc-addmvccore-addcontrollers-and-other-bootstrapping-approaches/
+ https://www.mshowto.org/mvc-data-annotations-nedir-nasil-kullanilir-bolum-35.html
+ https://www.tayfundeger.com/snapshot-nedir.html
+ https://www.bayramucuncu.com/single-singleordefault-ve-first-firstordefault-farki/
+ https://www.gencayyildiz.com/blog/cda-nullable-tipi-ve-operatoru/
+ http://blog.alicancevik.com/mvc-layout-icinde-partialview-yukleme/
+ https://wakeupandcode.com/authentication-authorization-in-asp-net-core-3-1/
+ https://docs.microsoft.com/en-us/aspnet/core/security/authentication/samples?view=aspnetcore-3.1
+ https://www.sharepointcafe.net/2019/10/razor-vs-mvc-in-dot-net-core.html
+ https://stackify.com/asp-net-razor-pages-vs-mvc/#:~:text=A%20Razor%20Page%20is%20very,%2DView%2DViewModel)%20framework
