---
title: "ClickHouse"
tags: ["OLAP"]
categories: ["Develop"]
date: 2025-04-21
draft: false
---
Java 初學者

## void function 沒有 return 的時候的對回傳值的定義
```
public class ch07_1 {

	public static void main(String[] args)
	{
		star();    //呼叫star函數
		System.out.println("使用函數印星星");
		star();    //呼叫star函數

	}

	public static void star()  //star函數
	{
		for(int i=0; i<20; i++)
			System.out.print("*");
		System.out.print("\n");
	}
    
public class ch07_2 {

	public static void main(String[] args)
	{
		int i;
		i=star(9);
		System.out.println(i+"個星星，使用函數印出");
	}

	public static int star(int n)  //注意資料型態
	{
		for(int i=0; i<2*n; i++)
			System.out.print("*");
		System.out.print("\n");
		return 2*n;
	}
```

## 函數多載
多載 （overloading）
1. 引數形態不同，引數個數相同
因此當在主程式呼叫函數時，將會執行相對應之型態的函數；show(a) ; a 為整數型態 int ，因此將會對應到引數為整數型態的函數.
```
public class ch07_9 {

	public static void main(String[] args) {
		int a=2, b[]={1,2,3};
		float c=3;
		double d=3.14;
		show(a);
		show(b);
		show(c);
		show(d);

	}
	public static void show(int i){
		System.out.println("值(a)＝"+i);
	}
	public static void show(int arr[]){
		System.out.print("值(b)=");
		for(int i=0;i<arr.length;i++)
		System.out.print(arr[i]+" ");
	}
	public static void show(float i){
		System.out.println("\n值(c)＝"+i);
	}
	public static void show(double i){
		System.out.println("值(d)＝"+i);
	}

}
```
2. 函數名稱相同，引數個數不同
透過引數個數的不同，對應到應該呼叫的函數
```
public class ch07_9_2 {

	public static void main(String[] args) {
		show();
		show(3);
		show('A',5);

	}
	public static void show(){
		System.out.println("no");
	}
	public static void show(int n){
		for(int i=0;i<n;i++)
			System.out.print("*");
		System.out.println("");
	}
	public static void show(char ch,int n){
		for(int i=0;i<n;i++)
			System.out.print(ch);
	}

}
```
Another Example： （不同面向的）
https://java.4-x.tw/java-08/java-08-7
p.s. 不能引數的個數與型態相同，只有函數成員的回傳型態不同。
Another Example：相當於 python class 的 init
https://java.4-x.tw/java-09/java-09-2

## 使用類別

```
class CRectangle {  //定義矩形類別
	//資料成員
	int width;  //寬
	int height; //長
}
// 實體化 class 並定義 book
// 方法1
CRectangle book;
book= new CRectangle();
// 方法2
CRectangle book=new CRectangle();
```

## public 與 private 的分別
1. 若冠上 public 公有的成員，可以被其他程式自由的存取
2. 若冠上 private 私有的成員，除了 package 其他程式無法存取外，自身的程式也無法存取；運作的範圍只在該類別當中
3. 若省略公有與私有 ( public & private) 都不使用，那麼成員只會在同一 package 存取使用

## private 與 protected 的分別
private 子類別無法直接讀取，protected 可以
protected 的優點：
- 父類別與子類別皆能使用
- 除了父、子類別之外就無法存取
- 不像 private 無法直接繼承只子類別或需透過公開函數來繼承
- 利用繼承，父類別中的 protected 可直接繼承至子類別
-


## 類別變數
count 因爲被宣告爲類別變數，所以是所有物件大家共享，radius 爲實例變數，會依類別的設定不同而改變。
``` 
class CCircle
{
   private static int count=0;         //宣告為類別變數
   private static double pi=3.14;      //宣告為類別變數
   private double radius;

   public CCircle()        // 建構元 沒有引數
   {
      this(1.0);           // 呼叫一個引數的建構元 並給值
   }
   public CCircle(double r)   // 建構元 一個引數
   {
      radius=r;
      count ++;               // 呼叫到此建構元就加一
   }
   public void show()
   {
      System.out.println("area="+pi*radius*radius);
   }
   public void show_count()   // 用來顯示建立物件的個數
   {
      System.out.println(count+" 個物件被建立");
   }
}
public class instance_vs_class_02
{
   public static void main(String args[])
   {
      CCircle cir1=new CCircle();      //呼叫沒有引數的建構元
      cir1.show_count();     // 呼叫顯示個數的函數 Ans 1 個物件被建立
      CCircle cir2=new CCircle(2.0);   //呼叫一個引數的建構元
      CCircle cir3=new CCircle(4.3);   //呼叫一個引數的建構元
      //以下使用物件呼叫顯示個數的函數
      cir1.show_count(); //Ans 3 個物件被建立
      cir2.show_count(); //Ans 3 個物件被建立
      cir3.show_count(); //Ans 3 個物件被建立
   }
}
```

## 類別函數


## Summary
實例變數：獨立，存在於不同記憶體
實例函數：必須透過物件來呼叫
類別變數：物件共享，佔用在同一個記憶體
類別函數：可以直接呼叫 （類似 python @staticmethod）

變數或函數加上 static 就可以定義爲類別變數或類別函數

## 繼承 extends
https://java.4-x.tw/java-11/java-11-1
- private 成員，必續透過父類別的函數才可以做存取。
```
class 父類別
{
  ...
} 

class 子類別 extends 父類別
{
 ...
}
```
example:
https://java.4-x.tw/java-11/java-11-2
呼叫子類別中的建構元；但在程式執行的結果卻發現 父類別的建構元會被先執行後，才輪到子類別的建構元。
```
class CCircle        // 父類別 CCircle
{
   private static double pi=3.14;
   private double radius;

   public CCircle()     // CCircle建構元
   {
      System.out.println("CCircle() 被呼叫 ");
   }
   public void setRadius(double r)
   {
      radius=r;
      System.out.println("radius="+radius);
   }
   public void show()
   {
      System.out.println("area="+pi*radius*radius);
   }
}
class CCoin extends CCircle      // 繼承父類別CCircle 的子類別CCoin
{
   private int value;            // 子類別資料成員

   public CCoin()          // 子類別建構元
   {
      System.out.println("CCoin() 被呼叫 ");
   }
   public void setValue(int t)         // 子類別函數
   {
      value=t;
      System.out.println("value="+value);
   }
}
public class class02
{
   public static void main(String args[])
   {
      CCoin coin=new CCoin(); // 建立 coin 物件
      coin.setRadius(2.0);    // 呼叫父類別中的函數
      coin.show();         // 呼叫父類別中的函數
      coin.setValue(5);       // 呼叫子類別中的函數
   }
}
```
## 類別繼承中的建構元呼叫
https://java.4-x.tw/java-11/java-11-3


## Overriding
https://java.4-x.tw/java-11/java-overriding
多載 overloading：名稱相同，透過不同引數、型態，來呼叫相對應的函數。
改寫 overriding：於子類別中，定義與父類別均相同的函數（名稱相同，引數個數相同）。
覆寫父類別的 fucntion


## 終止繼承
final,不能繼承 class 或是不能改寫父類別的 function
example：
```
final class Car{
...
}

class Caaa{
  protected final int num=10;
  protected final void show()
  {
    System.out.println("Caaa_num="+num);
  }
}

```

## 抽象類別
abstract
- 抽象類別不能用來建立物件
- 抽象函數不定義其處理方式
```
abstract class CShape               // 定義抽象類別 CShape
{
   protected String color;
   public void setColor(String str)    // 一般的函數
   {
      color=str;
   }
   public abstract void show(); // 只定義函數名稱的 抽象函數
}

class CRectangle extends CShape    // 子類別 矩形 CRectangle
{
   protected int width,height;
   public CRectangle(int w,int h)
   {
      width=w;
      height=h;
   }
   public void show()      // 定義繼承而來的抽象函數 show() 的處理方式
   {
      System.out.print("color="+color+",  ");
      System.out.println("area="+width*height);
   }
}
```
p.s. 抽象函數 只能宣告為 公有public 或 保護protected，
因為子類別必須取用，所以不能宣告成 私有private！

## interface
- interface 中的資料成員必須設定初值
- interface 沒有一般函數，只有抽象函數

```
interface 介面名稱
{
  final 資料型態 成員名稱=常數;

  public abstract 傳回值資料型態 函數名稱(引數...);
  //抽象函數並無定義處理方式
}
```
因為介面只有抽象函數，abstract 的關鍵字可以省略；
而資料成員部分：因為此值不會被更改，final 也是可以省略的。
抽象函數的修飾子也只能使用公有 public (或不宣告)，
為了都能取用到，所以不能宣告 私有private或保護 protected 。

```
\\ 介面實作
class 類別名稱 implements 介面名稱
{
   ...
}
```


## 一般 class 的多重繼承
https://java.4-x.tw/java-13/java-13-3
只有 interface 的 class 可以，一般的 class 不行
```
class 類別名稱 implements 介面1名稱,介面2名稱,介面3名稱...
{
   ...
}
```

## interface class 的多重繼承
```
interface 子介面名稱 extends 父介面1名稱,父介面2名稱,...
{
  ...
}
```
