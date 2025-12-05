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

- Cel: pracować z różnymi typami generyków (`FigureBox<Square>` ≠ `FigureBox<Figure>`).
- Zasada PECS: **Producer → Extends**, **Consumer → Super**.

- `?` (nieznany typ)
  ```java
  void method(FigureBox<?> box);
  ```
  - czytam jako `Object`, nie wkładam.

- `? extends Figure` (górne ograniczenie)
  ```java
  void method(FigureBox<? extends Figure> box);
  ```
  - coś, co dziedziczy po `Figure`;
  - czytam jako `Figure`, nie wkładam (`Producer → Extends`).

- `? super Circle` (dolne ograniczenie)
  ```java
  void method(FigureBox<? super Circle> box);
  ```
  - `Circle` lub wyżej (`Figure`, `Object`);
  - wkładam `Circle`, czytam jako `Object` (`Consumer → Super`).

| Akcja   | Użyj          | Co robi            |
|---------|---------------|--------------------|
| Czytam  | `? extends T` | nie wkładam        |
| Wkładam | `? super T`   | czytam jako Object |

## GUI_PL_10.01 Wprowadzenie do Java Collection Framework
JCF dzieli się na Kolekcje i Mapy:
- Kolekcje (interfejs _Collection_) to między innymi:
	- Listy (uporządkowany zbiór elementów)
	- Zbiory
	- Kolejki
- Mapy (interfejs _Map_)

---
- Jedne i drugie są iterowalne (interfejs _Iterable_) i wykorzystują generyki.
- Zbiory oraz Mapy to referencje do obiektów.
- Mapy posiadają dwa typy - klucza i wartości.
- Kolekcje nie są tak szybkie jak tablice.

```java
	Collection.add(); // Dodaje element
	Collection.addAll(); // Dodaje drugą kolekcję do tej
	Collection.clear(); // Czyści kolekcję
	Collection.contains(); // Zwraca czy element jest w kolekcji
	Collection.containsAll(); // Zwraca czy cała kolekcja jest zawarta w tej o którą pytamy
	Collection.equals(); // sprawdza czy są równe
	Collection.hashCode(); // zwraca hashcode
	Collection.empty(); // sprawdza czy jest pusta
	Collection.iterator(); // zwraca obiekt iteratora z tej kolekcji
	Collection.remove(); // Usuń element
	Collection.removeAll(); // Usuń elementy kolekcji z kolekcji
	Collection.removeIf(); // Usuwanie warunkowe
	Collection.retainAll(); // Pozostaw elementy
	Collection.size(); // Zwraca rozmiar
	Collection.stream(); // Zwraca stream danych
	Collection.toArray(); // Zwraca Array
```
- W ramach listy istnieją dwie podstawowe implementacje:
	- LinkedList - opiera się na listach dwukierunkowych - dostęp do elementu jest kosztowniejszy niż w ArrayList
	- ArrayList - dodawanie gdzie indziej niż na końcu jest kosztowne

---
- Set (zbiór) - nie może posiadać duplikatów
	- TreeSet - implementacja oparta o drzewo czerwono czarne. Dostęp do elementów w czasie logarytmicznym.
	- HashSet - Dostęp do elementów jest szybki (stały). Kolejność elementów jest nieokreślona
---
- Queue(kolejki) - elementy przechowywane w danej kolejności (FIFO):
	- podtypem jest PriorityQueue - elementy uporządkowane wg. priorytetu. Priorytet określony za pomocą `.compareTo()`.

## GUI_PL_10.02 JCF Listy
- Rozmiar jest dynamiczny, ale można zadeklarować przy tworzeniu rozmiar.
- Reprezentowane przez interfejs `java.util.List`.
- Dwa typy to `ArrayList` i `LinkedList`.
- ArrayList zazwyczaj ma zachowaną kolejność, ale może się zmienić.
- `.add(T elem)` dodaje na końcu a `.add(int index, T elem)` dodaje pod wskazanym indeksem.
- `.indexOf(T elem)` zwraca indeks pierwszego wystąpienia danego elementu.
- `.remove(int i)` usuwa po indeksie a `.remove(T elem);` po elemencie. Usuwanie po elemencie usuwa pierwsze wystąpienie elementu.

## GUI_PL_10.03 JCF Zbiory
- Po dodaniu dwóch takich samych wartości nie leci wyjątek ale nic się nie dzieje - nie jest dodawane.
-  W hashSet kolejność definiuje funkcja hashująca

## GUI_PL_10.04 JCF Kolejki
- Działają jako FIFO
- `Queue.remove()` zdejmuje ostatni element.
- `Queue.peek()` wyświetla pierwszy element.
- `Queue.poll()` wyświetli i zdejmuje pierwszy element.

## GUI_PL_10.05 JCF Mapy
- Kolekcje par klucz-wartość
- Implementują metody z interfejsu `java.util.Map`
- `.put(key, val)` - dodaje lub nadpisuje klucz
- `.putIfAbsent(key, val)` - dodaje jeżeli nie ma takiego klucza
- `.getOrDefault(key, defaultValue)` - jeżeli nie ma danego klucza to zwraca default
- `.keySet()` zwraca zbiór kluczy jako `Set`
- `.values()` zwraca kolekcję wartości z Mapy
- `.entrySet()` - `Map.entry()`

---
- `HashMap` - Czas stały dostępu do elementów, kolejność nieokreślona.
- `LinkedHashMap` - Hashmapa ale linkowana dwustronnie.
- `TreeMap` - elementy muszą mieć dobrze określoną kolejność - jeżeli klucz nie jest liczbą to przekazujemy komparator. Dostęp do elementów jest bardzo szybki (poziomu logarytmicznego).

---
```java
Map.containsKey(T key); // - czy posiada klucz
Map.containsValue(V value); // - czy posiada wartość
Map.remove(key, value); // usuwa klucz wartość, muszą się zgadzać obie składowe
Map.remove(key); // usuwa parę po kluczu

// Wyświetlanie w pętli foreach
Map<String, String> cityCountry = new HashMap<>();

for (String key: cityCountry.keySet())
	System.out.println(key + " " cityCountry.get(key));
	
for (String value: cityCountry.values())
	System.out.println(value);
	
for (Map.Entry<String, String> entry : cityCountry.entrySet())
	System.out.println(entry.getKey() + " " + entry.getValue());
```

## GUI_PL_10.06 Iteratory
- Obiekt który reprezentuje widok na kolekcję obiektów. 
- Pamięta które elementy już zwrócił i wie czy są jeszcze jakieś do zwrócenia.
- Używa typów generycznych.
--- 
```java
// Java.util.Iterator
hasNext() // Zwraca czy są jeszcze elementy
next() // Zwraca kolejny element
remove() // Usuwa ostatni element zwrócony przez next
forEachRemaining(Consumer<? super E> action) // Wykonuje akcje dla każdego elementu który został lub do jebnięcia wyjątku 
```

```java
// Java.util.Iterable
iterator(); // Zwraca obiekt iteratora dla danej kolekcji
```
---

```java
public class IterableRange implements Iterable<Integer> {
	private int start, stop;

	public IterableRange(int start, int stop) {
		this.start = start;
		this.stop = stop;
	}

	@Override
	public Iterator<Integer> iterator() {
		return new Iterator<Integer>() {

			private int current = start;
			
			@Override
			public boolean hasNext() {
				return current <= stop;
			}

			@Override
			public Integer next() {
				if (!hasNext())
					throw new NoSuchElementException();
				return current++;
			}
		}
	}
}
```

```java
// Main.java

for (int i new IterableRange(5, 12))
	System.out.println("val: " + i);

Iterator<Integer> it = (new IterableRange(5, 12)).iterator();

while (it.hasNext()) {
	int val = it.next();
	System.out.println("val2: " + val);
}
```

## GUI_PL_11.01 Wprowadzenie do przetwarzania strumieniowego

- Strumienie nie modyfikują źródła danych.
- Operacja terminalna triggeruje operacje pośrednie do wykonania. To ta operacja która kończy przetwarzanie strumienia i zwraca wynik nie strumieniowy (albo efekt uboczny).
- Short circuting (krótkie zwarcie) - operacja która nie musi przejść całego strumienia, kończy wcześniej gdy spełni warunek albo weźmie tylko część danych.

```java
List<Integer> nums = List.of(1, 2, 3, 4);
boolean hasBig = nums.stream().anyMatch(n -> n > 2); // zatrzyma się na 3
```
- Operacje strumieniowe dzielą się na **statefull** i **stateless** w zależności od tego czy mają stan (jeżeli element N będzie wiedział o N-1 to jest stanowe)
- Stanowe w bibliotece podstawowej to:
	- `sorted()` - buforuje całość żeby posortować
	- `distinct()` - pamięta już widziane elementy, by usuwać duplikaty.
	- `limit(n)` - liczy, ile już zwrócono (short-circut)
	- `skip(n)` - liczy, ile pominąć zanim zacznie zwracać

## GUI_PL_11.02 Tworzenie strumieni
```java
collection.stream(); // -> zwróci stream z kolekcji
list.stream(); // -> list to przykład kolekcji
```
analogicznie działa statyczna metoda `Stream.of()` na klasie Stream:

```java
Stream.of(collection); // -> zwróci stream z kolekcji collection
Stream.of(array);
Stream.of("val1", "val2", "val3");
```

- `Stream.generate` i `Stream.iterate`:
```java
Stream<Double> stream1 = Stream.generate(Math::random);
Stream<Integer> stream2 = Stream.iterate(1, n -> n + 3);
```

- `IntStream`:
```java
IntStream stream3 = IntStream.range(1, 5); // -> zwróci od 1 do 4
IntStream stream4 = IntStream.rangeClosed(1, 5); // -> od 1 do 5
```

- Strumienie z pliku
```java
try (Stream<String> stream5 = Files.lines(Path.of("file.txt"))) {
	// ...
} catch (Exception ex) {
	e.printStackTrace();
}
```

- Strumienie z wyrażeń regularnych:
```java
String text = "Alice has AIDS";
Stream<String> stream9 = Pattern.compile("\\s").splitAsStream(text);
```

## GUI_PL_11.03 Operacje na strumieniach

- `Stream<T> filter(Predicate<T> pred)`:

```java
Stream.of(1, 2, 3, 4, 5, 6)
	.filter(x -> x % 2 == 0)
	.forEach(System.out::println);
```

- `Stream<R> map(Function<T, R> fun)`:

```java
Stream.of("Python", "Java", "C++")
	.map(String::toUpperCase)
	.forEach(System.out::println);
```

- `Stream<T> distinct()`:

```java
Stream.of(1, 2, 2, 3, 3, 4, 5)
	.distinct() // Uważaj, to nie jest zbyt optymalne bo jest statefull
	.forEach(System.out::println);
```

- `Stream<R> flatMap(Function<T, Stream<R>> fun)`:

Zwraca strumień strumieni a potem go spłaszcza.

```java
Stream.of("a b", "c d e")
	.flatMap(s -> Stream.of(s.split(" ")))
	.forEach(System.out::println);
```

- `Stream<T> sorted(Comparator<T> cmp)`:

```java
Stream.of("pear", "apple", "orange")
	.sorted()
	.forEach(System.out::println);
```

- `Stream<T> peek(Consumer<T> cons)`:

```java
Stream.of(1, 2, 3)
	.peek(x -> System.out.println("Before change: " + x))
	.map(x -> x*2)
	.forEach(System.out::println);
```

## GUI_PL_11.04 Operacje terminalne

```
.count() - liczy sumę
.min() - zwraca minimalny element, trzeba przekazać komparator
chociażby Integer::compareTo
.max() - zwraca maksymalną
.collect() - zwraca kolekcję
.collect(Collectors.toList());
.toArray() - zwraca array
.groupingBy() - grupuje według klucza
.joining() - zwraca stringa z konkatenacją
.toList() - zwraca listę stringów
.allMatch() - przyjmuje funkcję, zwraca czy każdy element ją spełnia
.anyMatch() - czy cokolwiek spełnia
.noneMatch() - czy żaden nie spełnia
```

```java
int min = Stream.of(3, 7, 2, 5, 7).min(Integer::compareTo).get();

Map<Character, List<String>> map = Stream.of("java", "python", "C++", "rust)
	.collect(Collectors.groupingBy(s -> s.charAt(0)));

System.out.println(map);

String text = Stream.of("java", "python", "C++", "rust")
	.collect(Collectors.joining(", "));
```

- jest jeszcze `reduce`:

```java
int sum = Stream.of(1, 2, 3, 4).reduce(0, Integer::sum);
int product = Stream.of(1, 2, 3, 4).reduce(1, (a, b) -> a * b);
int count = Stream.of(1, 2, 3, 4).reduce(0, (a, b) -> a+1);
```

## GUI_PL_12.01 Wprowadzenie do predefiniowanych interfejsów funkcyjnych

- Mieszkają w `java.util.function`.
- Najczęstszy podział:
	- Konsumenci (consumers)
	- Funkcje (functions)
	- Operatory (operators)
	- Predykaty (predicats)
	- Dostawcy (supliers)
s
## GUI_PL_12.02 Konsumenci

- Przyjmują argument ale niczego nie zwracają.
- Celem są side effecty.
- Podstawowa implementacja to `Consumer<T>`:

```
Consumer<T>
void accept(T t)
```

```java
Consumer<String> printer = s -> System.out.println("Hello, " + s + "!");

printer.accept("Java");
printer.accept("World");

Consumer<String> length = s -> System.out.println("Length: " + s.length());

printer.andThen(length).accept("Lambda");

// BiConsumer<T, U> - przyjmuje dwa argumenty ale nic nie zwraca
// 		void accept(T t, U u)

BiConsumer <String, Integer> repeat = (word, times) -> {
	for (int i = 0; i < times; i++) {
		System.out.print(word + " ");
	}
	System.out.println();
};

repeat.accept("Java", 3); // -> wyświetli Java Java Java \n
```

## GUI_PL_12.03 Funkcje

- Wykonuje jakąś operację przekształcenia.

```java
// Function<T, R>
// 		R apply(T)

Function <Integer, String> intToString = i -> "Liczba: " + i;
Function <String, Integer> stringLength = s -> s.length();

System.out.println(intToString.apply(10));
System.out.println(stringLength.apply("Java"));

Function<Integer, Integer> square = x -> x * x;
Function<Integer, String> describe = x -> "Wynik to: " + x;

System.out.println(square.andThen(describe).apply(5));
System.out.println(describe.compose(square).apply(5));
// -- oba powyższe dadzą ten sam wynik, ale mają odwrotną kolejność wykonywania operacji

// Jest też wersja dla typów prymitywnych jak IntFunction, DoubleFunction etc.
// IntToDoubleFunction, LongToDoubleFunction etc. też.

// BiFunction<T, U, R>
// 		R apply(T t, U u)

BiFunction<Integer, Integer, String> sumToText = (a, b) -> "Sum: " + (a + b);
BiFunction<String, Integer, String> repeat = (text, times) -> text.repeat(times);

System.out.println(sumToText.apply(3, 4));
System.out.println(repeat.apply("Hi", 3));
```

## GUI_PL_12.04 Operatory

- Operator to funkcja która przyjmuje jedną lub dwie wartości danego typu i zwraca wynik tego samego typu.
- Dzielą się na `unary`(przyjmujące jedną wartość) i `binary`(przyjmujący dwie) .
```java
// UnaryOperator<T>

UnaryOperator<Integer> square = x -> x * x;
UnaryOperator<Double> halve = x -> x / 2.0;
UnaryOperator<String> exclain = x -> x + "!";

System.out.println(square.apply(5));
System.out.println(halve.apply(5.0));
System.out.println(exclain.apply("Java"));

/* Są też dedykowane:

	- IntUnaryOperator
	- LongUnaryOperator
	- DoubleUnaryOperator.applyAsInt();
	
*/

// BinaryOperator<T>

BinaryOperator<Integer> add = (a, b) -> a + b;
BinaryOperator<String> concat = (a, b) -> a + b;
BinaryOperator<Double> max = (a, b) -> Math.max(a, b);

System.out.println(add.apply(3, 4));
System.out.println(concat.apply("Hello ", "World"));
System.out.println(max.apply(2.5, 4.1));

/* Są też dedykowane:

	- IntBinaryOperator
	- LongBinaryOperator
	- DoubleBinaryOperator

*/
```

- W przypadku interfejsu `BinaryOperator` mamy dwie metody statyczne:
	- `.maxBy()` - przyjmuje komparator i zwraca operator wybierający **większy** element.
	- `.minBy()` - analogicznie – mniejszy.

```java
Comparator<Integer> cmp = Integer::compare;

BinaryOperator<Integer> minBy = BinaryOperator.minBy(cmp);
BinaryOperator<Integer> maxBy = BinaryOperator.maxBy(cmp);

System.out.println(minBy.apply(5, 9));
System.out.println(maxBy.apply(5, 9));
```

## GUI_PL_12.05 Predykaty

- Funkcje które sprawdzają wartości logiczne i zwracają `true` lub `false`.
- Nie ma metody `apply` tylko `test`.

```java
Predicate<Integer> isEven = x -> x % 2 == 0;
Predicate<String> isLongWord = x -> x.length() > 5;

System.out.println(isEven.test(4));
System.out.println(isEven.test(7));

System.out.println(isLongWord.test("Stream"));
System.out.println(isLongWord.test("Java"));


// Łączenie predykatów:

Predicate<Integer> isPositive = x -> x > 0;

Predicate<Integer> oddOrNegative = isEven
	.negate()
	.or(isPositive.negate());

System.out.println(oddOrNegative.test(-3));
System.out.println(oddOrNegative.test(3));
System.out.println(oddOrNegative.test(4));

/*	Defaulty:

		- IntPredicate
		- LongPredicate
		- DoublePredicate
	
*/

// BiPredicate<T, U> -> boolean

BiPredicate<Integer, Integer> isDivisibleBy = (a, b) -> a % b == 0;

System.out.println(isDivisibleBy.test(10, 5));
System.out.println(isDivisibleBy.test(10, 3));
```

## GUI_PL_12.06 Dostawcy

- Funkcja bez argumentów, która zwraca wartość jakiegoś zdefiniowanego typu:
- Posiada jedynie metodę `get()`

```java
Supplier<String> greeting = () -> "Hello Java!";
Supplier<Integer> constant = () -> 42;
Supplier<Double> random = () -> Math.random();

System.out.println(greeting.get());
System.out.println(constant.get());
System.out.println(random.get());

/* 

	Defaulty:
		- IntSupplier
		- LongSupplier
		- DoubleSupplier
		- BooleanSupplier
etc.

mają też:
 .getAsInt();
etc.
*/

Supplier<LocalTime> timeSuplier = () -> LocalTime.now();

System.out.println(timeSupplier.get());
System.out.println(timeSupplier.get());
System.out.println(timeSupplier.get());
```

## GUI_PL_13.01 Wielowątkowość - Tło historyczne

- Każdy wątek ma swój stos, ale wszystkie mają jedną przestrzeń adresową.

## GUI_PL_13.02 Wielowątkowość - Tworzenie wątków

- Wątki są reprezentowane przez klasę `Thread`.
- Są dwa sposoby tworzenia nowej instancji:
	- Dziedziczymy po `Thread` i uzupełniamy ciało metody `run()`. Uruchomiamy poprzed stworzenie instancji i wywołanie metody `start()`.
	- Wykorzystując interfejs `Runnable` i przekazując go do klasy `Thread`.

```java

// MyThread.java

public class MyThread extends Thread {
	@Override
	public void run() {
		for (int i = 0; i < 10 ; i++)
			System.out.println("Pierwszy wątek " + i);
	}
}

// MyRunnable.java

public class MyRunnable implements Runnable {
	
	@Override
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.println("Drugi wątek " + i);
		}
	}
}

// Main.java

public class Main {
	public static void main(String[] args) {
	
		// Sposób 1.
		MyThread myThread = new MyThread();
		myThread.start();

		// Sposób 2.
		MyRunnable myRunnable = new MyRunnable();
		Thread thread = new Thread(myRunnable);
		thread.start();

		// lub anonimowo:
		Thread t3 = new Thread(() -> {
			for (int i = 0; i < 10; i++) {
				System.out.println("Trzeci wątek " + i);
			}
		});
	}
}
```

## GUI_PL_13.03 Wielowątkowość wykonywanie wątków, stany i priorytety

- Stany sprawdzamy poprzez `Thread.state`
- Stany wątków:
	- `NEW` - po utworzeniu
	- `RUNNABLE` - wykonywany wątek
	- Stany końcowe:
		- `TERMINATED` - wykonany wątek
		- `WAITING` - czeka na zasoby lub jest wywłaszczony
			- `wait` - wywołano: `wait`, `join` lub `park` 
			- `notify` -
		- `BLOCKED` - wątek jest zablokowany
			- `lock acquired` - zablokuj wątek
			- `lock released` - odblokuj wątek (zmieni stan na `RUNNABLE`)
		- `TIMED_WAITING` - analogiczny do `WAITING` ale z maksymalny limit czasu 
			- `wait(time)`
			- `notify or timeout`

---

- Wątki mają priorytety wykonania. Można modyfikować priorytety metodą `Thread.setPriority()`. Implementacja zależy od systemu operacyjnego.
- Wątek ma flagę oznaczającą czy jest przerwany czy nie - ale to jest niezależne i niezautomatyzowane.

## GUI_PL_13.04 Wielowątkowość - Synchronizacja i oczekiwanie na wątki

```java
// NonSyncThread.java

public class NonSyncThread extends Thread {
	public static int value = 4000000;

	@Override
	public void run() {
		for (int i = 0; i < 1000000; i++) {
			value--;
		}
	}
}

// Main.java

public class Main {
	public static void main(String[] args) {
		Thread nonSyncThread1 = new NonSyncThread();
		Thread nonSyncThread2 = new NonSyncThread();
		Thread nonSyncThread3 = new NonSyncThread();

		nonSyncThread1.start();
		nonSyncThread2.start();
		nonSyncThread3.start();

		try {
			nonSyncThread1.join();
			nonSyncThread2.join();
			nonSyncThread3.join();
		} catch (InterruptedException ex) {
			ex.printStackTrace();
		}

		System.out.println(NonSyncThread.value); 
			// Zwróci jakąś losową wartość bo są nie synchronizowane
			// Nastąpiło **szamotanie wartości** (Race Condition)
	}
}
```

- Do synchronizacji można użyć monitora stanu wątku. Są dwa sposoby:
	- przez blok synchronizacyjny
```java
// Main.java, public static void main() {

synchronized (monitor) {
	// instrukcje
}
```

```java
// SyncThread.java

public class SyncThread extends Thread {
	public static int value = 4000000;
	public static String monitor = new String();

	@Override
	public void run() {
		for (int i = 0; i < 1000000; i++) {
			synchronized(monitor) {
				value--;
			}
		}
	}
}
```

	- przez metodę synchronizacyjną:

```java
// W klasie wątku:

public synchronized void nazwaMetody() {
	// instrukcje
}
```

```java
// SyncThread2.java

public class SyncThread2 extends Thread {
	public static int value = 4000000;
	public static String monitor = new String();

	@Override
	public void run() {
		for (int i = 0; i < 1000000; i++) {
			decrement();
		}
	}

	// chcę możliwie najmniejszy kawałek kodu wyciągnąć do metody
	// to się nazywa sekcja krytyczna
	public synchronized static int decrement() {
		return value--;
	}
}
```

- `.join()` czeka aż się skończy podwątek
- można odpalić `.join()` żeby czekał tylko jakiś czas
- można wykonać na wątku metodę `.wait()` żeby wrzucić go do stanu waiting albo `.sleep(interval)` żeby do timed_waiting

## GUI_PL_13.05 Wielowątkowość - Zatrzymywanie wątków
- korzystamy z metody `.interrupt()` - zostanie przerwany i flaga `isInterrupted` zostanie ustawiona na `true`

```java
// MyThread2.java
public class MyThread2 extends Thread {
	public int value = 1;

	@Override
	public void run() {
		while (!Thread.interrupted()) {

			// brak tego try wywaliłby wyjątek i nie przerwał działania podczas oczekiwania
			try { 
				Thread.sleep(1000); // Wykonujemy co 1s
			} catch (InterruptedException ex) {
				return;
			}

			value++;
		}
	}
}

// Main.java
public class Main {
	public static void main(String[] args) {
		MyThread2 myThread2 = new MyThread2();

		myThread2.start();
		
		try {
			Thread.sleep(10000); // czekamy na 10s żeby było widać wynik
		} catch (InterruptedException ex) {
			ex.printStackTrace();
		}

		myThread2.interrupt();
		System.out.println();
	}
}
```

## GUI_PL_13.06 Wielowątkowość - Najczęstsze problemy współbierzności

- Podstawowe problemy:
	- **Race condition** - dwa lub więcej wątków uzyskuje dostęp do wspólnego zasobu, a wynik zależy od kolejności wywołania
	- **Interferencja wątków** - dwa lub więcej wątków uzyskują dostęp do wspólnego zasobu i zakłócają się nawzajem
	- **Błędy w spójności pamięci** - jeżeli jest problem z synchronizacją
	- **Zakleszczenie** - jakieś wątki wzajemnie czekają na swój wynik
	- **LiveLock** - odwrotność zakleszczenia - ciągle zmieniają wartość czekając na siebie
	- **Zagłodzenie wątków** - cały czas są wątki z wyższym priorytetem
- Rozwiązania:
	- Używanie managera wątków - np. `ConcurencyAPI` i w nim `ExecutorService`.

## GUI_PL_14.01 Wprowadzenie do Swing'a

- **AWT** - *Abstract Window Toolkit* to biblioteka ciężkich komponentów:
	- importuje się go z `java.awt`
	- używa **ciężkich komponentów** czyli takich bazowanych na UI systemu (różne na różnych platformach)
- **Swing**
	- importuje się go z `javax.swing`
	- używa **lekkich komponentów** czyli takich napisanych od zera, niezależnych od platformy (z wyjątkiem `JFrame`, `JApplet`, `JWindow`)
	- jest nieznacznie wolniejszy ze względu na to, że komponenty są pisane od zera
	- jego komponenty dziedziczą po `JComponent`

---
Ponadto:
	- Niektóre klasy (np. `Font` czy `Color`) są współdzielone
	- `ContentPain` to taki kontener na zawartość
	- Zarządcy rozkładu definiują jak komponenty będą rozłożone na oknie
	- Za interakcje odpowiadają zdarzenia ale więcej będzie o nich przy okazji *Delegacyjnego Modelu Zdarzeń*

## GUI_14.02 Tworzenie i obsługa okien

- metoda `pack()` ustawia rozmiar okna na najmniejszy który pomieści jego zawartość
- metoda `setSize(x, y)` ustawia konkretny rozmiar okna
- To co ma zostać wyrenderowane musi być widoczne z wątku **EDT** (*Event Dispatching Thread*). W przeciwnym wypadku możemy dopuścić do zakleszczenia. Dbamy o to wywołując główne okno używając `SwingUtilities.invokeLater()`.
- *EDT* obsługuje:
	- zdarzenia
	- rysowanie komponentów
	- wywołanie listenerów

```java
// MyFrame.java
import javax.swing.*;

public class MyFrame extends JFrame {
	public MyFrame() {
		// można nadpisać jak ten panel ma wyglądać
		JPanel panel = new JPanel(){
			@Override
			protected void paintComponent(Graphics g) {
				super.paintComponent(g);

				int width = getWidth()-1;
				int height = getHeight()-1;

				for (int i = 10; i < width / 2; i += 10) {
					int w = width - i * 2;
					int h = height - i * 2;
					g.drawRect(i, i, w, h);
				}
			}
		};

		add(panel);
		setSize(200, 200);
		setLocationRelativeTo(null); // wyśrodkowuje okno
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true); // trzeba je wyświetlić
	}
}
```

```java
// Main.java
import javax.swing.*;

public class Main {
	public static void main(String[] args) {
		SwingUtilities.invokeLater(new Runnable() {
			@Override
			public void run() {
				new MyFrame();
			}
		});
	}
}
```

## GUI_14.03 Layout Managers

- Trzy podstawowe Layout Managery:
	- `FrameFlowLayout` - podstawowy layout
	- `GridLayout` - tabelaryczny do danych
	- `BorderLayout` - tabelaryczny

```java
// Main.java
import javax.swing.*;

public class Main {
	public static void main(String[] args) {
		SwingUtilities.invokeLater(new Runnable() {
			@Override
			public void run() {
				new MyFrameFlowLayout();
				// new MyFrameGridLayout();
				// new MyFrameBorderLayout();
			}
		});
	}
}
```

### `FlowLayout`

```java
import javax.swing.*;

public class MyFrameFlowLayout extends JFrame {
	public MyFrameFlowLayout(){
		JPanel jPanel = new JPanel();

		JButton b1 = new JButton("Button 1");
		JButton b2 = new JButton("Button 2");
		JButton b3 = new JButton("Button 3");
		JButton b4 = new JButton("Button 4");
		JButton b5 = new JButton("Button 5");

// 			// Domyślne:
		jPanel.setLayout(new FlowLayout(FlowLayout.CENTER));
// 			// Wyrównane do lewej: 
//		jPanel.setLayout(new FlowLayout(FlowLayout.LEFT));
//			// Też dorównane do lewej
//		jPanel.setLayout(new FlowLayout(FlowLayout.LEADING));
//			// Do prawej
//		jPanel.setLayout(new FlowLayout(FlowLayout.RIGHT));
//			// Do prawej
//		jPanel.setLayout(new FlowLayout(FlowLayout.TRAILING));

// 			// Można przekazać też hgap i vgap 
// 			// czyli marginesy wewnętrzne między elementami:
// 		new FlowLayout(FlowLayout.CENTER, 50, 100);

		jPanel.add(b1);
		jPanel.add(b2);
		jPanel.add(b3);
		jPanel.add(b4);
		jPanel.add(b5);

		add(jPanel);

		setSize(1000, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### `GridLayout`

```java
	// Domyślnie kolumn jest tyle ile elementów i jeden wiersz
	// Wypełniają całe okno
	jPanel.setLayout(new GridLayout());

	// 2 lub 4 wartości przyjmuje konstruktor
	// rows, cols
	// rows, cols, hgap, vgap 
```


### `BorderLayout`

```java
	// Domyślnie każdy komponent wypełnia sekcję w całości
	// Więc widoczny będzie tylko ostatni bo nadpisze poprzednie
	// Ale w tym wypadku konstruktor i tak zostaje pusty
	// Konfigurujemy layout metodą `.add()`
	jPanel.setLayout(new BorderLayout());

	// Stara notacja to kierunki świata
	// jPanel.add(b2, BorderLayout.NORTH); // Góra 
	jPanel.add(b3, BorderLayout.PAGE_START); // Góra
	jPanel.add(b4, BorderLayout.PAGE_END); // Dół
	jPanel.add(b5, BorderLayout.LINE_START); // Lewo
	jPanel.add(b1, BorderLayout.LINE_END); // Prawo
	jPanel.add(b1, BorderLayout.CENTER);

	// Jeżeli chcę dodać kilka elementów w jednej
	// sekcji to muszę zagnieździć jPanele
```

## GUI_14.04 Ogólnie o komponentach i kontenerach

- Kontenery to komponenty, które mogą zawierać inne. Zwykle mają domyślny `LayoutManager`, ale mogą też działać bez niego (`null`).
- Interakcje komponentów z użytkownikiem są obsługiwane za pomocą zdarzeń.
- *Komponenty lekkie* mogą być przezroczyste.
- W przypadku Swinga mamy do dyspozycji skórki - `PluggableLookAndFeel`.
- W przyciskach możemy definiować Mnemoniki czyli Alt-kliknięcia (kliknięcie przycisku z altem).
- Możemy używać HTMLa do osadzenia w komponentach.
- Walidację pól tekstowych można wprowadzić za pomocą dziedziczenia po klasie abstrakcyjnej `InputVerifier`. Musimy zdefiniować metodę `verify(String text)`.
- Jeżeli pozycjonuję element za pomocą `setLocationRelativeTo()`, to muszę pamiętać, że punkt (0,0) okna leży w lewym górnym rogu okna, wartości rosną w dół i w prawo.
- Komponenty możemy blokować lub odblokowywać (`setEnabled(boolean state)`).
- W przypadku komponentów `visible` jest domyślnie na `true` (w przeciwieństwie do okien), ale możemy ustawić je na `false`.
- `JLabel` domyślnie jest przezroczysta (nie ma tła). Reszta komponentów domyślnie nie jest przezroczysta.
- Żeby ustawić kolor tła w `JLabel` musimy wykonać `setOpaque(true)` żeby wyłączyć przezroczystość.

---

- Komponenty które będziemy omawiać:
	- Przyciski – możemy na nich definiować tekst lub ikony (mogą być różne ikony dla różnych stanów - np. hover, clicked, focus, unfocus). Mamy metodę `doClick()` która symuluje kliknięcie (odpowiednik `triggerClick()`).
		- `JButton` - domyślny przycisk
		- `JToggleButton` - przycisk przełączalny (włączony / wyłączony)
		- `JCheckbox` - definiuje grupę checkboxów
		- `JRadioButton` - definiuje grupę radiobuttonów
	- Menu rozwijane (dziedziczy po przyciskach):
		- `JMenu`
		- `JMenuItem`
		- `JCheckboxMenuItem`
		- `JRadioMenuItem`
		- `JPopupMenu` - menu kontekstowe (right-click)
	- Suwaki - `JSlider`
	- Dialogi wyboru:
		- `JColorChooser` - wybór koloru
		- `JFileChooser` - wybór pliku
	- Pola edycyjne (posiadają walidację)
		- `JTextField` - jednowierszowe pole
		- `JTextArea` - wielowierszowe pole
		- `JPasswordField`
		- `JFormattedTextField` - posiada wbudowany walidator
		- `JTextComponent`
		- `JEditorPane`
		- `JTextPane`
	- Listy (wymagają MVC)
		- `JList` - Listy
		- `JComboBox` - Listy rozwijalne
		- `JTable` - Tabele
		- `JTree` - Drzewa
	- Kontenery
		- `JPanel` - prosty kontener
		- `JSplitPanel` - podział pionowy lub poziomy
		- `JTabbedPane` - podział na taby
		- `JScrollPane` - skrolowalny kontener
		- `JToolBar` - pasek narzędzi

## GUI_PL_14.05 Przegląd podstawowych komponentów

### Plik w którym wywołujemy demka:

```java
// Main.java
import javax.swing.*;

public class Main {
	public static void main(String[] args) {
		SwingUtilities.invokeLater(new Runnable() {
			@Override
			public void run() {
				// tu tworzymy instancję tego frejma którego
				// chcemy przetestować i odpalić:
				new JButtonFrame();
				// ...
			}
		});
	}
}
```

### JButton:
[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JButton.html)


![Wygląd JButton przykłady](https://media.geeksforgeeks.org/wp-content/uploads/20221209210416/RB1.jpg)

```java
import javax.swing.*;

public class JButtonFrame extends JFrame {
	public JButtonFrame() {

		JButton jButton = new JButton("Sladan daj spokoj...");

		jButton.setTooltipText("Tekst po najechaniu na przycisk");
		jButton.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				jButton.setText("kurwa...");
				
				if (jButton.getBackground() == Color.GREEN) {
					jButton.setBackgroundColor(Color.ORANGE);
				} else {
					jButton.setBackgroundColor(Color.GREEN);
				}
			}
		});

		// shorthand:
		// jButton.addActionListener((e) -> { /* ... */ });

		add(jButton);

		pack();
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### JCheckBox:

[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JCheckBox.html)

![Wygląd JCheckBox przykłady](https://media.geeksforgeeks.org/wp-content/uploads/1-139.png)

```java
import javax.swing.*;

public class JCheckboxFrame extends JFrame {
	public JCheckboxFrame() {

		JCheckBox jCheckBox1 = new JCheckBox("CheckBox 1");
		JCheckBox jCheckBox2 = new JCheckBox("CheckBox 2");
		JCheckBox jCheckBox3 = new JCheckBox("CheckBox 3"); 

		jCheckBox2.setSelected(true);
		
		ItemListener itemListener = new ItemListener() {
			@Override
			public void itemStateChanged(ItemEvent e) {
				Object source = e.getItemSelectable(); // -> Object, źródło zdarzenia
				if (source == jCheckBox1)
					jCheckBox1.setText(jCheckBox1.getText() + "+");
				else if (source == jCheckBox2)
					jCheckBox2.setText(jCheckBox1.getText() + "-");
				else (source == jCheckBox3)
					jCheckBox3.setText(jCheckBox1.getText() + "*");

				if(e.getStateChange() == ItemEvent.SELECTED)
					System.out.println("Selected");
				else
					System.out.println("Deselected");
			}
		};

		jCheckBox1.addItemListener(itemListener);
		jCheckBox2.addItemListener(itemListener);
		jCheckBox3.addItemListener(itemListener);

		// Mnemonic to jest shortcut do odpalenia eventu domyślnego (selected)
		jCheckBox1.setMnemonic(KeyEvent.VK_C);
		jCheckBox2.setMnemonic(KeyEvent.VK_D);
		jCheckBox3.setMnemonic(KeyEvent.VK_E);

		setLayout(new FlowLayout()); // domyślnie jest border

		add(jCheckBox1);
		add(jCheckBox2);
		add(jCheckBox3);

		pack();
		setSize(200, 200);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### JRadioButton:

[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JRadioButton.html)

![Wygląd JRadioButton przykłady](https://media.geeksforgeeks.org/wp-content/uploads/Screenshot-30-1.png)

```java
import javax.swing.*;

public class JRadioButtonFrame extends JFrame {
	public JRadioButtonFrame() {

		JRadioButton jRadioButton1 = new JRadioButton("Sladan plz 1");
		JRadioButton jRadioButton2 = new JRadioButton("Sladan plz 2");
		JRadioButton jRadioButton3 = new JRadioButton("Sladan plz 3");

		ActionListener actionListener = new ActionListener(){
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println(
					((JRadioButton)e.getSource()).getText()
				);
			}
		};

		jRadioButton1.addActionListener(actionListener);
		jRadioButton2.addActionListener(actionListener);
		jRadioButton3.addActionListener(actionListener);

		jRadioButton1.addItemListener(new ItemListener() {
			@Override
			public void itemStateChanged(ItemEvent e) {
				// SELECTED ma wartość 1 a DESELECTED 2
				System.out.println(e.getStateChange());
			}
		});

		// Żeby dało się zaznaczyć tylko
		// jedną opcję potrzebny ButtonGroup
		ButtonGroup buttonGroup = new ButtonGroup();
		buttonGroup.add(jRadioButton1);
		buttonGroup.add(jRadioButton2);
		buttonGroup.add(jRadioButton3);
		// Jak to pominę to wyświetlą się ok, 
		// ale będą w pełni niezależne

		setLayout(new FlowLayout());

		add(jRadioButton1);
		add(jRadioButton2);
		add(jRadioButton3);

		// pack();
		setSize(200, 200);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### JLabel:

[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JLabel.html)


![Wygląd JLabel przykłady](https://media.geeksforgeeks.org/wp-content/uploads/4000.png)

```java
import javax.swing.*;

public class JLabelFrame extends JFrame {
	public JLabelFrame() {

		JLabel jLabel1 = new JLabel("Kurwa..1");
		JLabel jLabel2 = new JLabel("Kurwa...................1");
		JLabel jLabel3 = new JLabel("Kurwa Mać");

		jLabel1.setBackground(Color.GREEN);
		jLabel1.setOpaque(true);
		jLabel1.setFont(new Font("Courier", Font.BOLD, 12));

		jLabel2.setIcon(new ImageIcon("ok.png"));

		jLabel3.setText("<html><h1><i>AaaaAaaAaAa</i></h1></html>");

		setLayout(new FlowLayout());

		add(jLabel1);
		add(jLabel2);
		add(jLabel3);

		// pack();
		setSize(500, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### JTextField:

[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JTextField.html)

![Wygląd JTextField przykłady](https://media.geeksforgeeks.org/wp-content/uploads/150.png)

```java
import javax.swing.*;

public class JTextFieldFrame extends JFrame {
	public JTextFieldFrame() {

		JTextField jTextField = new JTextField();

		jTextField.setBackground(Color.GREEN);
		jTextField.setForeground(Color.BLUE);

		Font f = new Font("Times New Roman", Font.BOLD , 24);
		jTextField.setFont(f);

		setLayout(new FlowLayout());

		add(jTextField);

		// pack();
		setSize(500, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### JTextArea:

[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JTextArea.html)

![Wygląd JTextArea przykłady](https://media.geeksforgeeks.org/wp-content/uploads/1900.png)

```java
import javax.swing.*;

public class JTextAreaFrame extends JFrame {
	public JTextAreaFrame() {

		// Domyślnie nie będzie posiadał scrolla
		// trzeba resizeować
		//
		// Dlatego dodajemy ScrollPane

		JTextArea jTextArea = new JTextArea();
		JScrollPane scrollPane = new JScrollPane(jTextArea);

		// Modyfikacje jak w JTextField

		add(scrollPane);

		// pack();
		setSize(500, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### JSlider:

[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JSlider.html)

![Wygląd JSlider przykłady](https://media.geeksforgeeks.org/wp-content/uploads/1-144.png)

```java
import javax.swing.*;

public class JSliderFrame extends JFrame {
	public JSliderFrame() {

		JSlider jSlider1 = new JSlider(JSlider.HORIZONTAL, 0, 100, 50);
		JSlider jSlider2 = new JSlider(JSlider.VERTICAL, 0, 100, 50);

		// Major & Minor tick
		jSlider1.setMajorTickSpacing(50);
		jSlider1.setMinorTickSpacing(20);
		jSlider1.setPaintTicks(true);
		jSlider1.setPaintLabels(true); // Tylko dla Major

		jSlider.addChangeListener(new ChangeListener() {
			@Override
			public void stateChanged(ChangeEvent e) {
				System.out.println(((JSlider) e.getSource()).getValue());
			}
		});

		setLayout(new FlowLayout());

		add(jSlider1);
		add(jSlider2);

		// pack();
		setSize(500, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### JSplitPane:

[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JSplitPane.html)

![Wygląd JSlider przykłady](https://media.geeksforgeeks.org/wp-content/uploads/2-94.png)

```java
import javax.swing.*;

public class JSplitPaneFrame extends JFrame {
	public JSplitPaneFrame() {

		JPanel jPanel1 = new JPanel();
		JPanel jPanel2 = new JPanel();

		jPanel1.setBackground(Color.BLUE);
		jPanel2.setBackground(Color.GREEN);

		JSplitPane jSplitPane = new JSplitPane(JSplitPane.HORIZONTAL_SPLIT, jPanel1, jPanel2);

		jSplitPane.setOneTouchExpandable(true); // Pokazuje szczałki

		jSplitPane.setDividerLocation(150);

		add(jSplitPane);

		// pack();
		setSize(500, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### JTabbedPane:

[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JTabbedPane.html)

![Wygląd JTabbedPane przykłady](https://media.geeksforgeeks.org/wp-content/uploads/20231020114339/cap1.PNG)

```java
import javax.swing.*;

public class JTabbedPaneFrame extends JFrame {
	public JTabbedPaneFrame() {

		JPanel jPanel1 = new JPanel();
		JPanel jPanel2 = new JPanel();
		JPanel jPanel3 = new JPanel();

		jPanel1.setBackground(Color.BLUE);
		jPanel2.setBackground(Color.GREEN);
		jPanel3.setBackground(Color.RED);

		JTabbedPane tabbedPane = new JTabbedPane();

		tabbedPane.add("pierwsza", jPanel1);
		tabbedPane.add("druga", jPanel2);
		tabbedPane.add("ostatnia", jPanel3);

		// pack();
		setSize(500, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

### JOptionPane:

[Oficjalna dokumentacja](https://docs.oracle.com/en/java/javase/21/docs/api/java.desktop/javax/swing/JOptionPane.html)

![Wygląd JOptionPane przykłady](https://media.geeksforgeeks.org/wp-content/uploads/20231031162134/2.PNG)

```java
import javax.swing.*;

public class JOptionPaneFrame extends JFrame {
	public JOptionPaneFrame() {

		JButton button = new JButton("Dupa");

		button.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {

				// ---
				JOptionPane.showMessageDialog(null, "Hasło to okoń");
				// pierwszy argument to okno względem którego pozycjonuję
				// drugi to wiadomość
				// trzeci argument to tytuł okna (<title>)
				// czwarty to typ (predefiniowane ikony)
				// piąty to ikona
				JOptionPane.showMessageDialog(null, "Hasło to dupa", "dupa", JOptionPane.WARNING_MESSAGE);

				// ---
				// jak chcę opcje mieć to zmieniam typ:
				int odpowiedz = JOptionPane.showConfirmDialog(null, "srasz?", "tytuł", JOptionPane.YES_NO_OPTION);

				System.out.println("Odpowiedź to: " + odpowiedz);
				// ok = 0, no = 1

				// ---
				String inputUsera = JOptionPane.showInputDialog(null, "Wpisz coś", "Title", JOptionPane.PLAIN_MESSAGE);
				System.out.println(inputUsera);

				// --- jak chcę własne przyciski to wrzucam array
				// i jeszcze podaję która jest domyślna 
				String[] tab = {"Tak", "Nie"};
				int checkedOption = JOptionPane.showOptionDialog(null, "?", "Title", JOptionPane.YES_NO_OPTION, JOptionPane.QUESTION_MESSAGE, null, tab, tab[1]);
				System.out.println(checkedOption);

				// --- kilka opcji robi select boxa
				// trzeba rzutować na string bo zwraca object
				String[] tab = {"A", "B", "C"};
				String s = (String) JOptionPane.showInputDialog(null, "Message", "Title", JOptionPane.PLAIN_MESSAGE, null, tab, tab[0]);
			}
		});

		add(jButton);

		// pack();
		setSize(500, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

## GUI_PL_15.01 Wprowadzenie do delegacyjnego modelu zdarzeń

- Eventy są obsługiwane na kolejce FIFO przez *Event Dispatching Thread* (EDT).
- EDT powinien być jedynym wątkiem obsługującym obsługującym zdarzenia.
- Wrzucamy do niego zdarzenia za pomocą `invokeLater` (z klasy `SwingUtilities` albo `EventQueue` - to jest ta sama klasa tylko udostępniona w dwóch miejscach).
- Do obsługi zdarzeń mogą być oddelegowane dowolne EventListenery.
- Starszym (nie zalecanym) sposobem było `Action` lub `HandleEvent`.
- Jak chcę dodać kilka listenerów to po prostu wywołuje je jedno po drugim.
- Listenery dodajemy metodą `addXListener` (ew usuwam przez `removeXListener`) gdzie `X` to jedno z poniższych:

#### Podział:
- `ActionListener` - zdarzenia generowane na rzecz danego komponentu
- `AdjustmentListener` - zdarzenia generowane w momencie zmiany stany komponentu
- `FocusListener` - pole tekstowe staje się aktywne lub przestaje być
- `ItemListener` - pole wyboru zostaje zaznaczone / odznaczone
- `WindowListener` - JWindow albo JFrame (maksymalizacja, minimalizacja, zamknięcie okna, przesunięcie okna)
- `MouseListener` - zdarzenia myszy
- `MouseMotionListener` - przesunięcie kursora, dragndrop
- `KeyListener` - zdarzenia z klawiatury

## GUI_PL_15.02 ActionEvent
`ActionEvent` korzysta z metody `actionPerformed`.

```java
public class MyButtonActionListener implements ActionListener {
	@Override
	public void actionPerformed(ActionEvent e) {
		System.out.println("Clicked");
	}
}
```

```java
import javax.swing.*;

public class MyFrame extends JFrame {
	public MyFrame() {
		JButton jButton = new JButton("MyButton");

		jButton.addActionListener(new MyButtonActionListener());

		jButton.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println("AIC called");
				
				JButton jakbymChcialUzycEventu = (JButton) e.getSource();
				jakbymChcialUzycEventu.setText("kliknalem");
			}
		});

		jButton
			.addActionListener(e -> 
				System.out.println("Shorthand lambda"));

		add(jButton);
		pack();
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

```java
import javax.swing.*;

public class Main {
	public static void main(String[] args) {
		SwingUtilities.invokeLater(() -> new MyFrame());
	}
}
```

## GUI_PL_15.03 ActionCommand i ClientProperty

- Na obiekcie klasy `ActionEvent` możemy wywołać `e.getActionCommand()` który zwraca nam Stringa z domyślną wartością elementu powiązanego z `ActionEvent` 
	- dla JButton etykietę 
	- dla pola edycyjnego tekst zawarty tekst
	- dla listy rozwijanej element tej listy
- `ActionCommand` można nadpisywać żeby skomunikować się z komponentem który dostał event. Obsługuje tylko jednego Stringa na raz.
- Jak chcę kilka rzeczy to korzystam z `clientProperty`:
```
jButton.getClientProperty("nazwa-klucza");
jButton.putClientProperty("nazwa-innego-klucza", "Wartość");
```
- `clientProperty` obsługuje wszystkie typy a nie tylko Stringa, ale muszę rzutować wartość.

## GUI_PL_15.04 MouseEvent
- W mouse eventach nie poużywam lambd bo jest więcej niż jedna metoda 
- `MouseInputListener` implementuje zarówno `MouseListener` jak i `MouseMotionListener` - musi przysłonić 7 metod
- `MouseAdapter` - pozwala przysłonić tylko te które nas interesują

```java
import javax.swing.*;

public class MyFrame extends JFrame {
	public MyFrame() {
		JButton jButton = new JButton("MyButton");

		jButton.addMouseListener(new MouseListener() {
			@Override
			public void mouseClicked(MouseEvent e) {
				System.out.println("Sraluch1");
				// w stosunku lewego górnego rogu komponentu
				System.out.println("X: " + e.getX());
				System.out.println("Y: " + e.getY());
				// w stosunku lewego górnego rogu okna
				System.out.println("X: " + e.getXOnScreen());
				System.out.println("Y: " + e.getYOnScreen());
			}
			
			@Override
			public void mousePressed(MouseEvent e) {
				System.out.println("Sraluch2");
			}
			
			@Override
			public void mouseReleased(MouseEvent e) {
				System.out.println("Sraluch3");
			}
			
			@Override
			public void mouseEntered(MouseEvent e) {
				System.out.println("Sraluch4");
			}
			
			@Override
			public void mouseExited(MouseEvent e) {
				System.out.println("Sraluch5");
			}
		});

		jButton.addMouseMotionListener(new MouseMotionListener() {
			@Override
			public void mouseDragged(MouseEvent e) {
				System.out.println("MouseDragged, ruszam wciśniętą myszką");
			}

			@Override
			public void mouseMoved(MouseEvent e) {
				System.out.println("MouseDragged, ruszam nie wciśniętą myszką");
			}
		});

		add(jButton);
		pack();
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}

```

## GUI_PL_15.05 KeyEvent

```java
import javax.swing.*;

public class MyFrame extends JFrame {
	public MyFrame() {
		JButton jButton = new JButton("MyButton");

		jButton.addKeyListener(new KeyListener() {
			@Override
			public void keyTyped(KeyEvent e) {
			// To się wywoła pomiędzy keyPressed a keyReleased
				
			}
			
			@Override
			public void keyPressed(KeyEvent e) {
				System.out.println(e.getKeyChar()); // Dany znak 'char'
				System.out.println(e.getKeyCode()); // Dany unicode
				
				// Control / shift / etc. nie mają keyTyped
			}
			
			@Override
			public void keyReleased(KeyEvent e) {
				System.out.println("Sraluch3");
			}
		});

		add(jButton);
		pack();
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

## GUI_PL_15.06 WindowEvent
- Jak nie chcę wszystkich metod napisywać to mam `WindowAdapter`

```java
import javax.swing.*;

public class MyFrame extends JFrame {
	public MyFrame() {
		JButton jButton = new JButton("MyButton");

		JFrame jFrame = this;

		addWindowListener(new WindowListener(){
			@Overide
			public void windowOpened(WindowEvent e) {
				
			}
		
			@Overide
			public void windowClosing(WindowEvent e) {
				// próba zamknięcia (może się nie dać, albo confirm dialog leci)
				int answer = JOptionPane.showConfirmDialog(null, "Takiś kurwa cfany?", "Zamknij", JOptionPane.YES_NO_OPTION);

				if (answer == JOptionPane.YES_OPTION) {
					jFrame.dispose();
				}
			}
		
			@Overide
			public void windowClosed(WindowEvent e) {
				// zamknięcie faktyczne
			}
		
			@Overide
			public void windowIconfield(WindowEvent e) {
				// minimalizacja do paska zadań		
			}
		
			@Overide
			public void windowDeiconfield(WindowEvent e) {
				// Maksymalizacja z paska zadań
			}
		
			@Overide
			public void windowActivated(WindowEvent e) {
					// focus na oknie
			}
		
			@Overide
			public void windowDeactivated(WindowEvent e) {
				// unfocus na oknie
			}
		});

		add(jButton);
		pack();
		setLocationRelativeTo(null);
		// JAK CHCE CONFIRM DIALOG TO MUSZĘ TO ZMIENIĆ NA DO NOTHING
		setDefaultCloseOperation(JFrame.DO_NOTHING_ON_CLOSE);
		setVisible(true);
	}
}
```

## GUI_PL_16 Architektura MVC
- Przykłady komponentów MVC w Swingu to `JList` i `JTable`.
- Kontroler zapewnia interakcję komponentu z widokiem.
- W Swingu widok i kontroler to to samo (komponent)
- Własny model ustawiamy `setModel` na komponencie
- Pobieramy model przez `getModel`
- Domyślnie mamy model z prefixem `Default` np. `DefaultListModel` albo `DefaultTableModel`
- Interfejsy modelu:
	- `ButtonModel`
	- `ComboboxModel`
	- `DocumentListSelectionModel`
	- `TableColumnModel`
	- `ListModel`
	- `TableModel`

## GUI_PL_17 JList
- Komponent `JList` ma dwa modele: 
	- oparty na interfejsie `ListModel` - odpowiedzialny za dane
	- `ListSelectionModel` - odpowiedzialny za UI

- `JList` posiada 2 konstruktory:
	- pierwszy przyjmuje wektor obiektów
	- drugi przyjmuje tablicę obiektów

```java
import javax.swing.*;

public class Main {
	public static void main(String[] args) {
		SwingUtilities.invokeLater(() -> new MyJListFrame());
	}
}
```

```java
import javax.swing.*;

// Prosty przykład
public class MyListFrame extends JFrame {
	public MyJListFrame() {

		String[] arr = {"Stephan", "Damon", "Mielona", "Jeremy"};

		JList jList = new JList(arr);
		JScrollPane jScrollPane = new JScrollPane(jList);

		add(jScrollPane);

		setSize(200, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

--- 

Przykład z `AbstractListModel`:

```java
import javax.swing.*;
import java.util.Vector;

public class MyListModel extends AbstractListModel<String> {

	private Vector<String> names;

	public MyListModel(Vector<String> names) {
		this.names = names;
	}

	@Override
	public int getSize() {
		return names.size();
	}

	@Override
	public String getElementAt(int index) {
		return names.get(index);
	}

	public void add(String text, int index) {
		names.add(index, text);
		fireIntervalAdded(this, index, index);
	}
	
	public void add(String text) {
		add(text, getSize());
	}

	public void remove(int index) {
		names.remove(index);
		fireIntervalRemoved(this, index, index);
	}
}
```

```java
import javax.swing.*;

public class MyListFrame extends JFrame {
	public MyJListFrame() {

		String[] arr = {"Stephan", "Damon", "Mielona", "Jeremy"};

		Vector<String> names = new Vector<>(Arrays.asList(arr));
		MyListModel myListModel = new MyListModel(names);

		JList jList = new JList();
		jList.setModel(myListModel);

		JButton addButton = new JButton("Add");
		JButton removeButton = new JButton("Remove");

		addButton.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				int i = jList.getSelectedIndex();

				if (i >= 0) {
					myListModel.add("Added element", i+1);
				} else {
					myListModel.add("Added in the end");
				}
			}
		});

		removeButton.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				int i = jList.getSelectedIndex();

				if (i >= 0) {
					myListModel.remove(i);
				}
			}
		});

		JPanel jPanel = new JPanel(new GridLayout(1, 2));
		jPanel.add(addButton);
		jPanel.add(removeButton)
		
		JScrollPane jScrollPane = new JScrollPane(jList);
		add(jScrollPane);
		add(jPanel.BorderLayout.PAGE_END);

		setSize(200, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

Kolejny przykład:

```java
import javax.swing.*;

public class MyListCellRenderer extends JLabel implement ListCellRenderer<String> {

	public MyListCellRenderer() {
		setOpaque(true);
	}

	@Override Component getListCellRendererComponent(JList<? extends String> list, Object value, int index, boolean isSelected, boolean cellHasFocus) {
		setText(value);

		if(index % 2 == 0) {
			setFont(new Font(Font.SANS_SERIF, Font.ITALIC, 16));
		} else {
			setFont(new Font(Font.DIALOG, Font.PLAIN, 12));
		}

		if (isSelected) {
			setBackground(Color.YELLOW);
		} else {
			setBackground(Color.WHITE);
		}

		return this;
	}
}
```

## GUI_PL_18 JTable
- `JTable` przyjmuje dwa modele danych:
	- pierwszy reprezentuje dane (dwuwymiarowa tablica) - interfejs `TableModel`
	- drugi reprezentuje nagłówki kolumn - interfejs `TableColumnModel`
- Istnieje też
	- interfejs `ListSelectionModel`

```java
import javax.swing.*;

public class Main {
	public static void main(String[] args) {
		SwingUtilities.invokeLater(() -> new MyTableFrame());
	}
}
```

```java
import javax.swing.*;

public class MyTableFrame extends JFrame {

	public MyTableFrame() {
		Object[][] items = {
			{"John", "Smith", 30},
			{"Elon", "Musk", 10},
			{"Michael", "Brzoska", 45},
			{"Sławomir", "Dańczak", 35}
		};

		String[] columns = { "Name", "Surname", "Age" };

		MyTableModel myTableModel = new MyTableModel(items, columns);

		JTable jTable = new JTable();
		jTable.setModel(myTableModel);

		JScrollPane jScrollPane = new JScrollPane(jTable);

		add(jTable);

		setSize(500, 500);
		setLocationRelativeTo(null);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);
	}
}
```

```java
import javax.swing.table.AbstractTableModel;

public class MyTableModel extends AbstractTableModel {

	private Object[][] items;
	private String[] columns;

	public MyTableModel(Object[][] items, String[] columns) {
		this.items = items;
		this.columns = columns;
	}

	@Override
	public int getRowCount() {
		return items.length;
	}

	@Override
	public int getColumnCount() {
		return columns.length;
	}

	@Override
	public Object getValueAt(int rowIndex, int columnIndex) {
		return items[rowIndex][columnIndex];
	}

	@Override
	public String getColumnName(int column) {
		return columns[column];
	}

	@Override
	public Class <?> getColumnClass(int columnIndex) {
		return getValueAt(0, columnIndex).getClass();
	}

	@Override
	boolean isCellEditable(int rowIndex, int columnIndex) {
		return columnIndex > 0;
	}

	@Override
	public void setValueAt(Object aValue, int rowIndex, int columnIndex) {
		items[rowIndex][columnIndex] = aValue;
		fireTableCellUpdated(rowIndex, columnIndex);
	}
}

```

## GUI_PL_19.01 Wprowadzenie do JavaFX

1. W JavaFX można tworzyć aplikacje na dwa sposoby:
- Za pomocą plików FXML
- Klasycznie w Javie tak jak wcześniej 
2. Każda aplikacja ma 3 fazy, odpowiadają one metodą
	- `init` - dane, przygotowanie połączenia z bazą
	- `start` - UI
	- `stop` - zamykanie aplikacji

`module-info.java` - metadane projektu:

Przykład `module-info.java`:
```java
module pl.pja.sladan.gui_chapter19 {
	requires javafx.controls;
	requires javafx.fxml;

	opens pl.edu.pja.sladan.gui_chapter19 to javafx.fxml;
	exports pl.edu.pja.sladan.gui_chapter19;
}
```

```java

import javafx.application.Application;
import javafx.stage.Stage;

public class First extends Application {

	// Main jest opcjonalne
	public static void main(String[] args) {
		launch(args);
	}

	@Override
	public void init() throws Exception {
		System.out.println("Entering init() on " + Thread.currentThread().getName());
	}

	@Override
	public void start(Stage stage) throws Exception {
		Button btn = new Button("Say hello");

		btn.setOnAction(e -> System.out.println("Hello"));

		StackPane root = new StackPane();
		root.getChildren().add(btn);

		Scene	scene = new Scene(root, 300, 250);

		stage.setScene(scene);
		stage.setTitle("Hello world!");
		stage.show();
	}
	
	@Override
	public void stop() throws Exception {
		System.out.println("Entering stop() on " + Thread.currentThread().getName());
	}
}
```

## GUI_PL_19.02 Layouts
- `Pane` layout nie rozmieszcza automatycznie elementów, pozycjonuje względem punktu 0,0 (lewy górny róg ekranu). Wymaga `.relocate(X, Y)`.


```java
public class PlainPaneExample extends Application {

	public void start(Stage stage) throws Exception {
		Pane root = new Pane();
		Button b1 = new Button("Button1");
		Button b2 = new Button("Button2");
	
		b2.relocate(10, 10);

		root.getChildren().addAll(b1, b2);
	
		stage.setScene(new Scene(root));
		stage.setTitle("Pane example");
		stage.show();
	}
}
```

```java
public class StackPaneExample extends Application {

	public void start(Stage stage) throws Exception {
		StackPane root = new StackPane();
		Button b1 = new Button("Button1");
		Button b2 = new Button("Button2");

		root.getChildren().add(b1);
		root.getChildren().add(b2);

		StackPane.setAlignment(b1, Pos.TOP_RIGHT);
	
		stage.setScene(new Scene(root, 300, 300));
		stage.setTitle("StackPane example");
		stage.show();
	}
}
```

```java
public class BorderPaneExample extends Application {

	public void start(Stage stage) throws Exception {
		BorderPane root = new BorderPane();

		TextArea textArea1 = new TextArea("TextArea 1");
		TextArea textArea2 = new TextArea("TextArea 2");
		TextArea textArea3 = new TextArea("TextArea 3");
		TextArea textArea4 = new TextArea("TextArea 4");
		TextArea textArea5 = new TextArea("TextArea 5");

		root.setTop(textArea1);
		root.setLeft(textArea2);
		root.setCenter(textArea3);
		root.setRight(textArea4);
		root.setLeft(textArea5);

		stage.setScene(new Scene(root, 1200, 500));
		stage.setTitle("BorderPane example");
		stage.show();
	}
}
```

## GUI_PL_19.03 Properties i bindings
- W JavaFX mamy **Properties** i **Bindings**
	- **Property** - obserwowalne i zwykle możliwość zapisu
	- **Binding** - zależne od innych properties i tylko read only

```java
import javafx.beans.property.IntegerProperty;
import javafx.beans.property.SimpleIntegerProperty;
import javafx.beans.property.Observable;
import javafx.beans.property.InvalidationListener;

public class PropertyBindingExample {
	public static void main(String[] args) {
		IntegerProperty src = new SimpleIntegerProperty(1);
		src.addListener(new InvalidationListener() {
			@Override
			public void invalidated(Observable observable) {
				System.out.println("Invalidated");
			}
		});

		System.out.println("1. " + src);
		src.set(2); // Lazy evaluation
		src.set(3); // Nie zadziała invalidation listener
		src.set(4); // Dopóki nie odczytam zmiennej
		src.set(5);
		System.out.println("3. get: " + src);
	}
}
```

```java
import javafx.beans.property.IntegerProperty;
import javafx.beans.property.SimpleIntegerProperty;
import javafx.beans.property.Observable;
import javafx.beans.property.InvalidationListener;

public class PropertyBindingExample {
	public static void main(String[] args) {
		IntegerProperty src = new SimpleIntegerProperty(1);
		src.addListener(new ChangeListener<Number>() {
			@Override
			public void invalidated(ObservableValue <? extends Number> observableValue, Number number, Number t1) {
				System.out.println("Changed");
			}
		});

		System.out.println("1. " + src);
		src.set(2); // Każdy wykona listener
		src.set(3); // Wykonanie change listenera odpaliło by też
		src.set(4); // Invalidation listener
		src.set(5);
		System.out.println("3. get: " + src);
	}
}
```

```java
import javafx.beans.property.IntegerProperty;
import javafx.beans.property.SimpleIntegerProperty;
import javafx.beans.property.StringProperty;
import javafx.beans.property.SimpleStringProperty;

public class PersonFX {
	private final StringProperty name = new SimpleStringProperty();
	private final IntegerProperty age = new SimpleIntegerProperty();

	public PersonFX(String name, int age) {
		this.name.set(name);
		this.age.set(age);
	}

	public String getName() {
		return name.get();
	}

	public int getAge() {
		return age.get();
	}

	public SimpleString nameProperty() {
		return name;
	}

	public SimpleInteger ageProperty() {
		return age;
	}
}
```

```java

public class PropertyBindingExample {

	public static void main(String[] args) {
		// Jednostronne wiązanie
		IntegerProperty p = new SimpleIntegerProperty(1);
		IntegerProperty q = new SimpleIntegerProperty();

		q.bind(p);
		System.out.println(q.get());
		p.set(2);
		System.out.println(p.get());
		System.out.println(q.get()); // też 2

		q.set(1); // wyjątek A bound value cannot be set.
		q.unbind();ązanie
		IntegerProperty p = new SimpleIntegerProperty(1);
		IntegerProperty q = new SimpleIntegerProperty();

		q.bind(p);
		System.out.println(q.get());
		p.set(2);
		System.out.println(p.get());
		System.out.println(q.get()); /

		// Dwustronne wiązanie
		p.bindBidirectional(q);
		p.set(3);
		System.out.println(p.get() + " " q.get());
		q.set(4);
		System.out.println(p.get() + " " + q.get());

		// Własne wiązanie
		DoubleProperty x = new SimpleDoubleProperty(3);
		DoubleProperty y = new SimpleDoubleProperty(4);

		DoubleBinding area = new DoubleBinding() {
			{
				super.bind(x, y);
			}

			@Override
			protected double computeValue() {
				return x.get() * y.get();
			}
		};

		System.out.println(area.get());
		x.set(5);
		y.set(6);
		System.out.println(area.get());

		// Predefiniowane wiązania

		DoubleBinding avg = Bindings.divide(Bindings.add(x, y), 2.0);
		System.out.println(avg.get());

		// --- inny sposob

		DoubleBinding avg2 = Bindings.createDoubleBinding(() -> (x.get() + y.get())/2.0, x, y);
		System.out.println(avg2.get());

		// --- Inny sposób, fluen API

		DoubleBinding avg3 = x.add(y).divide(2.0);
		System.out.println(avg3.get());
	}
}
```

## GUI_PL_19.04 Effects
Efekty działają na renderingu - modyfikują to jak jest renderowany węzeł a nie sam węzeł.

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.effects.DropShadow;
import javafx.scene.layout.StackPane;
import javafx.scene.paint.Color;
import javafx.scene.text.Font;
import javafx.scene.text.Text;
import javafx.scene.Stage;

public class EffectsBasic extends Application {
	@Override
	public void start(Stage stage) throws Exception {
		Text title = new Text("JavaFX Effects");
		title.setFont(Font.font(48));
		title.setFill(Color.CORNFLOWERBLUE);

		DropShadow shadow = new DropShadow();
		shadow.setRadius(12);
		shadow.offsetX(6);
		shadow.offsetY(6);
		shadow.setColor(Color.color(0, 0, 0, 0.35);

		Reflection reflection = new Reflection();
		reflection.setInput(shadow);

		title.setEffect(reflection);

		// title.setEffect(shadow);

		StackPane root = new StackPane(title);
		stage.setScene(new Scene(root, 600, 300));
		stage.setTitle("Effects - Basic");
		stage.show();
	}
}
```

## GUI_PL_19.05 List

```java
import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.Scene;
import javafx.stage.Stage;
import javafx.scene.control.Button;
import javafx.scene.control.ListView;

public class ListViewExample extends Application {
	@Override
	public void start(Stage stage) throws Exception {
		ObservableList<String> list = FXCollections.observableArrayList("Mary", "John", "Kate", "Cindy");

		ListView<String> listView = new ListView<>(list);
		listView.getSelectionModel().selectedItemProperty().addListener((observable, oldVal, newVal) -> {
			System.out.println("oldVal: " + oldVal + " newVal: " + newVal);
		});

		Button addButton = new Button("Add");
		Button removeButton = new Button("Remove");

		addButton.setOnAction(e -> list.add("test"));
		removeButton.setOnAction(e -> list.remove("test"));

		HBox root = new HBox(listView, addButton, removeButton);

		stage.setScene(new Scene(root, 400, 200));
		stage.setTitle("ListView Example");
		stage.show();
	}
}
```

## GUI_PL_19.06 Table

```java
import javafx.application.Application;
import javafx.stage.Stage;
import scene.Scene;
import javafx.layout.VBox;

public class TableViewExample extends Application {
	@Override
	public void start(Stage stage) throws Exception {

		ObservableList<PersonFX> list = FXCollections.observableArrayList(
			new PersonFX("Mary", 30),
			new PersonFX("MaryJane", 21),
			new PersonFX("MaryAdolf", 37),
			new PersonFX("Marry Christmas", 997),
			new PersonFX("Mary Sławomir", 42)
		);

		TableView<PersonFX> tableView = new TableView<>(list);
		TableColumn<PersonFX, String> nameColumn = new TableColumn<>("Name");

		nameColumn.setCellValueFactory(new PropertyValueFactory<>("name"));
		TableColumn<PersonFX, Integer> ageColumn = new TableColumn<>("Age");
		ageColumn.setCellValueFactory(new PropertyValueFactory<>("age"));

		tableView.getColumns().addAll(nameColumn, ageColumn);

		TextField nameTxtField = new TextField("Name");
		TextField ageTextField = new TextField("Age");
		Button addButton = new Button("Add");

		addButton.setOnAction(e -> list.add(new PersonFX(nameTextField.getText(), Integer.parseInt(ageTextField.getText()))));
		

		stage.setScene(new Scene(new VBox(tableView, nameTextField, ageTextField, addButton), 300, 250));
		stage.setTitle("TableView Example");
		stage.show();
	}
}
```

## GUI_PL_19.07 Animacje

```java
import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.shape.Rectangle;
import javafx.stage.Stage;
import javafx.util.Duration;

public class AnimationsExample extends Application {
	@Override
	public void start(Stage stage) throws Error {
		Group group = new Group();
		Rectangle rect = new Rectangle(50, 100, 200, 100);

		FadeTransition fadeTransition = new FadeTransiton(Duration.seconds(1), rect);
		fadeTransition.setFromValue(1.0);
		fadeTransition.setToValue(0.1);
		// fadeTransition.setCycleCount(Animation.INDEFINITE);
		// fadeTransition.setAutoReverse(true);
		// fadeTransition.play();

		FillTransition fillTransition = new FillTransition(Duration.seconds(1), rect, Color.RED, Color.YELLOW);
		// fillTransition.setCycleCount(Animation.INDEFINITE);
		// fillTransition.setAutoReverse(true);
		// fillTransition.play();

		ScaleTransition scaleTransition = new ScaleTransition(Duration.seconds(1), rect);
		scaleTransition.setToX(0.1);
		scaleTransition.setToY(0.2);
		// scaleTransition.setCycleCount(Animation.INDEFINITE);
		// scaleTransition.setAutoReverse(true);
		// scaleTransition.play();

		RotateTransition rotateTransition = new RotateTransition(Duration.seconds(1), rect);
		rotateTransition.setFromAngle(0);
		rotateTransition.setToAngle(180);
		// rotateTransition.setCycleCount(Animation.INDEFINITE);
		// rotateTransition.setAutoReverse(true);
		// rotateTransition.play();

		PathTransition pathTransition = new PathTransition(Duration.seconds(1), rect);
		Path path = new Path();
		path.getElements().add(new MoveTo(0, 0));
		path.getElements().add(new LineTo(300, 300));

		pathTransition.setPath(path);
		pathTransition.setNode(rect);
		pathTransition.setOrientation(PathTransition.OrientationType.ORTHOGONAL_TO_TANGENT);
		// pathTransition.setCycleCount(Animation.INDEFINITE);
		// pathTransition.setAutoReverse(true);
		// pathTransition.play();

		ParallelTransition parallelTransition = new ParallelTransition();
		paralelTransition.getChildren().addAll(fadeTransition, fillTransition, scaleTransition, rotateTransition, pathTransition);
		parallelTransition.setCycleCount(Animation.INDEFINITE);
		parallelTransition.setAutoReverse(true);
		parallelTransition.play();

		// SequentialTransition sequentialTransition = new ParallelTransition();
		// sequentialTransition.getChildren().addAll(fadeTransition, fillTransition, scaleTransition, rotateTransition, pathTransition);
		// sequentialTransition.setCycleCount(Animation.INDEFINITE);
		// sequentialTransition.setAutoReverse(true);
		// sequentialTransition.play();

		group.getChildren().add(rect);
		Scene scene = new Scene(group, 300, 300);
		stage.setScene(scene);
		stage.setTitle("Animation example");
		stage.show();
	}
}

```


```java
import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.shape.Rectangle;
import javafx.stage.Stage;
import javafx.util.Duration;

public class TextAnimationExample extends Application {
	@Override
	public void start(Stage stage) throws Error {
		Group group = new Group();

		// String str = "Sławek Dańczak";
		// Text text = new Text(100, 100, str);

		// Animation animation = new Transition() {
		//	{
		//		setCycleDuration(Duration.seconds(3);
		//	}
		//	@Override
		//	protected void interpolate(double v) {
		//		int length = str.length();
		//		int n = Math.round(length * (float) v);
		//		text.setText(str.substring(0, n));
		//	}
		//};
		// animation.play();
		// group.getChildren().add(text);

		Circle circle = new Circle(150, 150, 10);

		KeyValue rad0 = new KeyValue(circle.radiusProperty(), 10);
		KeyFrame fra0 = new KeyFrame(Duration.millis(0), rad0);
		
		KeyValue rad1 = new KeyValue(circle.radiusProperty(), 80);
		KeyFrame fra1 = new KeyFrame(Duration.millis(1000), rad1);
		
		KeyValue rad2 = new KeyValue(circle.radiusProperty(), 10);
		KeyFrame fra2 = new KeyFrame(Duration.millis(4000), rad2);

		Timeline timeline = new Timeline();
		timeline.getKeyFrames().addAll(fra0, fra1, fra2);
		timeline.setCycleCount(3);
		timeline.play();

		group.getChildren().add(circle);

		Scene scene = new Scene(group, 300, 300);
		stage.setScene(scene);
		stage.setTitle("Animation example");
		stage.show();
	}
}
```

## GUI_PL_19.08 CSS

```java
import javafx.application.Application;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

public class CSSExample extends Application {
	@Override
	public void start(Stage stage) throws Exception {
		GridPane root = new GridPane();
		Label label = new Label("JavaFX");
		Button button = new Button("Button");
		TextArea textArea = new TextArea("TextArea");

		// textArea.setStyle("-fx-background-color: yellow");

		root.add(label, 0,0);
		root.add(button, 0, 1);
		root.add(textArea, 0, 2);

		Scene scene = new Scene(root, 300, 275);
		scene.getStylesheets().add("./style.css");
		stage.setScene(scene);
		stage.setTitle("CSS Example");
		stage.show();
	}
}
```

`style.css` powinien być w `./src/resources`
```css
.text-area {
	-fx-background-color: yellow;
	-fx-text-fill: red;
}

.text-area .content {
	-fx-background-color: green;
}

.button {
	-fx-font-size: 20;
}

.label {
	-fx-font-family: 'Times New Roman';
	-fx-font-size: 18;
}
```

## GUI_PL_20.02 FactoryMethod

```java
public class Report {
	private String text;

	public Report(String text) {
		this.text = text;
	}

	public String toString() {
		return text;
	}
}
```

```java
public abstract class AbstractLocation {
	public abstract Report generateReport();
}
```

```java
public class WarsawLocation extends AbstractLocation {
	@Override
	public Report generateReport() {
		return new Report("Report from Warsaw");
	}
} 
```

```java
public class KrakowLocation extends AbstractLocation {
	@Override
	public Report generateReport() {
		return new Report("Report from Krakow");
	}
} 
```

```java
public class GdyniaLocation extends AbstractLocation {
	@Override
	public Report generateReport() {
		return new Report("Report from Gdynia");
	}
} 
```

```java

public class Main {

	public static void main(String[] args) {
		AbstractLocation a1 = new WarsawLocation();
		AbstractLocation a2 = new CracowLocation();
		AbstractLocation a3 = new GdyniaLocation();

		List<AbstractLocation> list = List.of(a1, a2, a3);

		for (AbstractLocation a: list)
			System.out.println(a.generateReport());
	}
}
```

## GUI_PL_20.03 Singleton

```java
public class Singleton {
	private static Singleton singleton;
	private Singleton() {
		// constructor body
	}

	public static Singleton getInstance() {
		if (singleton == null) {
			singleton = new Singleton();
		}

		return singleton;
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		Singleton s = Singleton.getInstance();
		Singleton s2 = Singleton.getInstance();

		System.out.println("Czy są tym samym: " + s.equals(s2));
	}
}
```

## GUI_PL_20.04 Flyweight

```java
public class SquareDimension {
	public int x;
	public SquareDimension(int x) {
		this.x = x;
	}
}
```

```java
import java.util.HashMap;
import java.util.Map;

public class SquareFactory {
	private static Map<Integer, SquareDimension> map = new HashMap<>();

	public static SquareDimension getSquareDimension(int length) {
		if (!map.containsKey(length)) {
			map.put(length, new SquareDimension(length));
		}
	
		return map.get(length);
	}

	public static void count() {
		System.out.println(map.size());
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		for(int i = 0; i < 1000; i++)
			SquareFactory.getSquareDimension((int)(Math.random()*1000);

		SquareFactory.count();
	}
}
```

## GUI_PL_20.05 Memento

```java
public class Orginator {
	private StringBuilder text;
	
	public Orginator() {
		text = new StringBuilder();
	}

	public void addText(String text) {
		this.text.append(text);
	}

	public Memento save() {
		return new Memento(text.toString());
	}

	public void restore(Memento memento) {
		text = memento.getText();
	}

	@Override
	public String toString() {
		return text.toString();
	}
}
```

```java
public class Caretaker {
	private List<Memento> mementoList = new LinkedList<>();

	public void addMemento(Memento memento) {
		mementoList.add(memento);
	}

	public Memento getMemento(int index) {
		return mementoList.get(index);
	}
}
```

```java
public class Memento {
	private StringBuilder text;

	public Memento(String text) {
		this.text = new StringBuilder(text);
	}	

	public StringBuilder getText() {
		return text;
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		Caretaker caretaker = new Caretaker();
		Orginator orginator = new Orginator();
		orginator.addText("Joanna ");
		caretaker.addMemento(orginator.save());
		orginator.addText("has ");
		caretaker.addMemento(orginator.save());
		orginator.addText("a ");
		caretaker.addMemento(orginator.save());
		orginator.addText("cat.");
		caretaker.addMemento(orginator.save());

		System.out.println(orginator);
		orginator.restore(caretaker.getMemento(2));
		System.out.println(orginator);
		orginator.restore(caretaker.getMemento(0));
		System.out.println(orginator);
	}
}
```
