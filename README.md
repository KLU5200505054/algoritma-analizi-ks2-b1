# Algoritma Analizi ve Tasarımı

## Zaman Karmaşıklığı Analizi

Aşağıda, kaynak kodlar parçalar halinde analiz edilmiştir : 

`#define A_SIZE 20
#define G_SIZE 10
#define MAX_W 10

//Max deger icin sonsuz sayi
#define INF INT_MAX

void generate(int a[], int size);
void function1(int a[], int size);
int function2(int a[], int size);
void function3(int g[][G_SIZE], int d[][G_SIZE], int size);
void print1(int a[], int size);
void print2(int g[][G_SIZE], int size);
void print3(int g[][G_SIZE], int size, int t);

`
Değişken, liste vb tanımlamalar ve define terimleri O(1) karmaşıklığına sahiptir. Bu durumda toplamı yine O(1) olarak düşünülebilir.

`void generate(int a[], int size) {
    for (int i = 0; i < size; i++) {
        a[i] = rand() % (2 * MAX_W) - MAX_W;
    }
}
`

Yukarıda ise generate fonksiyonu bir dizi ve bu dizinin boyutunu parametre alır. Burada fonksiyonun zaman karmaşıklığı ise, dizinin boyutu kadar çalışacak olan döngüden dolayı O(n) olacaktır. Döngü içinde yapılan rand işlemi ise sabittir ve bu yüzden toplam karmaşıklık O(n) diyebiliriz.

`void function1(int a[], int size) {
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - 1 - i; j++) {
            if (a[j] > a[j + 1]) {
                int tmp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = tmp;
            }
        }
    }
}`

Function 1, yine bir dizi ve boyutunu parametre alır. Ancak burara iç içe 2 döngü vardır.Döngüler n-1 ve n-1-i kez yineleniyor. Bu durumda toplam yineleme sayısı n^2-n*i + i^2 olacaktır. Burada sabitleri ve çarpımları görmezden gelirsek O(n^2) sonucuna ulaşacağız. İf koşuu ve içindeki durumlar yine sabit karmaşıklığa sahip olduğu için hesaplamaya dahil etmedim. Ayrıca yukarıdaki fonksiyonun da bubble sort algoritması olduğunu açıkça görebiliyoruz. Daha hızlı bir çözüm için merge veya quick sort kullanabiliriz.

`int function2(int a[], int size) {
    int t = 0, current_sum = 0, count=0;
    for (int i = 0; i < size; i++) {
        if (current_sum + a[i] > 0) {
            current_sum = current_sum + a[i];
        }
        else {current_sum = 0;}
        if (current_sum > t) {
            t = current_sum;
            count++; }}
    return t/count;}`

Burada ise toplam zaman karmaşıklığı yine O(n)'dir; burada n, dizinin boyutudur. Bunun nedeni, for döngüsünün dizi üzerinde n kez yinelenmesi ve her yinelemenin sabit süre almasıdır. T, current_sum ve count tanımlamarı O(1) karmaşıklığına sahiptir. Ayrıca içerideki if else durumları da her kontrol için 1 kere çalışacağından en kötü durumda iki if de çalışır ve toplamda O(5) olur ve bu da sabittir. Bu nedenle yine toplam karmaşıklığa O(n) diyebiliriz.

`void function3(int g[][G_SIZE], int d[][G_SIZE], int size) {
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            if (i == j) {
                d[i][j] = 0; }
            else if (g[i][j] != 0) {
                d[i][j] = g[i][j]; }
            else {
                d[i][j] = INF; }}}
    for (int k = 0; k < size; k++) {
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                if (d[i][k] != INF && d[k][j] != INF &&
                    d[i][k] + d[k][j] < d[i][j]) {
                    d[i][j] = d[i][k] + d[k][j];
                }}}}}`

Bu fonksiyon graf algoritmalarından floyd warshal algoritmasına aittir. Ağırlıklı graflarda kullanılır. İki düğüm arasındaki en kısa yolu bulur. G matrisi grafı,, d matrisi iseen kısa yol matrisidir.
İlk döngüye bakalım. 
i ve j aynı olduğunda, d[i][j] 0 olarak ayarlanır çünkü bir düğümün kendisiyle olan mesafesi 0'dır. g[i][j] değeri 0'dan farklı ise, d[i][j] değeri g[i][j] olarak ayarlanır. Bu durumda, grafın i'den j'ye olan doğrudan bir kenarı olduğu anlamına gelir. Bu iki durum sağlanmazsa, d[i][j] değeri sonsuz (INF) olarak ayarlanır. Bu durumda, i'den j'ye bir yolun bulunmadığı anlamına gelir.
İkinci döngüyü incelersek,
Bu döngüde, tüm düğümler (k) üzerinde dolaşarak i ve j indisli elemanları günceller. Her i ve j değeri için,

d[i][k] != INF ve d[k][j] != INF ise, i'den k'ya ve k'dan j'ye giden yolların var olduğu anlamına gelir.
d[i][k] + d[k][j] < d[i][j] ise, i'den j'ye olan mevcut yolun, i'den k'ya ve k'dan j'ye giderek daha kısa bir yol olduğu anlamına gelir. Bu durumda, d[i][j] değeri güncellenir.

Kodlarda da görüldüğü gibi function3 içinde içiçe olmayan 2 for döngüsü bulunmaktadır. Bu durumda karmaşıklık hesaplarken bu iki döngünün karmaşıklıklarını toplamamız gerekir. 

Döngüleri tek tek hesaplayalım olursak, ilk döngü içinde bir döngü daha bulundurur. Bu durumda ikisinin de karmaşıklıklarını çarparız. İki döngü de size değeri kadar tekrar edeceği için n kadar tekrar gerçekleşir. İkinci döngünün içindeki if else durumları en kötü zamanda bile sabit zaman karmaşıklığına sahip olacaktır. Öyleyse ilk döngünün toplam zaman karmaşıklığı O(n^2) diyebilriz. 

İkinci döngüde ise yine n kadar tekrar eden 3 döngü bulunmaktadır. Bu durumda  yine en içerideki döngüdeki if koşuluna bakacak olursak, içerideki koşul sağlandığında bile sadece kontroller geçrkeleştirdiği ve atama işlemi yaptığı için zaman karmaşıklığı sabittir. Bu durumda yalnızca döngülerin karmaşıklığına bakacak olursak O(n^3) diyebiliriz. Methodun tamamını incelersek n^2 ve n^3 karmaşıklığına sahip 2 döngü gördük.
E o zaman bunun da karmaşıklığına O(n^2 + n^3) diyeibliriz. N^2, n^3 yanında görmezden gelinebilir olduğu için toplam zaman karmaşıklığı : O(N^3) diyebiliriz.

`void print1(int a[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", a[i]);
        if ((i + 1) % 10 == 0) {
            printf("\n");
        }
    }
}`

Bu fonksiyon, bir diziyi ve boyutunu parametre alır. Ayrıca her satırda 10 eleman oalcak şekilde kontrol de yapılıyor. Zaman karmaşıklığına bakacak olursak size kadar dönen bir döngü bulunmaktadır. Ardışık artan döngülerin O(n) karmaşıklığında olduğunu biliyoruz. İçerideki print, if koşulu ve içindeki print ifadesi de sabit karmaşıklığa sahip olduğuna göre bu fonksiyonun toplam zaman karmaşıklığı O(n) diyebiliriz. Tam olarak ne iş yaptığını zaman karmaşıklığı açıklamalarının altında inceleyeceğiz.

`void print2(int g[][G_SIZE], int size) {
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            if (g[i][j] == INF) {
                printf("INF ");
            }
            else {
                printf("%3d ", g[i][j]);
            }
        }
        printf("\n"); } }`

Print2 fonksiyonu ise iki boyutlu bir matris ve bu matrisin size’ını parametre alır. Ardında iç içe size kadar tekrar eden bir döngü ile bunları yazdırır. Eğer eleman INF ise “INF” yazdırır. Değilse elemanı 3 karakter alacak şekilde yazdırır. Bu durumda if kontrolleri sabit oalcağı için iç içe birer artan döngünün karmaşıklığı n*n = n^2 olduğu için bu fonksiyonun zaman karmaşıkığına da O(n^2) diyebiliriz.

`void print3(int d[][G_SIZE], int size, int t) {
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            if (i != j && d[i][j] < t) {
                printf("%c -> %c: %d\n", 'A' + i, 'A' + j, d[i][j]);
            }
        }
    }
}`

Yukarıdaki print3 fonksiyonu ise print2den farklı oalrak t parametresi alır ve bu t değerini bir eşik değeri oalrak kullanır. Eğer i != j && d[i][j] < t koşulu sağlanırsa print yapılır
Bu duurmda da yine kontroller sabit kalacağı için iç içe birer artan döngülerin karmaşıklığından toplam karmaşıklık O(N^2) oalcaktır.




# Kodların Genel Açıklaması ve Toplam Zaman Karmaşıklığı


Kodlara göre bir analiz gerçekleştirecek olursak, bu kodların bir graf uygulamasına ait olduğunu anlayabiliriz. Çünkü function 3’ün floyd warshal olduğunu yukarıda da söylemiştim. 

Bu durumda g matrisi hakkında şunları diyebirliz: 
G matrisi, ağırlıklı grafın komşuluk matrisini temsil eder. Her bir g[i][j] değeri, düğüm i'den düğüm j'ye olan ağırlığı ifade eder. Eğer i ve j düğümleri arasında bir kenar yoksa, bu değer 0 olarak ayarlanır.
Main içinde en başta g matrisini statik olarak tanımlıyoruz.
Sonrasında ise bir timer başlatıyoruz.
Ardından generate fonksiyonu ile, önceden oluşturduğumuz a matrisi içine A_SIZE değeri kadar eleman ekliyoruz. Bu elemanlar rastgele oluşturuluyor.
Sonrasında ise function1 ile bu matrisi sıralıyoruz. Function1’in bubble sort olduğunu da yukarıdaki kodlarda söylemiştim. 
Ardından function 2 ile a dizisindeki alt dizi toplamlarının en büyük ortalaması hesaplanır ve sonuç ekrana yazdırılır. 
Kodları çalıştırdığımızda da görüyoruz ki  20 adet değer eklenmiş ve bunlar yazdırılırken 10arlı satırlar şeklinde yazdırılmış. Ardından function1 de sıralanmış ve function2de ise pozitif değerlerin ortalaması verilmiş.

A:
3 5 0 8 -3 5 -2 8 2 -10 
1 7 -10 -5 -1 2 -6 3 -6 5 
Function1 Sonuc:
-10 -10 -6 -6 -5 -3 -2 -1 0 1 
2 2 3 3 5 5 5 7 8 8 
Function2 Sonuc: 4

Buradan sonra önceden zaten oluşturduğumuz g matrisi yazdırılır. 
Function3 methodu ise g matrisinden yola çıkarak d matrisini hesaplar. İlk başta d, gnin kopyası olarak başlar. d matrisindeki her bir d[i][j] değeri, i’nci düğümden j’nci düğüme olan en kısa yolun ağırlığını belirtir. 
Burada yaptığımız şey aslında floyd warshall için bir hazırlık gibidir. Ya da böyle düşünebiliriz.
Ardınadn, floyd warshall algoritması uygulanır. Algoritma, her bir düğüm çifti (i, j) için, herhangi bir ara düğüm k üzerinden geçerek i'den j'ye olan en kısa yolun ağırlığını kontrol eder. Eğer bu yol daha kısa bir yol bulunursa, d[i][j] değeri güncellenir. Bunu yapınca ekrana function3 sonuç adında d matrisi yazdırılır. Print2 kullanılır.

Ardından print3 ile t değeri için sonuçları yazdırıyoruz.  Burada dikkat edilmesi gereken nokta, düğümler arasındaki ağırlıkların t değerinden küçük olanların yazdırılıyor olmasıdır. Sonuçlar dediğim şey de d matrisine göre t eşiğinden küçük ağırlıklar diyebiliriz.

Sonrasında timer kapatılır, çalışma süresi yazdırılır. 


# Toplam zaman karmaşıklığı

Tüm foksiyonların çalışma zamanlarını yukarıda hesaplamıştık. Bu durumda;
`generate(a, A_SIZE);
printf("A:\n");
print1(a, A_SIZE);`

Burada zaman karmaşıklığı hem generate hem de print1 ile O(n) gelir. Print ise O(1) dir. Bu durumda toplam O(2n+1) olur.

`function1(a, A_SIZE);
printf("\nFunction1 Sonuc:\n");
print1(a, A_SIZE);`

Burada function1 n^2 karmaşıklığında çalışır. Print1 fonksiyonu O(n), print komutu ise O(1) karmaşıklığına sahip olduğu için toplam karmaşıklık da O(n^2 + n + 1) oalcaktır. 

`t = function2(a, A_SIZE);
printf("\nFunction2 Sonuc: %d\n", t);`

Function2'nin karmaşıklığı O(n) olarak belirlenmişti. Bu durumda bu satırın karmaşıklığı O(n) olacaktır. Print ise sabit olduğu için toplam karmaşıklık O(n+1) oalcaktır.

`printf("\nG:\n");
print2(g, G_SIZE);`

Print2 fonksiyonu O(n^2), printf komutu ise sabit zaman karmaşıklığına sahip olduğundan, bu satırın karmaşıklığı O(n^2+1) olacaktır.

`function3(g, d, G_SIZE);
printf("\nFunction3 Sonuc:\n");
print2(d, G_SIZE);
`
Function3 fonksiyonunun karmaşıklığı O(n^3) olarak belirlenmişti. Print2 fonksiyonu O(n^2) ve printf komutu da sabit zaman karmaşıklığına sahip olduğundan, bu satırların toplam karmaşıklığı 
O(n^3 + n^2 + 1) olacaktır.
`
printf("\n%d icin sonuc:\n", t);
print3(d, G_SIZE, t);`

Print3 fonksiyonu O(n^2) karmaşıklığına sahip olduğundan, bu satırların toplam karmaşıklığı O(n^2 + 1) olacaktır.

Ayrıca srand fonksiyonunun karmşıklığı O(N) , diğer tanımlama ve kontroller ise sabittir.
Bu durumda toplam karmaşıklığı inceleyecek olursak : 

O(2n+1)   +   O(n^2 + n + 1)   +   O(n+1)   +   O(n^2+1)   +   O(n^3 + n^2 + 1)   +   O(n^2 + 1)    +   O(N)

Toplam karmaşıklık :  O(n^3 + 4n^2 + 5n + 6c) denebilir. Yani bunu da  O(N^3)  oalrak ifade edeiblriz.

# Kodlar hakkında genel yorum : 

Bu kodlar, verilen bir grafta rastgele belirlenen bir dizi üzerinden oluşturulmuş eşik değerine göre, düğümler arasındaki mesafeleri en kısa yolu hesaplar. 
Ve bu işlemleri gerçekleştirirken de geçen zamanı hesaplar.

# Tavsiyeler:

Verilen uygulamanın daha hızlı çalışması için öncelikle bubble sort yerine quick veya merge sort kullanılabilir. 
Bu durumda toplam zaman karmaşıklığı değişmeyecektir. Ancak çalışma süresi yine de azalacaktır.

Çünkü floyd warshall algoritması n^3 karmaşıklığına sahiptir. Bu durumda ne olursa olsun uygulama n^3 karmaşıklığında çalışacaktır. 

Eğer tüm düğümlerin birbirine olan uzaklığı yerine bir root’tan diğer düğümler arasında hesaplama işlemi yapmayı düşünürsek dijkstra kullanarak bu karmaşıkığı n^2 ye çekebiliriz. Bu durumda uygulamanın zaman karmaşıklığı hatrı sayılır seviyede azalacaktır.
