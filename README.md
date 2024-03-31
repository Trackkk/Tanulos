# A Java SE/JDK új lehetőségei:


## **Boilerplate kód fogalma:**
- A boilerplate kód ismétlődő, sablonkódot jelent, ami szintaxisbeli követelményeknek megfelelő, de nincs valós tartalma. Látszik, hogy sokkal egyszerűbbnek kellene lennie. A Java fejlődése során ezt a felesleges kódot igyekeznek minimalizálni. 

## **Előzetes lehetőség (preview feature):**
- Az előzetes lehetőségek három fajtája: előzetes nyelvi lehetőségek, előzetes VM lehetőségek, előzetes API-k. 
	- pontosan meghatározott, teljesen implementált, de még nem végleges. 
	- soha nem kísérleti, kockázatos, hiányos vagy instabil.
	- A JDK parancssori eszközöknek a --enable-preview parancssori opciót kell megadni az előzetes lehetőségek engedélyezéséhez. 
	
	
## **Lokális változók kikövetkeztetés:**
- A Java fordító a változó típusát fordítási időben következteti ki, ami növeli a kód olvashatóságát és rövidítheti a fejlesztési időt. E  folyamat lényegében az inicializáló kifejezés típusát adja a változónak.
- A nem null kezdőértékű lokális változók típus megadása nélkül deklarálhatók a "var" azonosítóval.
- A "var" azonosító nem kulcsszó, hanem egy fenntartott típusnév. A "var" használható változó, metódus vagy csomag neveként. Pl.: hagyományos- vagy for-each ciklusban, try-with-resources.

	- Helyes:
	
			var i = 0;
			var numberOfItems = 0L;
			var epsilon = 1e-10;
			var sum = 0.0f;
			var failOnError = false;
			var greeting = "Hello, World!\n";
			var tags = new HashSet<String>();
			var lines = Files.readAllLines(Path.of("file.txt"));
		
		
	- Helytelen(Hibát okoz):
	
			var a = 1, b = 2;
			var[] c = new int[5];
			var d = {1, 2, 3};
			var e = (int x, int y) -> x + y;
			var f = System::currentTimeMillis;
			var g = System.out::println;
	
	
	
## **Switch utasítás/kifejezések:**
- Minden lehetséges esethez kell, hogy legyen egy illeszkedő címke, azaz default záradék szükséges kivéve, az összes enum konstanst lefedő enum switch kifejezéseknél.
- Switch kifejezésnek vagy normális módon egy értékkel kell befejeződnie, vagy váratlanul egy kivétel dobásával.
- A switch címke egy új formája (case ... ->) került bevezetésre, mely esetenként több vesszővel elválasztott konstanst is megenged. 
- Ha egy címke illeszkedik, akkor csak a nyíl jobb oldalán lévő kifejezés vagy utasítás kerül végrehajtásra, nincs “átesés”
- "Yield" utasítás egy érték egy switch blokkból történő visszaadására.
	
		int j = switch (day) {
			case MONDAY -> 0;
			case TUESDAY -> 1;
			default -> {
				int k = day.toString().length();
				int result = f(k);
				yield result;
			}
		};
	
		enum Season {
			SPRING,
			SUMMER,
			AUTUMN,
			WINTER;
			public static Season of(java.time.Month month) {
				return switch (month) {
					case MARCH, APRIL, MAY -> SPRING;
					case JUNE, JULY, AUGUST -> SUMMER;
					case SEPTEMBER, OCTOBER, NOVEMBER -> AUTUMN;
					case DECEMBER, JANUARY, FEBRUARY -> WINTER;
				};
			}
		}

## **Szövegblokkok:**
- Egy szövegblokk egy többsoros sztring literál, mely bárhol használható, ahol egy közönséges sztring literál.  Pl.: 
	
	- Régi stílusú inicializálás:
	
			String html = "<!DOCTYPE html>\n" +
			"<html lang=\"en\">\n" +
				" <head>\n" +
					" <meta charset=\"utf-8\">\n" +
					" <title>Hello, World!</title>\n" +
				" </head>\n" +
				" <body>\n" +
					" <p>Hello, World!</p>\n" +
				" </body>\n" +
			"</html>\n";
		
		
	- Új stílusú inicializálás szöveggblokk segítségével:
	
			var html = """
				<!DOCTYPE html>
				<html lang="en">
					<head>
						<meta charset="utf-8">
						<title>Hello, World!</title>
					</head>
					<body>
						<p>Hello, World!</p>
					</body>
				</html>
				""";
		
	
## **Mintaillesztés az instanceof operátorhoz:**
- Lehetővé teszi egy programban komponensek objektumokból történő feltételes kinyerésének tömörebb és biztonságosabb kifejezését.
- Egy típus minta egy típust meghatározó predikátumból és egyetlen minta változóból áll.  Az instanceof operátor úgy lett kiterjesztve, hogy csupán egy típus helyet egy típus mintát kapjon.
- Általában típusellenőrzésre vagy típuskonverzióra használjuk.

		Az instanceof-and-cast idióma:
		if (obj instanceof String) {
			String s = (String) obj;
			// ...
		}
	
	
## **Rekord osztályok:**
- A java.lang.Record osztály alosztályai.
- Nem módosítható adatokat becsomagoló újfajta osztályok. A rekord példányok rekord komponenseknek nevezett rögzített értékek egy halmazát ábrázolják.
- Egy rekord osztálynak minden egyes komponenséhez van egy implicit módon deklarált lekérdező metódusa. Van implicit módon deklarált konstruktora, equals(), hashCode() és toString() metódusa is
	- Pl.:
 
			record LegoSet(String number, Year year, int pieces) {}
			var legoSet = new LegoSet("75211", Year.of(2018), 519);
			System.out.println(legoSet.number()); // 75211
			System.out.println(legoSet.year()); // 2018
	
	
## **Sztring sablonok:**
- Számos programozási nyelv (pl.: a C#, JavaScript, Python)  teszi lehetővé a sztring interpolációt a sztring összefűzés alternatívájaként. 
- Az interpoláció kifejezések sztring literálokba történő beágyazását jelenti, melyek automatikusan kiértékelésre és az értékükkel való helyettesítésre kerülnek.
- Az interpoláció veszélyes lehet, mivel ki van téve a befecskendezéses támadásoknak.
- Sablon kifejezések
	
		var width = 1024;
		var height = 768;
		var s = STR."\{width} * \{height} = \{width * height}";
		System.out.println(s);
		
		var r = 3.0;
		var s = FMT."""
		{
		"circle": {
		    "radius": %.4f\{r},
		    "area": %.4f\{r * r * Math.PI},
		    "perimeter": %.4f\{2 * r * Math.PI}
		  }
		}
		""";
		System.out.println(s);
	


# A Java haladó szintű lehetőségei:
   
## **Nem absztrakt (alapértelmezett, statikus, privát) interfész metódusok:**

- Probléma: hogyan adhatók hozzá új metódusok egy már létező interfészhez? 
	
- Megoldás: az alapértelmezett és statikus interfész metódusok úgy teszik lehetővé új metódusok hozzáadását egy interfészhez, hogy azok automatikusan rendelkezésre állnak minden implementációban. (Ráadásul ezen metódusok hozzáadása nem igényli a létező implementációk módosítását vagy újra fordítását. Ezt bináris kompatibilitásnak nevezik.)

- Implicit módon absztrakt minden olyan interfész metódus, melynek nincs private, default vagy static módosítója.

- Default: A metódustörzs a metódus implementációját szolgáltatja az interfészt a metódus felülírása nélkül implementáló osztályok számára.  
	- Amikor egy interfész kiterjeszt egy alapértelmezett metódust tartalmazó interfészt, akkor a következőket teheti:
		– Egyáltalán nem említi az alapértelmezett metódust, mely azt jelenti, hogy örökli azt.
		– Újradefiniálhatja a metódust, felülírva azt.
		– Absztraktként deklarálhatja újra a metódust, mely a felülírására kényszeríti az implementáló osztályokat.


	- Hasonlóan, amikor egy osztály implementál egy alapértelmezett metódust tartalmazó interfészt, akkor a következőket teheti:
		– Egyáltalán nem említi az alapértelmezett metódust, mely azt jelenti, hogy örökli azt.
		– Újradefiniálhatja a metódust, felülírva azt.
		– Absztraktként deklarálhatja újra a metódust, mely a felülírására kényszeríti az alosztályokat. (Ez a lehetőség csak akkor adott, ha az osztály absztrakt.)
		
		
- Statikus:  A statikus interfész metódusokat nem öröklik az alinterfészek. Lehetővé teszik egy interfészhez kötődő konkrét segédmetódusok hozzáadását közvetlenül magához az interfészhez.
		Fordítási hiba egy statikus metódus törzsében a "this" vagy a "super" kulcsszó előfordulása. 
		
		
- Private:  private módosító kombinálható a static módosítóval. A privát interfész metódusokat nem öröklik az alinterfészek.
	

## **java.util.Optional:**
Egy konténer objektum, mely vagy tartalmaz egy nem null értéket, vagy nem.
Elsődlegesen olyan metódusok visszatérési típusaként szolgál, melyeknél egyértelműen szükséges a „nincs eredmény” ábrázolása és ahol null használata valószínűleg hibát okoz.
Egy Optional típusú változó értéke soha nem szabad, hogy null legyen, mindig egy Optional példányra kell, hogy mutasson.
	
	
## **Funkcionális interfészek:**
Egy olyan interfész, melynek csak egy absztrakt metódusa van. De akár több alapértelmezett, statikus és/vagy privát metódusa is lehet.
	

## **Beépített funkcionális interfészek:**
- Consumer:  java.util.function.Consumer<T>
				- Egyetlen input argumentumot vár és nem ad vissza eredményt  és várhatóan mellékhatást fejt ki
				- Funkcionális metódusa: void accept(T t).
				- Nem absztrakt metódusai:
					andThen(after): egy összetett Consumer-t ad vissza, mely először a példány által ábrázolt műveletet hajtja vége, majd az after műveletet



- Function: java.util.function.Function <T, R>
				- Egy eredményt létrehozó egyargumentumú függvényt ábrázol.
				- Funkcionális metódusa: R apply(T t).
				- Nem absztrakt metódusai:
					andThen(after): visszaad egy összetett függvényt, mely először a példány által ábrázolt függvényt alkalmazza a bemenetére, majd az after függvényt az eredményre. 
					compose(before): visszaad egy összetett függvényt, mely először a before függvényt alkalmazza a bemenetére, majd a példány által ábrázolt függvényt az eredményre. 
					identity(): visszaad egy, mindig az argumentumát visszaadó függvényt. 


- Predicate: java.util.function.Predicate<T>
				- Egy egyargumentumú predikátumot (logikai értékű függvényt) ábrázol.
				- Funkcionális metódusa: boolean test(T t). 
				- Nem absztrakt metódusai:
					and(other), or(other): visszaad egy összetett predikátumot, mely a példány és az other predikátum logikai konjunkcióját/diszjunkcióját ábrázolja.
					negate(): visszaad egy, a példány logikai negáltját ábrázoló predikátumot.


- Supplier:  java.util.function.Supplier<T>
				- Egy eredményeket szolgáltató objektumot ábrázol. 
				- Funkcionális metódusa: T get().
				- Nem absztrakt metódusai: nincsenek

## **Lambda kifejezések:**
- Egy funkcionális interfészt implementáló névtelen belső osztály egy példányát ábrázolják nagyon tömören. 

		new VmilyenFunkcionálisInterfész() {
			@Override
			VmilyenTípus vmilyenMetódus(paraméterek) {
				törzs
			}
		}
- Vele ekvivalens lambda kifejezés:

		 (paraméterek) -> {törzs}
	

## **Metódus referenciák:**
- Egy metódus referencia arra szolgál, hogy egy metódushívásra hivatkozzunk anélkül, hogy ténylegesen hívás történne.  Egy metódus referencia kifejezés kiértékelése egy funkcionális interfésztípus egy példányát hozza létre.

- Lambda: Function<String, Integer> stringLength = (str) -> str.length();
- Metódus referencia: Function<String, Integer> stringLength = String::length;
	
		IntSupplier supplier = "Hello, World!"::length;
		System.out.println(supplier.getAsInt()); // 13
		
		Consumer<Object> consumer = System.out::println;
		consumer.accept("Hello, World!"); // Hello, World!
		consumer.accept(42); // 42
		consumer.accept(LocalDate.now()); // 2021-03-07
	
	
		
## **Streamek:**
- Egy stream elemek egy sorozata, melyen műveletek végezhetők. Nincs mögöttük tárhely. Egy művelet egy streamen egy eredményt hoz létre, de nem módosítja a stream forrását. Streameket létre lehet hozni: Kollekciókból, Tömbökből, Egyedi objektumokból. 
	

## **Stream műveletek:**
- Köztes:  egy új streamet adnak vissza, Példák: filter(), map(), sorted()
		
- Terminális: Egy streamtől különböző eredményt hoznak létre vagy mellékhatást eredményeznek. Tehát void vagy nem stream visszatérési típusúak. Példák: count(), max(), forEach()
		
- Állapotmentes: Minden egyes elem a többi elemen történő műveletektől függetlenül dolgozható fel. Példák: filter(), map()
		
- Állapotőrző műveletek: Felhasználhatnak a korábban látott elemekből állapotot, amikor új elemeket dolgoznak fel. Példák: distinct(), sorted()
	

## **Streamek: viselkedési paraméterek kívánatos jellemzői (interferencia-mentesség, állapotmentesség):**
- Interferencia-mentesség: A viselkedési paraméterek nem szabad, hogy módosítsák a stream adatforrását.
- Állapotmentesség: Egy lambda kifejezés állapotőrző, ha eredménye olyan állapottól függ, mely a stream csővezeték végrehajtása során megváltozhat. 

## **Stream csővezetékek, műveletek kiértékelése, csővezetékek végrehajtása:**
- Csővezetékek: Stream műveletek egy csővezetékké láncolhatók össze, mely egy forrásból áll, melyet nulla vagy több köztes művelet és egy terminális művelet követ.
- Műveletek kiértékelése: A köztes műveletek mindig lusta kiértékelésűek. Egy köztes művelet végrehajtása nem eredményez műveletvégzést. A terminális műveletek majdnem minden esetben mohó kiértékelésűek. Egy terminális művelet végrehajtása indítja el az adatforrás bejárását, a csővezeték feldolgozása a visszatérés előtt fejeződik be.
- Csővezetékek végrehajtása: Vertikálisan történik, ami csökkentheti az elemeken végrehajthandó műveletek számát. 
A terminális művelet végrehajtása során a csővezeték elhasználódik és nem használható többé.


## **Streamek: redukciós műveletek (reduce, collect):**
- egy terminális művelet, mely egy input elemsorozatból egyetlen összesítő eredményt képez egy egyesítő művelet ismételt alkalmazásával.
- Reduce: elemek egy sorozatát egyetlen elemre redukálja.
	- *Egységelem (identity):* a redukció kezdőértéke, alapértelmezett értéke, ha nincsenek input elem
	- *Akkumulátor (accumulator):* egy függvény, mely két paramétert kap (a redukció egy részleges eredményét, következő elemet) -> új részleges eredményt állít elő.
	- *Egyesítő (combiner):* egy kétparaméteres függvény, mely két részleges eredményt kap -> egy új részleges eredménnyé egyesíti. 
- A reduce() műveletnek három formája van: 
	reduce(akkumulátor)
	reduce(egységelem, akkumulátor)
	reduce(egységelem, akkumulátor, egyesítő)
- Collect:  egy módosítható eredmény konténerbe (pl. egy kollekció vagy egy StringBuilder) gyűjti össze az input elemeket a stream elemeinek feldolgozásakor
	- *Ellátó (supplier):* egy új módosítható eredmény konténert létrehozó függvény
	- *Akkumulátor (accumulator):* egy konténerbe egy elemet helyező függvény.
	- *Egyesítő (combiner):* egy függvény, mely két részleges eredmény konténert fésül össze, a második konténer elemeit az első konténerbe helyezi
	- *Befejező (finisher):* egy végső transzformációt végez egy eredmény konténeren
	- *Gyűjtő (collector):* egy ellátó, egy akkumulátor, egy egyesítő és egy opcionális befejező alkotja.
	- A collect() műveletnek két formája van:
		- collect(ellátó, akkumulátor, egyesítő)
		- collect(gyűjtő)
			
	
## **Streamek: párhuzamosság:**
- Stream megvalósításai szekvenciális streameket hoznak létre, hacsak nem kérünk kifejezetten párhuzamosságot [ stream(), parallelStream() ]. A findAny()) kivételével egy számítás eredményét nem befolyásolja az, hogy szekvenciális vagy párhuzamos. 

- Így kaphatunk párhuzamos streamet:
	- kollekció parallelStream(), már létező szekvenciális stream parallel().
	- A párhuzamos streamek egy megosztott szálkészletet használnak, mely a ForkJoinPool.commonPool() statikus metódushívásával kapható meg. 
	
	
- Gonosz számok megszámolása: a párhuzamos streammel gyorsabb mint a szekvenciális streammel.
	A számok listába gyűjtése: ekkor a szekvenciális stream gyorsabb.
	A gonosz számok számolása hagyományos for ciklussal :  a szekvenciális stream gyorsabb.
