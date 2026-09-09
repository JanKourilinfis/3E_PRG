# C# konzole – výpis, vstup a parsování dat

## 1. Konzolová aplikace v C#

Jednoduchý C# program může vypadat například takto:

```csharp
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("Ahoj světe!");
    }
}
```

V novějších verzích C# je možné psát i kratší variantu:

```csharp
Console.WriteLine("Ahoj světe!");
```

`Main` je vstupní bod programu, podobně jako v Javě:

```java
public static void main(String[] args)
{
    System.out.println("Ahoj světe!");
}
```

---

## 2. Výpis do konzole

V C# se pro práci s konzolí používá třída:

```csharp
Console
```

Je součástí namespace:

```csharp
System
```

Proto často na začátku programu vidíme:

```csharp
using System;
```

### `Console.WriteLine()`

Vypíše text a přejde na nový řádek.

```csharp
Console.WriteLine("Ahoj");
Console.WriteLine("Jak se máš?");
```

Výstup:

```text
Ahoj
Jak se máš?
```

Porovnání s Javou:

```text
Java                          C#

System.out.println()    ->    Console.WriteLine()
```

### `Console.Write()`

Vypíše text, ale nepřejde na nový řádek.

```csharp
Console.Write("Ahoj ");
Console.Write("Petře");
```

Výstup:

```text
Ahoj Petře
```

Porovnání s Javou:

```text
Java                       C#

System.out.print()    ->    Console.Write()
```

---

## 3. Výpis proměnných

```csharp
int vek = 20;

Console.WriteLine(vek);
```

Můžeme také spojovat text a proměnné:

```csharp
int vek = 20;

Console.WriteLine("Věk: " + vek);
```

### Interpolace řetězců

V C# se velmi často používá interpolace řetězců.

```csharp
string jmeno = "Petr";
int vek = 20;

Console.WriteLine($"Jmenuji se {jmeno} a je mi {vek} let.");
```

Výstup:

```text
Jmenuji se Petr a je mi 20 let.
```

Do `{}` můžeme vložit i výraz:

```csharp
int a = 10;
int b = 20;

Console.WriteLine($"Součet je {a + b}");
```

---

## 4. Čtení z konzole

Pro čtení vstupu používáme:

```csharp
Console.ReadLine();
```

Metoda čeká, dokud uživatel něco nenapíše a nepotvrdí klávesou Enter.

```csharp
Console.Write("Zadej své jméno: ");

string jmeno = Console.ReadLine();

Console.WriteLine($"Ahoj {jmeno}");
```

### Důležité: `ReadLine()` vrací text

`Console.ReadLine()` vrací hodnotu typu `string`.

Pokud uživatel zadá:

```text
25
```

program dostane text:

```text
"25"
```

nikoliv přímo číslo.

Proto musíme při práci s čísly text převést.

---

## 5. Parsování celého čísla

Toto není správně:

```csharp
int vek = Console.ReadLine();
```

`Console.ReadLine()` vrací `string`, ale proměnná očekává `int`.

Použijeme:

```csharp
string vstup = Console.ReadLine();

int vek = int.Parse(vstup);
```

Nebo kratší zápis:

```csharp
int vek = int.Parse(Console.ReadLine());
```

### `int.Parse()`

Převede text na celé číslo.

```csharp
string text = "42";

int cislo = int.Parse(text);
```

Příklad:

```csharp
Console.Write("Zadej svůj věk: ");

int vek = int.Parse(Console.ReadLine());

Console.WriteLine($"Je ti {vek} let.");
```

---

## 6. Problém s `Parse()`

Pokud uživatel místo čísla zadá:

```text
ahoj
```

pak:

```csharp
int vek = int.Parse(Console.ReadLine());
```

vyvolá chybu.

Proto je při vstupu od uživatele často lepší používat `TryParse()`.

---

## 7. `int.TryParse()`

`TryParse()` se pokusí text převést na číslo.

Pokud se převod podaří, vrátí `true`.

Pokud se nepodaří, vrátí `false`.

```csharp
string vstup = Console.ReadLine();

bool povedloSe = int.TryParse(vstup, out int cislo);
```

Příklad:

```csharp
Console.Write("Zadej číslo: ");

string vstup = Console.ReadLine();

if (int.TryParse(vstup, out int cislo))
{
    Console.WriteLine($"Zadal jsi číslo {cislo}");
}
else
{
    Console.WriteLine("Nezadal jsi platné číslo.");
}
```

### Co znamená `out`

V tomto příkazu:

```csharp
int.TryParse(vstup, out int cislo);
```

říkáme metodě, aby se pokusila převést `vstup` a výsledek uložila do proměnné `cislo`.

---

## 8. `Parse()` vs. `TryParse()`

### `Parse()`

```csharp
int cislo = int.Parse(text);
```

Výhoda:

- jednoduchý zápis.

Nevýhoda:

- při neplatném vstupu dojde k chybě.

### `TryParse()`

```csharp
if (int.TryParse(text, out int cislo))
{
    Console.WriteLine(cislo);
}
```

Výhody:

- program nespadne při špatném vstupu,
- můžeme jednoduše zjistit, zda uživatel zadal správnou hodnotu.

Pro vstup od uživatele je `TryParse()` často lepší volba.

---

## 9. Čtení desetinného čísla

Pro `double` existují stejné metody.

```csharp
double cislo = double.Parse(Console.ReadLine());
```

Bezpečnější varianta:

```csharp
if (double.TryParse(Console.ReadLine(), out double cislo))
{
    Console.WriteLine($"Zadal jsi {cislo}");
}
else
{
    Console.WriteLine("Neplatné číslo.");
}
```

Stejný princip funguje i u dalších typů:

```csharp
int.Parse(text);
double.Parse(text);
float.Parse(text);
decimal.Parse(text);
bool.Parse(text);
```

---

## 10. Třída `Convert`

Pro převody lze použít také třídu `Convert`.

```csharp
string text = "25";

int cislo = Convert.ToInt32(text);
```

Další příklady:

```csharp
Convert.ToDouble(text);
Convert.ToBoolean(text);
Convert.ToString(cislo);
```

Pro vstup od uživatele je ale velmi běžné používat `Parse()` nebo `TryParse()`.

---

## 11. Součet dvou čísel

```csharp
Console.Write("Zadej první číslo: ");
int a = int.Parse(Console.ReadLine());

Console.Write("Zadej druhé číslo: ");
int b = int.Parse(Console.ReadLine());

int soucet = a + b;

Console.WriteLine($"Součet je {soucet}");
```

Bezpečnější varianta:

```csharp
Console.Write("Zadej první číslo: ");

if (!int.TryParse(Console.ReadLine(), out int a))
{
    Console.WriteLine("První hodnota není číslo.");
    return;
}

Console.Write("Zadej druhé číslo: ");

if (!int.TryParse(Console.ReadLine(), out int b))
{
    Console.WriteLine("Druhá hodnota není číslo.");
    return;
}

Console.WriteLine($"Součet: {a + b}");
```

---

## 12. Opakování vstupu, dokud není správný

```csharp
int vek;

while (true)
{
    Console.Write("Zadej svůj věk: ");

    if (int.TryParse(Console.ReadLine(), out vek))
    {
        break;
    }

    Console.WriteLine("Musíš zadat celé číslo.");
}

Console.WriteLine($"Tvůj věk je {vek}.");
```

Kratší varianta:

```csharp
int vek;

Console.Write("Zadej svůj věk: ");

while (!int.TryParse(Console.ReadLine(), out vek))
{
    Console.Write("Neplatný vstup. Zadej věk znovu: ");
}

Console.WriteLine($"Tvůj věk je {vek}.");
```

---

## 13. Kontrola rozsahu hodnoty

To, že uživatel zadal číslo, ještě neznamená, že číslo dává smysl.

Například věk `-500` je platný `int`, ale není platný věk.

```csharp
Console.Write("Zadej svůj věk: ");

if (int.TryParse(Console.ReadLine(), out int vek))
{
    if (vek >= 0 && vek <= 130)
    {
        Console.WriteLine($"Věk: {vek}");
    }
    else
    {
        Console.WriteLine("Věk musí být mezi 0 a 130.");
    }
}
else
{
    Console.WriteLine("Musíš zadat číslo.");
}
```

### Logické operátory

AND:

```csharp
&&
```

OR:

```csharp
||
```

NOT:

```csharp
!
```

Například:

```csharp
vek >= 0 && vek <= 130
```

---

## 14. Práce s načteným textem

### `Trim()`

Odstraní mezery na začátku a konci textu.

```csharp
string jmeno = Console.ReadLine().Trim();
```

### `ToLower()`

Převede text na malá písmena.

```csharp
string odpoved = Console.ReadLine().Trim().ToLower();

if (odpoved == "ano")
{
    Console.WriteLine("Pokračujeme.");
}
```

Uživatel tak může zadat:

```text
ANO
Ano
ano
```

### `ToUpper()`

Převede text na velká písmena.

```csharp
string text = Console.ReadLine();

Console.WriteLine(text.ToUpper());
```

---

## 15. Načtení více hodnot z jednoho řádku

Uživatel může zadat:

```text
10 20
```

Celý vstup načteme jako jeden `string`:

```csharp
string vstup = Console.ReadLine();
```

A rozdělíme:

```csharp
string[] casti = vstup.Split(' ');
```

Pole potom obsahuje:

```text
casti[0] = "10"
casti[1] = "20"
```

Příklad:

```csharp
Console.Write("Zadej dvě čísla oddělená mezerou: ");

string[] casti = Console.ReadLine().Split(' ');

int a = int.Parse(casti[0]);
int b = int.Parse(casti[1]);

Console.WriteLine($"Součet: {a + b}");
```

---

## 16. Formátování čísel

Při interpolaci můžeme nastavit počet desetinných míst.

```csharp
double cislo = 12.345678;

Console.WriteLine($"{cislo:F2}");
```

Výstup bude například:

```text
12,35
```

`F2` znamená dvě desetinná místa.

```csharp
Console.WriteLine($"{cislo:F1}");
```

vypíše jedno desetinné místo.

---

## 17. `Console.ReadKey()`

`Console.ReadKey()` načte jednu stisknutou klávesu.

```csharp
Console.WriteLine("Stiskni libovolnou klávesu.");

Console.ReadKey();
```

Můžeme také zjistit, která klávesa byla stisknuta:

```csharp
ConsoleKeyInfo klavesa = Console.ReadKey();

Console.WriteLine();
Console.WriteLine($"Stiskl jsi {klavesa.Key}");
```

### `ReadLine()` vs. `ReadKey()`

`Console.ReadLine()`:

- načte celý řádek,
- uživatel potvrzuje Enterem.

`Console.ReadKey()`:

- načte jednu klávesu,
- Enter není potřeba.

---

## 18. Java vs. C#

### Výpis

Java:

```java
System.out.println("Ahoj");
```

C#:

```csharp
Console.WriteLine("Ahoj");
```

Java:

```java
System.out.print("Ahoj");
```

C#:

```csharp
Console.Write("Ahoj");
```

### Vstup

Java:

```java
Scanner scanner = new Scanner(System.in);

String jmeno = scanner.nextLine();
```

C#:

```csharp
string jmeno = Console.ReadLine();
```

V Javě:

```java
int vek = scanner.nextInt();
```

V C#:

```csharp
int vek = int.Parse(Console.ReadLine());
```

nebo bezpečněji:

```csharp
int.TryParse(Console.ReadLine(), out int vek);
```

### Důležitý rozdíl

Java `Scanner` nabízí například:

```java
nextInt()
nextDouble()
nextLine()
```

`Console.ReadLine()` v C# vždy načítá řádek jako text.

Potom se rozhodneme, jak ho zpracujeme:

```csharp
string text = Console.ReadLine();

int cislo = int.Parse(Console.ReadLine());

double desetinneCislo = double.Parse(Console.ReadLine());
```

---

## 19. Nejčastější chyby

### Chyba 1 – pokus uložit `string` do `int`

Špatně:

```csharp
int cislo = Console.ReadLine();
```

Správně:

```csharp
int cislo = int.Parse(Console.ReadLine());
```

### Chyba 2 – `Parse()` nad neplatným vstupem

```csharp
int cislo = int.Parse(Console.ReadLine());
```

Pokud uživatel zadá `abc`, dojde k chybě.

Bezpečnější:

```csharp
if (int.TryParse(Console.ReadLine(), out int cislo))
{
    Console.WriteLine(cislo);
}
else
{
    Console.WriteLine("Neplatné číslo.");
}
```

### Chyba 3 – zapomenutý `$` u interpolace

Špatně:

```csharp
string jmeno = "Petr";

Console.WriteLine("Ahoj {jmeno}");
```

Výstup:

```text
Ahoj {jmeno}
```

Správně:

```csharp
Console.WriteLine($"Ahoj {jmeno}");
```

---

## 20. Kompletní příklad

```csharp
Console.WriteLine("=== Registrace uživatele ===");

Console.Write("Zadej jméno: ");
string jmeno = Console.ReadLine().Trim();

int vek;

Console.Write("Zadej věk: ");

while (!int.TryParse(Console.ReadLine(), out vek))
{
    Console.Write("Neplatný věk. Zadej celé číslo: ");
}

Console.Write("Zadej město: ");
string mesto = Console.ReadLine().Trim();

Console.WriteLine();
Console.WriteLine("=== Zadané údaje ===");
Console.WriteLine($"Jméno: {jmeno}");
Console.WriteLine($"Věk: {vek}");
Console.WriteLine($"Město: {mesto}");
```

---

## 21. Malý projekt – kalkulačka

```csharp
Console.WriteLine("=== Kalkulačka ===");

Console.Write("První číslo: ");

if (!double.TryParse(Console.ReadLine(), out double a))
{
    Console.WriteLine("Neplatné číslo.");
    return;
}

Console.Write("Druhé číslo: ");

if (!double.TryParse(Console.ReadLine(), out double b))
{
    Console.WriteLine("Neplatné číslo.");
    return;
}

Console.WriteLine($"Součet: {a + b}");
Console.WriteLine($"Rozdíl: {a - b}");
Console.WriteLine($"Součin: {a * b}");

if (b != 0)
{
    Console.WriteLine($"Podíl: {a / b}");
}
else
{
    Console.WriteLine("Nulou nelze dělit.");
}
```

---

# Úkoly k procvičení

## Úkol 1 – pozdrav

Načti jméno uživatele a vypiš například:

```text
Ahoj, Petře!
```

## Úkol 2 – věk

Načti věk uživatele a vypiš, kolik mu bude za rok.

```text
Zadej věk: 18
Za rok ti bude 19.
```

## Úkol 3 – obdélník

Načti šířku a výšku obdélníku a vypočítej:

- obsah,
- obvod.

## Úkol 4 – průměr

Načti tři čísla a vypočítej jejich průměr.

## Úkol 5 – bezpečný vstup

Načítej celé číslo tak dlouho, dokud uživatel nezadá skutečné celé číslo.

Použij:

```csharp
int.TryParse()
```

## Úkol 6 – kontrola věku

Načti věk a povol pouze hodnoty od `0` do `130`.

## Úkol 7 – ano/ne

Zeptej se:

```text
Chceš pokračovat?
```

Povol například odpovědi `ano` a `ne` bez ohledu na velikost písmen.

Nápověda:

```csharp
Trim()
ToLower()
```

## Úkol 8 – dvě čísla na jednom řádku

Uživatel zadá:

```text
12 30
```

Použij `Split()` a vypiš jejich součet.

---

# Tahák

Výpis s novým řádkem:

```csharp
Console.WriteLine("Text");
```

Výpis bez nového řádku:

```csharp
Console.Write("Text");
```

Načtení textu:

```csharp
string text = Console.ReadLine();
```

Načtení celého čísla:

```csharp
int cislo = int.Parse(Console.ReadLine());
```

Bezpečné načtení čísla:

```csharp
if (int.TryParse(Console.ReadLine(), out int cislo))
{
    Console.WriteLine(cislo);
}
```

Načtení desetinného čísla:

```csharp
double cislo = double.Parse(Console.ReadLine());
```

Interpolace:

```csharp
Console.WriteLine($"Hodnota je {cislo}");
```

Odstranění mezer:

```csharp
text = text.Trim();
```

Převod na malá písmena:

```csharp
text = text.ToLower();
```

Rozdělení textu:

```csharp
string[] casti = text.Split(' ');
```

Načtení jedné klávesy:

```csharp
Console.ReadKey();
```

---

# Co si zapamatovat

Nejdůležitější je tento postup:

```text
uživatel
   ↓
Console.ReadLine()
   ↓
string
   ↓
Parse / TryParse
   ↓
int, double, ...
```

`Console.ReadLine()` načítá text.

Pokud chceme s hodnotou počítat, musíme ji převést.

```csharp
string vstup = Console.ReadLine();

int cislo = int.Parse(vstup);
```

Pro vstup od uživatele je často bezpečnější:

```csharp
if (int.TryParse(Console.ReadLine(), out int cislo))
{
    // vstup je správně
}
else
{
    // vstup není číslo
}
```

## Nejkratší shrnutí

```csharp
// výpis
Console.WriteLine("Ahoj");

// výpis bez nového řádku
Console.Write("Zadej jméno: ");

// textový vstup
string jmeno = Console.ReadLine();

// číslo
int vek = int.Parse(Console.ReadLine());

// bezpečné číslo
int.TryParse(Console.ReadLine(), out int cislo);

// interpolace
Console.WriteLine($"Ahoj {jmeno}");
```
