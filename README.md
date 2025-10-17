# GUI 2025/26
## GUI_PL_02_01 Przypomnienie podstaw OOP

- Zakres materiału tego działu to: Czym jest klasa, obiekt? Różnice między polami statycznymi i niestatycznymi. Konstruktory, metody, hermetyzacja, pakiety, dziedziczenie, przesłonięcie i przeciążenie metod, "this", "super", polimorfizm, equals i hashCode. Bloki inicjalizacyjne (statyczne i niestatyczne).
- Programowanie opiektowe ma odzwierciedlać rzeczywistość.

## GUI_PL_02.02 Pola
- Obiekty są zawsze tworzone na stercie (w wolnej pamięci) oraz anonimowe. 
- Na stosie są tworzone tylko referencje do obiektów a nie obiekty.
- Tworzymy obiekty za pomocą `new` -> to zwraca referencję do obiektu.
- Obiekt bez referencji zostanie sprzątnięty przez garbage collector ale nie wiadomo kiedy.
- Można wystosować prośbę do Garbage Collectora a flush pamięci ale nie daje to pewności, że zostanie ona odrazu zrealizowana.
- Garbage Collector jest częścią JVM
- Statyczne pola są inicjowane podczas ładowania klasy do pamięci (jeżeli to Enum to po utworzeniu wartości).

## GUI_PL_02.03 Konstruktory
- Konstruktor musi mieć nazwę taką samą jak klasa oraz nie deklaruje typu
- W klasie możemy zdefiniować wiele konstruktorów o tej samej nazwie ale z innymi sygnaturami. To się nazywa **PRZECIĄŻENIE KONSTRUKTORA (Overloading)**
- Podobnie można przeciążać metody
- Jak nie zdefiniujemy żadnego konstruktora to powstanie pusty konstruktor domyślny
- Domyślne wartości to:
	- dla `int` to `0`
	- dla referencji to `null`
	- dla `booleana` to `false`
- `this()` to wywołanie konstruktora

## GUI_PL_02.04 Metody
- *Przysłonięcie* to nadpisanie metody
- Domyślny `.toString()` zwraca `NazwaKlasy@hashReferencjiObiektu`
- Można dodawać adnotację `@Override`

## GUI_PL_02.05 Bloki inicjalizacyjne
- niestatyczne:
```java
{
	// Gdziekolwiek wewnątrz klasy ale poza metodami i polami
}
```
- statyczne:
```java
static {
	// tak samo jak wyżej
}
```
- Ciało niestatycznego bloku jest wywołane po utworzeniu obiektu i zainicjowaniu pól wartościami domyślnymi ale przed wywołaniem konstruktora.
- Ciało statycznego w momencie załadowania klasy po zainicjowaniu pól statycznych
- Czyli stack trace wygląda:
```
- Klasa i pola statyczne
- Blok statyczny
- Instancja obiektu
- Blok nie statyczny
- Konstruktor
```

## GUI_PL_02.06 Hermetyzacja
- `public`/`private`/`protected` to deklaratory dostępu
- jest jeszcze `default` czyli `package private` ale `default` nie jest słowem kluczowym - to po prostu brak żadnego
- W `protected` mamy dostęp z poziomu:
	- tego samego pakietu
	- klasy dziedziczącej po tej klasie niezależnie od pakietu

## GUI_PL_02.07 Pakiety
- W pakietach pełną nazwę piszemy małymi literami
- Do zassania klasy z innego pakietu używamy `import`

## GUI_PL_02.08 Dziedziczenie
- Można dziedziczyć tylko po jednej klasie

## GUI_PL_02.09 Przesłonięcie i przeciążenie
- *Przesłonięcie (Overwriting)* - Nadpisuje metodę / pole z klasy po której dziedziczę
- *Przeciążenie (Overloading)* - Deklaruję kilka metod z tą samą nazwą ale z różnymi sygnaturami

## GUI_PL_02.10 `this`, `super`
- `this` - referencja do obecnego obiektu. Wywołany odwoła się do konstruktora.
- `super` - referencja do klasy po której dziedziczę. Wywołany odwoła się do konstruktora klasy po której dziedziczę a po kropce udostępnia metody klasy po której dziedziczę.
- `super` - musi być jako pierwsza w konstruktorze

## GUI_PL_02.11 Polimorfizm
- **Polimorfizm** to zdolność obiektów róœnych klas do reagowania w różny sposób na to samo wywołanie metody.

```java
Person p1 = new Person();
Employe p2 = new Employe();

Person []tab = {p1, p2};

for(Person p: tab)
	p.sayHello(); // Tu się wykona NAJBARDZIEJ WYSPECJALIZOWANA implementacja sayHello() czyli ta z Employe
```
## GUI_PL_02.12 hashCode, equals
- `.hashCode()` zwraca hash _instancji_ obiektu.
- `o1.equals(Obj o2)` zwraca porównanie czy `o1` i `o2` to ta sama instancja
- w tym wykładzie chodzi o to, że można sobie nadpisać `.equals(o)` żeby porównywało po polach.

```java
@Override
public boolean equals(Object obj) {
	if(obj == this)
		return true;
	if(obj == null || this.getClass() != obj.getClass())
		return false;

	Person tmp = (Person) obj;
	boolean isEquals = true;

	if(tmp.name != null && this.name != null {
		// ...
	}
}
```

## GUI_PL_02.13 Rekurencja
- Do testowania performance:

```java
long sum = 0;
for (int i = 0; i<1000000; i++) {
	long start = System.nanoTime();
	// metoda którą mierzę
	long stop = System.nanoTime();
	sum += (stop - start);
}
```

- Duże liczby liczymy typem `BigDecimal`
- `BigDecimal.ONE` zwraca `1`
- `BigDecimal.multiply()` mnoży
- Iteracyjnie silnia i fibbonacci są szybsze niż rekurencyjnie
- Iteracyjny fibbonacci:
```java
public static long iter(int i) {
	long f0 = 0;
	long f1 = 1;
	long tmp = 0;
	
	for(long i = 2; i<n; i++) {
		tmp = f0 + f1;
		f0 = f1;
		f1 = tmp;
	}

	return f1;
}
```
- Rekurencja odkłada więcej rzeczy na stosie - więc może polecieć `StackOverflowException`
- Używamy rekurencji znając pesymistyczną wielkość danych

## GUI_PL_03.01 Wstęp do strumieni IO
- Strumienie dzielą się na:
	- **Bajtowe (binarne)**
		- podtypem ich mogą być obiektowe czyli serializowane obiekty
		- Rozszerzają `InputStream` lub `OutputStream`
	- **Tekstowe**
		- Rozszerzają `Reader` lub `Writer`
- Jedne i drugie trzeba zamykać
- Występuje buforowanie
- Przy zamknięciu bufor się opróżnia i zapisuje dane w pliku

## GUI_PL_03.02 Strumienie bajtowe IO
- `FileInputStream`:
```
FileInputStream {
	int read() throws IOException
	int read(byte[] b) throws IOException
	int read(byte[] b, int off, int len) throws IOException
	void close() throws IOException
}
```
- `FileOutputStream`:
```
FileOutputStream {
	void write(int b) throws IOException
	void write(byte[] b) throws IOException
	void write(byte[] b, int off, int len) throws IOException
	void close() throws IOException
	void flush() throws IOException
}
```
- Ścieżka do plików jest relatywna względem roota projektu

## GUI_PL_03.03 Serializacja
- `ReadObject`
```java
ObjectInputStream ois = null;

try {
	ois = new ObjectInputStream(new FileInputStream("./plik.txt"));
	String text = (String) ois.readObject();
	System.out.println(text);
} finally {
	if(ois != null)
		ois.close();
}
```
- `WriteObject`
```java
String s = new String("dupa");
ObjectOutputStream oos = null;

try {
	oos = new ObjectOutputStream(new FileOutputStream("./plik.txt"));
	oos.writeObject(s);
} finally {
	if(oos != null)
		oos.close();
}
```
- Żeby obiekt dał się serializować musi implementować interfejs `Serializable` - nie trzeba dodawać metod. Dzieje się automatycznie wszystko.

## GUI_PL_03.04 Strumienie znakowe
- `BufferedReader`:
```java
BufferedReader br = null;
try {
	br = new BufferedReader(new FileReader("./plik.txt"));
	String s;

	while((s = br.readLine()) != null) {
		System.out.println(s);
	}
} finally {
	if (br != null)
		br.close();
}
```
- `BufferedWriter`:
```java
BufferedWriter bw = null;
String text = "Dupa";
try {
	bw = new BufferedWriter(new FileWriter("./plik.txt"));
	bw.write(text);
	bw.newLine();
} finally {
	if (bw != null)
		bw.close();
}
```
- `PrintWriter`:
```java
PrintWriter pw = null;
String text = "Dupa";
try {
	pw = new PrintWriter(new FileWriter("./plik.txt"));
	pw.printLine(text);
} finally {
	if (pw != null)
		pw.close();
}
```

## GUI_PL_03.05 RandomAccessFile
- Służy do otwierania plików w jednym z 4 trybów:
	- `r` - read
	- `rw` - read / write (jeżeli plik nie istnieje będzie próba utworzenia go)
	- `rws` - read / write synchronised (zmienia również metadane - każda operacja na pliku automatycznie zapisuje zmiany na dysku)
	- `rwd` - read / write data - podobnie jak powyżej ale bez zapisu metadanych

```java
RandomAccessFile file = null;

try {
	file = new RandomAccessFile("data.dat", "rw");
	String name = "Adam";
	String surname = "Smith";
	int age = 25;

	file.writeUTF(name);
	file.writeUTF(surname);
	file.writeInt(age);
} finally {
	if (file != null)
		file.close();
}
```
- Istnieje metoda do zapisywania pointerów w pliku: `file.getFilePointer()` - zwraca `long`a z miejscem w pliku gdzie jest "karetka".
- kluczowe jest kiedy ją wywołujemy (aktualny koniec pliku)
- jak mam zapisane miejsce to mogę wywołać `file.seek(position)` a potem normalnie metodę do zapisywania do pliku
- jeżeli będę próbował wyjść poza plik to rzuci `EndOfFileException`
- rozmiar pliku dostanę poprzez: `file.length();`
- Jak chcę powiększyć plik to `file.setLength(int);` - wypełni go nullami

## GUI_PL_04.01 Wprowadzenie do klas wewnętrznych
- Klasy wewnętrzne dzielą się na:
	- *standardowe klasy wewnętrzne (niestatyczne)*
	- *klasy wewnętrzne statyczne (zanurzone)*
	- *lokalne klasy wewnętrzne*
	- *anonimowe klasy wewnętrzne*
- W ostatnich 3ch typach modyfikator `static` jest pomijalny

## GUI_PL_04.02 Niestatyczna klasa wewnętrzna
- Dostęp do publicznej klasy wewnętrznej jest po kropce:
```java
OuterClass.InnerClass innerClassInstance = outerClassInstance.new InnerClass();
```
## GUI_PL_04.04 Klasy lokalne
- Klasa lokalna:
	- może być zadeklarowana gdziekolwiek, nawet w metodzie
	- nie ma modyfikatora dostępu - jest prywatna
	- nie da się do niej odwołać z zewnątrz

## GUI_PL_04.05 Anonimowa klasa wewnętrzna
- Można utworzyć tylko jedną instancję:
```java
Show show = new Show() {
	@Override
	public void showHelloWithText() {
		// ...
	}
};
```
- W tym przykładzie ona dziedziczy po show
- Zmienne lokalne do których się w niej odwołuję muszą być `final` lub `effectively final` czyli nigdy nie modyfikowane.

## GUI_PL_05 Klasa Abstrakcyjna
- Może zawierać sygnaturę metody bez jej ciała. Wtedy w klasie dziedziczącej trzeba ją nadpisać.

## GUI_PL_06 Interfejsy i interfejsy funkcyjne
- Wszystkie metody są **zawsze publiczne**.
- Klasa może implementować kilka interfejsów.
- Aby dodać implementację używamy słowa kluczowego `default`.
- Interfejs mogą dziedziczyć po innych interfejsach, nawet po kilku.
- ***Interfejsy Funkcyjne*** są wtedy gdy interfejs posiada tylko jedną niezaimplementowaną metodę
- ***Interfejsy Funkcyjne*** mogą posiadać adnotację `@FunctionalInterface`

## GUI_PL_07 Lambda wyrażenia
- Nie trzeba deklarować w niej typu
- Tworzy się na podstawie interfejsu funkcyjnego z jedną niezaimplementowaną metodą, stąd wiadomo co wykonać na instancji

## GUI_PL_08 Enumeratory czyli typy wyliczeniowe
- Na wartościach mam metody:
	- `.valueOf()` - zwraca wartości
	- `.ordinal()` - zwraca indeks
	- `.name()` - zwraca nazwę (jakbym potrzebował jakoś rzeźbić w `.toString()` to się przyda).
- Na samym enumie:
	- `.values()` - zwraca tablicę z wartościami (wykona `.toString()`)
```java
public enum MyColor {
	RED = "#FF0000",
	GREEN,
	BLUE
}
```

`Color.GREEN` zwróci `GREEN` - nazwę.

- Można przedefiniować wartość poprzez klasę anonimową:
```java
public enum MyColor {
	RED {
		@Override
		public String toString(){
			// ...
			return "Czarny";
		}
	},
	BLUE
}
```
- Można tworzyć pola i konstruktory:

```java
public enum MyColor {
	private boolean isBasicColor;
	Color() {
		isBasicColor = false;
	}
	Color(boolean isBasicColor) {
		this.isBasicColor = isBasicColor;
	}
	public boolean isBasicColor() {
		return this.isBasicColor;
	}
	BLACK,
	RED(true),
	BLUE(true),
	PINK
}
```
```java
// Main.java
Color.RED.isBasicColor(); // -> true
```
- Enumeratory mogą posiadać metody abstrakcyjne:
```java
public enum Formatter {
	public abstract String format(String message);

	CALM {
		@Override
		public String format (String message) {
			// cośtam
			return "coś na miękko";
		}
	},
	NERVOUS {
		@Override
		public String format (String message) {
			// cośtam
			return "COS KAPSLOKIEM";
		}
	},
}
```
## GUI_PL_09.01 Wprowadzenie do Java Generics
```java
FigureBox <Square> squareFigureBox = new FigureBox<>(square);
```
Drugi nawias kwadratowy może być pusty bo zadeklarowałem typ w pierwszym.

## GUI_PL_09.02 Ogranieczenia typów generycznych
- Ograniczamy je po interfejsach:
```java
public class FigureBox<T extends Figure> {
```

## GUI_PL_09.03 Wildcards
