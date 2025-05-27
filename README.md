# 1. Hogyan hozunk létre dinamikusan egy Button-t? Mutasd be kóddal! Klikk eseményt rendeljünk hozzá! (WINFORMS)

```csharp
private Button CreateNewButton(Control control, string name, string text, Point location, Size size, Dictionary<string, EventHandler> events)
{
    Button newButton = new Button
    {
        Name = name,
        Text = text,
        Location = location,
        Size = size
    };

    foreach (var e in events)
    {
        EventInfo eventInfo = typeof(Button).GetEvent(e.Key);
        if (eventInfo != null) eventInfo.AddEventHandler(newButton, e.Value);
        else MessageBox.Show($"No such event found: {e.Key}");
    }

    control.Controls.Add(newButton);
    return newButton;
}
```

---

# 2. Sorold fel a tanult 5 speciális adatszerkezetet!

---

# 3. Mutasd be a speciális adatszerkezetek deklarálását (példányosítását) és jellemezd őket!

```csharp
/// <List>
/// Bármennyi elemet tárolhat, sorrendet tart.
/// Hozzáadás: list.Add(item)
/// </List>
List<int> numbers = new List<int>();

/// <Stack>
/// Az utoljára betett elem jön ki elsőként (Last-In, First-Out)
/// Hozzáadás: Push()
/// Kivétel: Pop()
/// Visszaadás: Peek()
/// </Stack>
Stack<string> stack = new Stack<string>();

/// <Queue>
/// Az elsőként betett elem jön ki elsőként (First-In, First-Out)
/// Hozzáadás: Enqueue()
/// Visszaadás és kivétel: Dequeue()
/// Visszaadás kivétel nélkül: Peek()
/// </Queue>
Queue<string> queue = new Queue<string>();

/// <HashSet>
/// Csak egyedi elemeket tárol.
/// Nincs sorrend, nincs duplikáció.
/// </HashSet>
HashSet<int> halmaz = new HashSet<int>();

/// <Dictionary>
/// Kulcs–érték párokat tárol.
/// Gyors keresés kulcs alapján.
/// dictionary["key"]
/// </Dictionary>
Dictionary<string, int> dictionary = new Dictionary<string, int>();
```

---

# 4. Mi az az öröklődés? Mi a feladata?

**Definíció:**  
Az öröklődés (angolul: inheritance) egy objektumorientált programozási (OOP) eszköz, amely lehetővé teszi, hogy egy új osztály (gyermek/leszármazott) átvegye egy másik osztály (szülő/alaposztály) mezőit, metódusait és viselkedését.

**Feladata / Célja:**
- Újrafelhasználhatóság – Kód újrahasznosítása már meglévő osztályból.
- Hierarchia kialakítása – "Is-a" kapcsolatot modellez („egy diák egy ember”).
- Bővíthetőség – Új funkciók hozzáadása anélkül, hogy az eredeti osztályt módosítanánk.
- Egységesség – Több osztály azonos alap viselkedést követhet.

---

# 5. Készítsünk kódszinten egy Etel osztályt 3 tulajdonsággal: súly, név és ár, konstruktorral!

# 6. Származtassunk az Etel osztályból egy gulyas osztályt és legyen saját tulajdonsága 2 db: -csípős, -magyaros!

```csharp
class Food
{
    public double Weight { get; private set; }
    public string Name { get; private set; }
    public double Price { get; private set; }

    public Food(double weight, string name, double price)
    {
        this.Weight = weight;
        this.Name= name;
        this.Price= price;
    }
}

class Gulyas : Food
{
    public bool Csipos { get; private set; }
    public bool Magyaros { get; private set; }

    public Gulyas(double weight, string name, double price, bool csipos, bool magyaros) : base(weight, name, price)
    {
        this.Csipos = csipos;
        this.Magyaros = magyaros;
    }
}
```

---

# 7. Mi az a metódus felülbírálás!

**Definíció:**  
A metódus felülbírálás (angolul: method overriding) azt jelenti, hogy egy származtatott (gyermek) osztály újra megvalósítja az alaposztályban már meglévő virtuális metódust, így a viselkedést testre szabhatja.

**Feladata / Célja:**
- Lehetővé teszi a polimorfizmust.
- Különböző osztályok ugyanazzal a metódusnévvel másképp viselkedhetnek.
- Segíti a bővíthetőséget, anélkül hogy módosítani kéne az alaposztályt.

---

# 8. Milyen módon tudunk metódust felülbírálni, mutasd be azt a 2 féle megvalósítást!

```csharp
class Parent
{
    /// <RealOverride>
    /// Ez az ajánlott és helyes módszer, amely az öröklés és a polimorfizmus alapja.
    /// </RealOverride>
    public virtual void RealOverride() {}

    /// <Redefining>
    /// Ez nem valódi felülbírálás, hanem az alaposztály metódusának elrejtése.
    /// Nem támogatja a polimorfizmust.
    /// </Redefining>
    public void Redefining() { }
}

class Child : Parent
{
    public override void RealOverride() { }
    public new void Redefining() { }
}
```

---

# 9. Mit nevezünk futás idejű hibának?

**Definíció:**  
A futás idejű hiba (runtime error) olyan hiba, amely a program futása közben jelentkezik – tehát a fordító nem jelzi előre –, és általában megszakítja a program végrehajtását.

**Jellemzők:**
- A program lefordul, de futtatás közben hibát dob.
- Gyakran kivétel (exception) formájában jelentkezik.
- Megfelelő kivételkezeléssel (try-catch) elkerülhetők a programleállások.

**Szerkezete:**
- `try` → műveleti blokk
- `catch` → hibakezelési blokk
- `finally` → mindig lefutó blokk

---

# 10. Mutasd be a kivételkezelés formáját kód formájában, jelöld melyik blokkja mire való!

```csharp
public string ReadFile(string filePath)
{
    StreamReader reader = null;
    try
    {
        reader = new StreamReader(filePath);
        string content = reader.ReadToEnd();
        return content;
    }
    catch (FileNotFoundException)
    {
        MessageBox.Show("Error", "File not found!" + filePath, MessageBoxButtons.OK);
    }
    catch (UnauthorizedAccessException)
    {
        MessageBox.Show("Error", "You do not have permission to open the file.", MessageBoxButtons.OK);
    }
    catch (IOException ex)
    {
        MessageBox.Show("Error", "General I / O error: " + ex.Message, MessageBoxButtons.OK);
    }
    finally
    {
        if (reader != null)
        {
            reader.Close();
            MessageBox.Show("Info", "File closed!", MessageBoxButtons.OK);
        }
    }

    return string.Empty;
}
```

---

# 11. Mikor van szükségünk kivétel dobásra?

Ha olyan hiba vagy rendellenes állapot lép fel a programban, amelyet nem lehet vagy nem érdemes helyben kezelni, hanem a vezérlést egy külső (felsőbb) szintnek kell átadni, hogy ott megfelelő módon reagáljanak rá.

---

# 12. Mutasd be kóddal egy feltételhez kötött hibadobást!

```csharp
public void SetAge(int age)
{
    if (age < 0) throw new ArgumentException("Age cannot be negative!");
}
```

---

# 13. Mi az az absztrakt osztály, kifejtősebben?

**Definíció:**  
Egy absztrakt osztály olyan osztály C#-ban, amelyet nem lehet példányosítani (nem hozhatunk létre belőle objektumot), és alapul szolgál más osztályok számára.  
Olyan közös alapviselkedést és szerkezetet határoz meg, amelyet a belőle származó osztályok (leszármazottak) valósítanak meg.

**Fő Jellemzők:**
- Nem példányosítható
- Megvalósított metódusokat és absztrakt (nem megvalósított) metódusokat is tartalmazhat
- Bázisosztályként szolgál más osztályok számára
- Közös alap biztosítása és kényszer a leszármazott osztályokra bizonyos metódusok megvalósítására

```csharp
public abstract class AbstractClass
{
    public abstract string AbstractField();
    public abstract void AbstractMethod();
    public void KnownMethod() { /* do something... */ }
}
```

---

# 14. Jellemezd az absztrakt osztály absztrakt metódusait, függvényeit?

**Definíció:**  
Egy absztrakt metódus (függvény) olyan metódus egy absztrakt osztályban, amelynek nincs törzse, csak a fejlécét definiáljuk.  
A metódus megvalósítását (testét) a leszármazott osztálynak kötelezően meg kell valósítania.

**Jellemzők:**
- Nem használható nem absztrakt osztályban
- Nincs törzse
- Kötelező felülírni
- Polimorf viselkedést támogat

---

# 15. Mi az a lezárt osztály, mire használjuk?

**Definíció:**  
A lezárt osztály (sealed class) egy olyan osztály, amelyből nem lehet örökölni. Ez azt jelenti, hogy nem lehet belőle származtatott osztályt létrehozni.

**Cél:**
- Biztonság (Megakadályozza az osztály nem kívánt bővítését vagy módosítását örökléssel)
- Teljes implementáció (A fejlesztő garantálja, hogy az osztály végleges, nem kell tovább testreszabni)
- Teljesítményoptimalizálás (A JIT fordító gyorsabb lehet sealed osztályokkal, mert nem kell dinamikus késleltetett felülbírálást kezelnie)
- API stabilitás (Nyilvános könyvtárakban gyakori, hogy az osztályt lezárják a hibás öröklés elkerülése érdekében)

**Gyakori használat:**
- Helper / Utility osztályoknál (Math, Logger, Validator, stb...)
- Biztonsági vagy logikai egységeknél
- Frameworkök belső osztályainál

---

# 16. Sorold fel a S.O.L.I.D. elveket és jellemezd röviden!

| Betű | Név                             | Jelentés                                          |
| ---- | ------------------------------- | ------------------------------------------------- |
| S    | Single Responsibility Principle | Egy osztály – egy felelősség                      |
| O    | Open/Closed Principle           | Nyitott bővítésre, zárt módosításra               |
| L    | Liskov Substitution Principle   | Bármely leszármazott behelyettesíthető            |
| I    | Interface Segregation Principle | Csak azt az interfészt használd, amit kell        |
| D    | Dependency Inversion Principle  | Felső szintű modul ne függjön közvetlenül alsótól |

---

# 17. Mi a különbség a metódus és a függvény között?

**Function:**
- Egy önálló kódrészlet, amely bemeneti paramétereket fogad, és általában visszaad egy értéket.
- Nem kapcsolódik közvetlenül egy objektumhoz vagy osztályhoz.

**Method:**
- Egy osztály vagy objektum tagjaként definiált függvény.
- Egy objektum viselkedését írja le.

**Különbség:**  
A metódus egy osztály része, míg a függvény lehet önálló is.  
A metódus a fő építőelem, és minden függvény egy metódus formájában valósul meg.

---

# 18. Milyen alapvető elemei vannak egy osztálynak?

- field
- property
- method
- constructor (1 vagy több)
- destructor
- static members

**Fields:**
- Az osztály állapotát tárolják.
- Változók, amelyek jellemzik az adott objektumot.

**Properties:**
- A mezők szabályozott elérését biztosítják.
- Általában get és set metódusokat használnak.

**Methods:**
- Az osztály viselkedését határozzák meg.
- Olyan függvények, amelyek az osztályon belül működnek.

**Constructor(s):**
- Egy speciális metódus, amely az osztály példányosításakor fut le.
- Beállíthatja az alapértékeket.

**Destructor:**
- A példány megszűnésekor hívódik meg.
- C# esetén ritkán használjuk, helyette van garbage collector.

**Static Members:**
- Nem az objektumhoz (példányhoz), hanem az osztályhoz tartoznak.

```csharp
class TemplateClass
{
    private int field;
    public int Property { get; set; }
    public static int StaticMember;
    public void Method() { }
    public TemplateClass() { }
    public TemplateClass(int property) { this.Property = property; }
    ~TemplateClass() { }
}
```

---

# 19. Sorold fel az Objektum Orientált Programozás 6 fő kritériumát!

**Class:**  
Az osztály egy sablon, amely leírja, milyen adatokat (mezők, property-k) és viselkedéseket (metódusok) tartalmaz egy objektum.

**Object:**  
Egy objektum az osztály példánya, egy konkrét "példány", amely saját adattal rendelkezik. Az objektum az, amivel ténylegesen dolgozunk futás közben.

**Abstraction:**  
Az absztrakció azt jelenti, hogy csak a lényeges információkat emeljük ki, és elrejtjük a részleteket.  
Például: autó vezetésekor nem érdekel, hogyan működik a motor, csak a kormányt és a pedálokat használjuk.

**Inheritance:**  
Az öröklődés lehetővé teszi, hogy egy osztály átvegye egy másik osztály tulajdonságait és metódusait.  
Ezáltal kódújrafelhasználás és strukturált felépítés valósítható meg.

**Polymorphism:**  
A polimorfizmus lehetővé teszi, hogy ugyanaz a metódusnév különbözőképpen viselkedjen az objektum típusa alapján.  
Method override/overload

**Encapsulation:**  
A kapszulázás során az adatok el vannak rejtve az osztályon belül, és csak ellenőrzött módon, property-ken/metódusokon keresztül érhetők el.

---

# 20. Miben hasonlítanak és miben különbözik a lista és a tömb?

| Tulajdonság        | Tömb (`Array`)                              | Lista (`List<T>`)                                             |
| ------------------ | ------------------------------------------- | ------------------------------------------------------------- |
| **Méret**          | Fix méretű (példányosításkor meg kell adni) | Dinamikus, tetszőlegesen bővíthető vagy csökkenthető          |
| **Deklaráció**     | `int[] nums = new int[5];`                  | `List<int> nums = new List<int>();`                           |
| **Bővíthetőség**   | Nem lehet `Add()`-dal bővíteni              | `Add()`, `Remove()` stb. használható                          |
| **Funkcionalitás** | Egyszerű, kevés beépített metódus           | Sok beépített metódus: `Add()`, `Remove()`, `Contains()` stb. |
| **Típus**          | Nem generikus típus (`int[]`)               | Generikus típus (`List<int>`)                                 |
| **Teljesítmény**   | Gyorsabb, ha ismert a méret                 | Rugalmasabb, de kicsit lassabb lehet                          |
