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
