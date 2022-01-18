# C# Note

## 物件導向4大特性

### 1. 封裝

* 朕賜給你的，才是你的；朕不給，你不能搶

> * public : 做公益有愛心，大家都能用
>
> * protected : 家族企業，同個繼承鍊的 class 才可以用
>
> * private : 個人秘密，自己才能用
>

* 使用 private 來實現封裝性的概念！

> 優點 : 
>
>  * 可以避免資料汙染
>
>    [^資料汙染]: 資料在寫入、讀取、存儲、傳輸或處理過程中發生的錯誤，這些錯誤給原始數據帶來了意想不到的變化
>
>  * 清楚分辨 class 內的成員有誰可以用
>
>  * 追求高內聚低耦合的理念
>
>    + 內聚，指的是和其它程式碼「無」相關
>    + 耦合，指的是和其它程式碼「有」相關

### 2. 繼承

* 透過類別的繼承，減少重複出現的結構

> ***EX : 動物 --> 哺乳類 --> 貓***
>
> 貓會有動物 & 哺乳類的屬性，也會有貓本身自己的屬性
>
> 哺乳類會有動物的屬性， **但** 不會有貓的屬性

### 3. 多型

* 當 **子類別** 的物件宣告或轉型成 **父類別** 或 **祖先類別** 的型別時，還可以正確執行該子類別的行為。

* ​		How to implement ?

> + 1. 多載 (overload)
>
>      ```c#
>      public int computeArea (int length){
>          return length * length;
>      } 
>                
>      public int computeArea (int length, int width){
>      	return length * width;
>      }
>      ```
>      
>      多載是指在相同 class 中，定義名稱相同，但是參數個數不同，或是參數型態不同的函式，這樣就可以利用參數個數或者參數型態，呼叫到對應的方法。例如：一個計算面積的方法，如果傳入一個參數，就當正方形來算面積；傳入兩個參數，就當成長方形來算面積。
>      
>      
>
> * 2. 覆寫 (override)
>
>      ```c#
>      class Animal{
>          protected int legs = 1;
>          protected virtual int getLegs(){
>            return legs*4;
>          }
>      }
>      class Bird extends Animal{
>          protected override int getLegs(){
>            return legs*2;
>          }
>      }
>      ```
>
>      是指覆寫掉父類別中的函式。例如：動物類別(父類別) getLegs()方法被鳥類別(子類別)覆蓋

### 4. 抽象

* 當實體化沒有任何意義的父類別，就可以考慮改成抽象類別。

> EX : 
>
> ```c#
> //產生一個車子的類別，定義了啟動以及消耗能源
>         public abstract class 車子
>         {
>             public abstract string 消耗能源();
>             public abstract string 啟動();
>         }
>         //產生一個特斯拉類別繼承車子
>         public class 特斯拉 : 車子
>         {
>             public override string 啟動()
>             {
>                 return "特斯拉啟動";
>             }
>             public override string 消耗能源()
>             {
>                 return "消耗電能";
>             }
>         }
>         //產生一個汽車類別繼承車子
>         public class 汽車 : 車子
>         {
>             public override string 啟動()
>             {
>                 return "汽車啟動";
>             }
>             public override string 消耗能源()
>             {
>                 return "消耗汽油";
>             }
>         }
> 		
> 	private static void Main(string[] args)
>         {
>             汽車 car = new 汽車();
>             Console.WriteLine(car.啟動());
>             Console.WriteLine("汽車:" + car.消耗能源());
>             特斯拉 Tesla = new 特斯拉();
>             Console.WriteLine(Tesla.啟動());
>             Console.WriteLine("特斯拉:" + Tesla.消耗能源());
>         }
> ```
>
> 抽象 class 內的 method 不會實作，會等到子類別再進行實作
>
> 如果類別中包含抽象方法，那麼整個類別就必須定義為抽象類別，不論是否還包含其他一般方法

## Using

### 1. 指示詞

* ​	建立 namespace 的 Alias

* ​    import 其他 namespace 的定義型別

* ​    在整份檔案的最上方

> 標準用法：
>
> ```c#
> using System.Text;
> ```
>
> 別名用法：
>
> ```c#
> using Models = MvcApplication.Models;
> ```

### 2. 陳述式

* 使用 using 最主要的目的是為了讓物件建立的同時能確保該物件所佔用的資源一定會被完整釋放，如果沒有釋放這些無法自動釋放的資源，就很有可能讓 .NET 應用程式發生 **資源耗盡 (Resource Exhausted)** 的狀況。

> ```c#
> using (System.IO.StreamReader sr = 
>          new System.IO.StreamReader(@"C:\\test.txt"))
> {
>     string s = null;
>     while((s = sr.ReadLine()) != null)
>     {
>         Console.WriteLine(s);
>     }
> }
> ```

* 使用 using 陳述式有一個最基本的條件，就是該物件必須有實做 IDisposable 介面，才能確保在 using 的結尾數時自動執行 **Dispose()** 方法。
* 在使用 using 陳述式時，雖然有 try 與 finally，但並 **不包含 catch** 的成分！ 換句話說，使用 using 陳述式並不會幫你捕捉例外狀況！！

## Namespace

* 讓一組名稱與其他名稱分隔開的方式。在一個命名空間中聲明的類的名稱與另一個命名空間中聲明的相同的類的名稱不衝突。

> 我們舉一個計算機系統中的例子，一個文件夾(目錄)中可以包含多個文件夾，每個文件夾中不能有相同的文件名，但不同文件夾中的文件可以重名。
>
> ![](https://www.runoob.com/wp-content/uploads/2019/09/0129A8E9-30FE-431D-8C48-399EA4841E9D.jpg)

* 在C#程式中都靠「.」來區別使用哪個命名空間的類別的方法， 「.」有「的」的意思
  **命名空間.類別.方法()**

## LINQ (Language-Integrated Query)

* LINQ 是一種數據查詢語言，可以讓我們使用同一個語句來對不同資料來做資料查詢 (包括Objects、SQL、Datasets、Entities、Data Source、XML/XSD等)，並且具有擴充性。
* 使用前要先載入 LINQ 的 namespace 

> ```c#
> using System.Linq;
> ```

> EX : 
>
> ```c#
> using System;
> using System.Linq;
> using System.Collections.Generic;
> 
> namespace ConsoleApplication2
> {
>     class Program
>     {
>         public class Student
>         {
>             public string Name { get; set; }
>             public int Sexual { get; set; }
>             public int ID { get; set; }
>         }
> 
>         public static List<Student> GetStudents()
>         {
>             List<Student> students = new List<Student>
>             {
>                new Student {Name="Adam", Sexual=1, ID=1055010001},
>                new Student {Name="Steven", Sexual=2, ID=1055010002},
>                new Student {Name="Brown", Sexual=1, ID=1055010003},
>                new Student {Name="Cindy", Sexual=2, ID=1055010004},
>                new Student {Name="Tom", Sexual=1, ID=1055010005}
>             };
> 
>             return students;
>         }
>         
>         static void Main(string[] args)
>         {
>             var getstudents = GetStudents();
>             var students = from st in getstudents select st;
>             
>             foreach (var i in students)
>             {
>                 string sex = i.Sexual == 1 ? "male" : "female";
>                 Console.WriteLine("Name:" + i.Name + ", Sexual:" + sex + ", ID:" + i.ID);
>             }
>         }
>     }
> }
> ```

* 基本語法

  * **From..Select** 搜尋資料

    ```csharp
    var students = from st in getstudents select st;
    ```

  * **where** 篩選資料

    ```csharp
    var students = from st in getstudents where st.Sexual==1 select st;
    ```

  * **orderby** 排序資料 可以搭配 **ascending (小至大, A to Z)**排序或**descending(大至小, Z to A)**排序

    ```c#
    var students = from st in getstudents orderby st.ID descending select st;
    ```

  * **group by** 群組 可以將資料依照群組分類，儲存成物件(包含 Key 及對應項目)

    ```csharp
    var getstudents = GetStudents();
    var students = from st in getstudents group st by st.Sexual ;
    
    foreach (var i in students)
    {
    	//第一層迴圈取出group物件，可以透過 .Key得到group值
        Console.WriteLine("Group物件的Key="+i.Key);
        foreach(var j in i)
        {
        	//第二層迴圈可以取出Group對應資料
            Console.WriteLine(j.Name);
        }
    }
    ```

    ```c#
    /*
    結果:
    Group物件的Key=1
    Adam
    Brown
    Tom
    Group物件的Key=2
    Steven
    Cindy
    */
    ```

  * 使用**into**將group暫存在一個變數， 並且對群組執行額外的邏輯， 再透過select結束查詢語句 當然，可以同時加入 **where**

    ```c#
    var students = from st in getstudents where st.Name!="Tom" group st by st.Sexual into x where x.Key==1 select x;
    ```

    

## 同步與非同步

* 同步 : 必須從頭到尾等待直到完成為止
* 非同步 : 一個任務可以處於尚未完成的狀態，之後再接續下去繼續完成

> EX : 家庭主婦
>
> - 如果你是個同步的家庭主婦，洗衣服時，你必須等到衣服洗完才能繼續做其他事，不能去打電動看youtube。
> - 如果你是個非同步的家庭主婦，你可以啟動洗衣機之後，先去做其他事，之後等到洗衣機脫水完逼逼叫，再接續下去處理其他任務。

* 執行緒 (Thread)
  * 單執行緒
    - 從頭到尾只有一個 thread 可以執行程式
  * 多執行緒
    - 有兩個以上的 thread 能夠執行程式

* #### **非同步和多執行緒無關**

  * Threading is about `workers`; Asynchrony is about `tasks`.
  * **多執行緒(Multithreading)**:著重在`使用多個工作者(Thread)`，它可以同時進行多個工作，但實際上的運作方式是並行還是平行，就看您CPU的能耐如何了
  * **非同步(Asynchronous)**:著重在`有效的使用工作者(Thread)`，它可以讓有限的Threads盡可能的完成更多的工作

* 多執行緒不一定比較有效率；非同步不會做得比較快，但是非同步讓你 **不用耗在等待回應** 上，而是可以利用等待時間去處理其他工作，效能也就跟著提升了。

* #### **任務，是非同步的抽象**

  * 這就是非同步的本質，對「任務」這件事進行抽象，讓我們可以表達一件事情是否做到一半、是否完成、接下來要繼續做什麼。
  * 在C#裡，我們用 Task 類別表達非同步這件事。

* #### **C# Task**

  * 不僅乘載了非同步的概念，另外還有許多關於執行的方法

* **async 是實作細節**
  * 要在這個方法內進行一個非同步的任務
  * 而且我需要在這個方法內等待某個非同步任務完成後，繼續進行之後的任務
  * 有 async 就一定有 await
    * Compiler 才知道非同步的斷點在哪裡

## 委派 (Deligate)

* delegate是一個類別，在實體化委派事件時，可以自行決定這個委派事件要做哪些方法

* 簡單來說 delegate 是將 function 當成參數傳遞的型別。

* 注意事項 : 

  * 委派的事件必須要與 **定義的方法回傳型態** 與 **傳入的參數類型** 一致才能使用

    > EX : 
    >
    > ![](https://miro.medium.com/max/1370/1*QrsrRMA6e8KZ053FIzh6vg.png)
    >
    > 

## 參考來源

> 1. https://savory-okapi-734.notion.site/C-2166556e3be24ea78bd05a2d2fe1e51c
> 2. https://ithelp.ithome.com.tw/articles/10191485
> 3. https://sunnyday0932.github.io/2020/object-oriented%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91-4_%E6%8A%BD%E8%B1%A1%E9%A1%9E%E5%88%A5abstract%E8%88%87%E4%BB%8B%E9%9D%A2interface/
> 4. https://blog.miniasp.com/post/2009/10/12/About-CSharp-using-Statement-misunderstanding-on-try-catch-finally
> 5. https://ithelp.ithome.com.tw/articles/10223785
> 6. https://hoohoo.top/blog/c-linq-teaching-notes-using-visual-studio/
> 6. https://blog.opasschang.com/understand-csharp-asyn/
> 6. https://medium.com/@WilliamWhetstone/c-%E4%BD%95%E8%AC%82%E5%A7%94%E6%B4%BE-delegate-e7eec68da4e2
