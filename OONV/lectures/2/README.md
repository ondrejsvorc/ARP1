# Polymorfismus
- statick� (v dob� p�ekladu)
- dynamick�

# Statick� polymorfismus

## P�etypov�n�
- implicitn� (kl��ov� slovo implicit)
- explicitn� (kl��ov� slovo explicit)
- https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/user-defined-conversion-operators
- pokud se p�i p�etypov�n� ztr�c� informace (tak d�lat explicitn�)
- z int na double je implicitn�, ale z double na int u� explicitn� (proto�e se ztr�c� informace, a to desetinn� ��st)

### Probl�m �. 1
M�me t��du Digit a chceme za��dit konverzi na typ int. Jsou 3 zp�soby, jak to �e�it:
1. int(Digit) - p�etypovac� konstruktor (nelze u int, proto�e je to vestav�n� typ)
2. instan�n� metoda na t��d� Digit (.ToInt)
3. p�etypovac� oper�tor
 
```C#
public readonly struct Digit
{
    private readonly byte digit;

    public Digit(byte digit)
    {
        if (digit > 9)
        {
            throw new ArgumentOutOfRangeException(nameof(digit), "Digit cannot be greater than nine.");
        }
        this.digit = digit;
    }

    public static implicit operator byte(Digit d) => d.digit;
    public static explicit operator Digit(byte b) => new Digit(b);

    public override string ToString() => $"{digit}";
}
```

# P�et�ov�n�
- metod/oper�tor�
- schopnost m�t metodu se stejn�m jm�nem, ale s jin�m po�tem parametr� anebo se stejn�m, ale s parametry s jin�mi datov�mi typy
- t�ko p�epsateln� do jazyk�, kter� p�et�ov�n� nepodporuj� (nap�. Python)

```C#
double Sin(double x);
double Sin(int x);
```

```C#
public class Zlomek
{
    // Zastupuje konstruktor bez parametr�, s jedn�m parametrem a se dv�ma parametry
    public Zlomek(int citatel = 0, int jmenovatel = 1) { }
}
```

# Generick� typy (Generics)
- zp�sob jak t��dy a metody parametrizovat pomoc� typ�

```C#
List<T> = generick� typ, kter� m� jeden typov� parametr a jmenuje se List

Tuple<T1>
Tuple<T1, T2>
Tuple<T1, T2, T3>

// Specializace = dosazen� za typov� parametry, tak dostaneme tzv. konkr�tn� typ, co� je typicky t��da (nap�. )
List<int>
Tuple<int, int>
```

```
C# generika = specializace za p�ekladu

List<T>, Add(T item) - kdy� ud�l�m List<int>, tak p�eklada� vygeneruje metody s T parametrem se zvolen�m typov�m parametrem (tedy int)
```

### F�ze p�ekladu
1. Vlastn� p�eklad - kontrola generick�ch typ�
1. P�eklad p�ed b�hem - bytecode
1. P�eklad bytecode do strojov�ho k�du (JIT, AOT)
   - JIT (Just in Time) = ka�dou funkci p�i prvn�m vol�n� p�elo��, a pak u� ji nep�ekl�d�
   - AOT (Ahead of Time) = p�elo�� v�e dop�edu
1. Specializace generick�ch typ� (generika zmiz�, jsou nahrazeny a p�elo�eny pro konkr�tn� k�d, proto generika nejsou pomalej�� ne� negenerick� metody)
1. B�h (runtime)

# Dynamick� polymorfismus
- d�di�nost

## D�di�nost
- mechanismus pozdn� vazby
- p�edefinov�n� (override = chci, aby metoda byla jinou verz� metody, kterou jsem zd�dil)
- shadowing (zast�n�n�, p�ekryt� metody rodi�e)
- abstract metoda (o�ek�v�me, �e bude implementov�na potomkem, nem� t�lo)
- virtual metoda (m� defaultn� t�lo)
- kdy� u n�jak� metody/property v b�zov� t��d� nevyspecifikuju abstract/virtual, tak je to implicitn� virtual
- zast�n�n� neexistuje v Pythonu
- jednoduch� d�di�nost - grafem/relace d�di�nosti je strom - lze d�dit z jedn� t��dy (hiearchick� pohled na sv�t)

```C#
public override string Hlas() { } // p�edefinov�n�
public new string Hlas() { } // zast�n�n� (m�li bychom se mu vyh�bat, jen v p��pad� kolize)
```

```C#
t��da A: metody x, y
t��da B d�d� z A: d�d� metody x, y a p�id�v� z
t��da A: p�id� metodu z
```

```
public abstract class Zvire
{
    public abstract string Hlas();

    public virtual string Jmeno() // dovolujeme p�edefinov�n�, kdybychom vynechali, tak to p�edefinovat potomkem nelze
    {
        return "";
    }
}

public class Pes : Zvire
{
    public override string Hlas()
    {
        return "Haf";
    }
}

Zvire z = new Pes();
z.Hlas();  // zavol� se metoda Hlas ze t��dy Pes
z.Jmeno(); // zavol� se metoda Jmeno ze t��dy Zvire
```

## Interface
- v z�kladu m��e m�t jen hlavi�ky metod, dnes i dal�� mo�nosti
- schopnost n�co d�lat, schopnost m�t n�jak� metody
- nepou��v�me v n�m identifik�tory p��stupu (public, )

```C#
public interface IFlyable
{
    void TakeOff();
    void Land();
    int Altitude { get; }
}
```

# Duck polymorfismus
- dynamic i;
- b�n� se nepou��v�
- kontrola existence metod a� p�i vykon�n� (za b�hu)