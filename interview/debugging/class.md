# Debuggolas 101

### Szukseges eszkozok:
- [java 21](https://www.oracle.com/java/technologies/downloads/#jdk21-windows) 
- [IntelliJ IDEA Community Edition](https://www.jetbrains.com/idea/download/?section=windows)

## Bevezetes
Szoftverfejlesztes soran elkerulhetetlen, hogy elerkezzunk egyszer arra pontra, hogy a kodunk egyszeruen nem ugy mukodik mint ahogy azt mi szeretnenk.
- Lehet, hogy nem fordul, mert szintaktikai hibat vetettunk 
- Vagy esetleg valami erdekes tobb szaz soros hiba uzenettel talaljuk szembe magunkat
- Lehet, rosszul valositottuk meg a logikat ezert rossz eredmenyeket ad
- Vagy az is elofordulhat, hogy soha nem all meg az algoritmus amit irtunk

Ilyenkor mielott tovabb lepnenk el kell haritanunk a hibat (debugolnunk kell a programunkat). Megtanulni hibat keresni es javitani egy elengedhetetlen tudas, ha szoftverfejlesztessel foglalkozunk. De nem kell megijedni, mert a debugolas sem agysebeszet (meg ha neha ugy is tunhet). Az ora soran vegig fogunk venni lepeseket es kerdeseket amiket, ha kovetunk es megvalaszolunk nagyvaloszinuseggel fogunk megoldast talalni a problemara.


## 0. Lepes: Nyugi

Amikor dolgozunk egy probleman, de a programunk nem az elvart eredmeny adja jo otletnek tunhet nekiallni azonnal modositani az erintett kodreszletet. Ezzel mindaddig nincs baj amig ranezesre meg tudjuk mondani a hiba forrasat "hopp kimaradt egy pontosvesszo" stb... 

Viszont, ha tenyleg egyaltalan nem ertjuk mi tortenik es mi okozhatja a hibat alljunk meg inkabb es ne rakjunk bele plusz bugokat a kodba. Mert akkor mar nem csak az eredeti, de az esetlegesen ujonnan hozzaadott hibat is javithatjuk (been there, done that). 

Felmerulhet a kerdes, hogy jo de akkor mit csinaljak. Kovessuk az alabbi lepeseket es nagy valoszinuseggel mire a vegere erunk, lesz megoldasunk.

## 1. Lepes: Ismerjuk fel pontosan mi a problema

Az elso kerdes amit fel kell tennunk magunknak: 
> Pontosan mi a problema amibe utkozunk?

Ha szerencsesek vagyunk a hiba beleesik az alabbi kategoriak egyikebe
- **Forditasi hiba**: A program le sem fordul.
- **Program crash**: A program elindul, de egy ponton crashel.
- **Vegtelen ciklus**: A program elindul, de soha nem all meg.
- **Logikai hiba**: Rossz logikat valositottunk meg ezert rossz eredmenyeket kapunk.

Miutan sikerult felismerni, hogy pontosan milyen hibat produkal a kodunk, lokalizalnunk kell, hogy hol tortenik.


## 2. Lepes: Lokalizaljuk a bugot
A masodik fontos kerdes:
> Pontosan hol tortenik a hiba?

Erre az alabbi eszkozoket hasznalhatjuk.

### Nezzuk at a kodot
Atnezzuk a kodot, hatha valami trivialis dolog okozza a hibat. Pl. figyelmetlenek voltunk es nem annak a valtozonak adtunk erteket aminek szerettunk volna.

### Logolas
Nagyon egyszeru, de annal hasznosabb eszkoz hiba keresesre. A segitsegevel keszithetunk egy timeline-t amit vegig tudunk kovetni. 

### Debugger 
A debugger egy olyan eszkoz ami lehetove teszi szamunkra, hogy sorrol sorra vegrehajtsuk az utasitasokat a programunkban. Segitsegevel betekinthetunk a program aktualis allapotaba. Meg tudjuk nezni a valtozok ertekeit, ki tudunk ertekelni kulonbozo muveleteket, el tudunk kapni vagy akar dobni exceptionoket.

> Hasznos link: [intellij debug tutorial](https://www.jetbrains.com/help/idea/debugging-code.html#refreshers
)

### Gumikacsa
Ez nem egy technikai tool amit hasznalhatunk hiba keresesre. De rengeteget tud segiteni, ha valakinek elmondjuk a problemat hangosan, kozben vegig gondoljuk az egesz folyamatot. A gumikacsa lehet akar egy ember is, munkatars vagy osztalytars, de mar az is rengeteget segithet, ha magunkban elismeteljuk mi tortenik.

### Tesztek
Ha vannak tesztek amik az erintett kod reszletet tesztelik akkor futtassuk le oket es nezzuk meg hatha eltornek valahol. Vegyuk figyelembe, hogy a tesztek attol fuggetlenul lefuthatnak, hogy fennall egy hiba.

# Feladatok
Talaljuk meg a hibakat az alabbi kod reszletekben az ismertetett eszkozok segitsegevel es javitsuk ki.
### 1. feladat:

### Lepesek:
1. Nezzuk at a kodot, hatha talalunk valami kezenfekvo szintaktikai hibat. 
    
    > Hmmm minden jonak tunik

2. Inditsuk el nezzuk meg mi tortenik.

    > Erdekes a program elindult, de valahol utkozben megadta magat, de mar tudjuk, hogy pontosan mi a hiba: `ArrayIndexOutOfBoundsException`
3. Lokalizaljuk a hibat

    > Szerencsenk van, mert tudjuk az exception tipusat. Igy meg tudjuk azt csinalni, hogy beallitunk egy breakpointot ami akkor all meg, ha a kodunk eldobja az adott exceptiont ([IntelliJ tutorial](https://www.jetbrains.com/help/idea/using-breakpoints.html#exception-breakpoints)). 
4. Ha megvan, hogy mi tortenik es hol, javitsuk ki a hibat.

```java
public class DebugAverage {

    public static void main(String[] args) {
        int[] numbers = {1,2,3,4,5,6};
        double average = calculateAverage(numbers);
        System.out.println("Average: " + average);
    }

    public static double calculateAverage(int[] numbers) {
        int sum = 0;

        for (int i = 0; i <= numbers.length; i++) {
            sum += numbers[i];
        }

        return sum / (numbers.length - 1);
    }
}
```

<details>
  <summary>Segitseg</summary>





### A kod ket hibat tartalmaz:


- A ciklus feltetel helytelen a `calculateAverage` fugvenyben: `i <= numbers.length` helyett `i < numbers.length`-nek kene lennie, igy `ArrayIndexOutOfBoundsException`-t fog dobni futas kozben.


- Az atlagot az egesz tomb hossza (`numbers.length`) alapjan kell szamitani, mert igy igy eggyel kevesebbel lenne elosztva az osszeg.
  ### megoldas
  ```java
  public static double calculateAverage(int[] numbers) {
        int sum = 0;

        for (int i = 0; i < numbers.length; i++) {
            sum += numbers[i];
        }

        return (double) sum / numbers.length;
    }
  ```
</details>


