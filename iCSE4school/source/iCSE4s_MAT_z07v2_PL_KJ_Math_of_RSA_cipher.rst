RSA szyfrowanie asymetryczne. 
=============================

1. Wprowadzenie metodyczne.
^^^^^^^^^^^^^^^^^^^^^^^^^^^

    Zajęcia odbywały się na dodatkowych godzinach (ICSE4school) w grupie uczniów, które miały rozszerzony poziom nauczania z matematyki i informatyki w drugiej klasie liceum. Powyższy temat nadaje się również jako praca projektowa, która łączy wiedzę matematyczno-informatyczną. Język SageMath umożliwia pracę na dużych liczbach przekraczających zakres zmiennych typu float, double, a jednocześnie szybkość obliczeniowa jest naprawdę imponująca. Jak wiadomo powyższe elementy są istotne w dziedzinie kryptografii, która łączy teorię liczb z praktyką programistyczną. Nie przekracza to zakresu materiału przewidzianego na rozszerzeniu z informatyki liceum lub technikum. Dlatego też postanowiłem przeprowadzić lekcje dotyczące asymetrycznego szyfrowania wiadomości **RSA**.

    Materiał dla uczniów jest podzielony na trzy rozdziały. Pierwszy z nich wprowadza pojęcia kongruencji oraz istotne matematyczne twierdzenia, które są wykorzystywane w kryptografii. Co prawda dowody i szczegóły zagadnień są pominięte, ale zainteresowani uczniowie bez problemu znajdą te informacje w internecie. Drugi rozdział to szczegółowe wprowadzenie szyfrowania asymetrycznego stosowanego na początku lat 70 poprzedniego stulecia (obecnie stosowanego już tylko w celach dydaktycznych). Trzeci rozdział to już pełne szyfrowanie RSA. W każdej części są wyszczególnione ćwiczenia i zadania dla uczniów.  
    
    
    
**Uczniowie powinni znać i  rozumieć:**

- działania na potęgach, NWD, dzielenie z resztą. *(1.1, 1.4, 1.5 mat_p)*,

- podstawowy algorytm Euklidesa a najlepiej rozszerzoną jego wersję, *(1.0-II-5.11.a inf_r)*,

- podstawowe komendy programistyczne w SageMath: działania, funkcję warunkową, pętle *(1.0-II-5.22-23 inf_r)*,

- potrzebę szyfrowania wiadomości *(1.0-II-2.5, 1.0-II-5.11.e inf_r)*,

- kodowanie znaków ASCI *(1.0-II-5.11.d inf_r)*.



**Uczniowie na poniższych zajęciach poznają:**
    
- własności działania modulo (kongruencję), chińskie twierdzenie o resztach,

- małe twierdzenie Fermata, proste szyfrowanie asymetryczne,

- szyfrowanie asymetryczne RSA – oparte na liczbach pierwszych *(1.0-II-5.11.e inf_r)*.



**Powyżej w nawiasach jest wpisany szczegółowy zakres nauczanych treści.**

mat_p – matematyka poziom podstawowy, inf_r – informatyka poziom rozszerzony.   


Ilość godzin prowadzenia zajęć 3 + zadania dodatkowe. 



    **Uwaga!**

W każdym z okien programu można zmieniać liczby, tekst, zmienne lub cały kod.

Nie musisz się martwić, jeśli program przestanie działać, bo po odświeżeniu powróci do ustawień początkowych.

Często następny kod wynika z poprzedniego, więc należy ćwiczenia (algorytmy) wykonywać według kolejności.

1a. Definicja kongruencji.
^^^^^^^^^^^^^^^^^^^^^^^^^^

Dwie liczby całkowite : :math:`a, b` przystają do siebie **modulo** *n*, jeśli: :math:`(a-b) \cdot k=n,\hspace{2mm} k \in Z.`

Kongruencję zapisujemy symbolicznie: :math:`a = b (mod \hspace{2mm} n)`.

Przykłady:
""""""""""

2 = 2 (mod 8), ponieważ 2 - 2 = 0,  0 jest podzielne przez 8,

3 = 18 (mod 5), ponieważ 3 - 18 = -15, -15 jest podzielne przez 5,

100 = 1 (mod 9), ponieważ 100 - 1 = 99, 99 jest podzielne przez 9,

250 = 206 (mod 22), ponieważ 250 - 206 = 44, 44 jest podzielne przez 22.

Ćwiczenie 1.
""""""""""""

Znajdź x takie, że: 3x = 1 (mod 4), 5<x<10

.. sagecellserver::

    for x in range(5,11):        #You can change this range
        if (3*x - 1) % 4 == 0:   #You can change this equation
            print "x=",x

Ćwiczenie 2.
""""""""""""

Znajdź x takie, że: 3x+2 = 1 (mod 5)

.. sagecellserver::

    for x in range(40):
        if (3*x+2 - 1) % 5 == 0:
            print x

Oczywiście istnieje nieskończenie takich rozwiązań. Dodatkowo te rozwiązania wyznaczają ciąg arytmetyczny.

Ćwiczenie 3.
""""""""""""

Znajdź x takie, że: 3x = 1 (mod 6)

.. sagecellserver::

    for x in range(100):
        if (3*x-1) % 6 == 0:
            print x
    print "?"


W powyższym ćwiczeniu nie istnieje żadna liczba, która spełnia powyższą kongruencję.


1b. Chińskie twierdzenie o resztach.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Poniższe ćwiczenie można rozwiązać przy użyciu chińskiego twierdzenia o resztach. Jedno z najważniejszych twierdzeń z teorii liczb i kryptografii. Twierdzenie to pozwala dzielić sekret wśród kilku osób (ważne hasło liczbowe).

Ćwiczenie 4.
""""""""""""

Tabliczka czekolady składa się z mniej niż 100 kawałków. Przy podziale na trzy równe części, pozostaje 1 kawałek czekolady. Dzieląc na 5 równych części, zostają 3 kawałki czekolady, a przy podziale na 7 równych części, pozostają 2 kawałki.

Wiemy, że liczba kawałków czekolady musi spełniać poniższe kongruencje:

x = 1 mod 3,

x = 3 mod 5,

x = 2 mod 7.

.. sagecellserver::

    for x in range(100):
        if (x-1) % 3 == 0 and (x-3) % 5 == 0 and (x-2) % 7 == 0:
            print x
    

1c. Małe twierdzenie Fermata.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Jeśli** *p* jest liczbą pierwszą oraz *a*, *p* są względnie pierwsze, **wtedy** :math:`a^{p-1} - 1` jest wielokrotnością liczby *p*. Zapisujemy to symbolicznie: :math:`a^{p-1}=1 (mod \hspace{2mm} p)`.

Sprawdźmy poprawność powyższego twierdzenia, dla kolejnych liczb pierwszych, numerycznie z wykorzystaniem języka Python.

Dla a = 35 i p = 3 lub p = 5 liczby nie spełniają założeń twierdzenia. Możemy dodatkowo stwierdzić, że liczba :math:`a^{p-1} - 1` jest podzielna przez p.

.. sagecellserver::

    for x in range (1, 30):
        p = nth_prime(x)
        print(p, 35^(p-1) % p)


2. Szyfrowanie wiadomości.
^^^^^^^^^^^^^^^^^^^^^^^^^^

Pierwsze wzmianki o kryptografii pochodzą już ze starożytności. Można stwierdzić, że szyfrowanie powstało równocześnie z wynalezieniem pisma. Szyfrowanie było stosowne przy przekazywaniu wiadomości wojskowych lub politycznych. Na lekcjach informatyki poznaliśmy (lub poznamy) szyfr Cezara. Jest to prosty szyfr, w którym zamieniamy litery. Co prawda zaszyfrowana wiadomość jest niezrozumiała, ale także prosta do odszyfrowania. Inne metody starożytnych były bardziej wyrafinowane i trudniejsze do odszyfrowania. Do lat sześćdziesiatych dwudziestego wieku znane były tylko szyfry symetryczne, to znaczy takie, które mają jeden klucz (jedną metodę) dzięki, któremu szyfrujemy i deszyfrujemy wiadomości.

W latach siedemdziesiątych dwudziestego wieku kryptografowie dzięki informatyzacji, zwiększeniu mocy obliczeniowej komputerów oraz potrzebie zabezpieczenia danych wymyślili szyfr asymetryczny, czyli taki, w którym używamy dwóch różnych kluczy – jeden do zaszyfrowania, a drugi do odszyfrowania (kolejność kluczy jest nieważna). Jeden z kluczy udostępniamy osobie, która ma przesłać nam tajną wiadomość. Możemy nawet udostępnić klucz na naszej stronie internetowej (dostępny dla wszystkich - klucz publiczny). Drugi klucz jest tajny (klucz prywatny) znamy go tylko my i nie możemy go nikomu udostępnić. Tylko i wyłącznie dzięki kluczowi prywatnemu możemy odszyfrować wiadomość.

Poniżej opiszemy prosty szyfr asymetryczny, który można złamać (czyli znając liczby d, n można szybko znaleść liczbę e). Będzie to Wasze zadanie dodatkowe.

**Jak matematycznie stworzyć szyfr asymetryczny?**

Do stworzenia prostego szyfru asymetrycznego będą nam potrzebne różne liczby naturalne: :math:`a, b, a1, b1`.

Czym większe liczby tym szyfr jest bezpieczniejszy - trudniejszy do odszyfrowania bez znajomości odpowiedniego klucza.

Dla naszego przykładu wystarczą liczby dwu, trzy cyfrowe.

Obliczamy: :math:`M=a \cdot b-1`, wtedy: :math:`e=a1 \cdot M+a, \hspace{3mm} d=b1\cdot M+b, \hspace{3mm} n=(e \cdot d-1)/M`

Otrzymujemy parę kluczy, klucz publiczny: :math:`(d, n)` i klucz prywatny: :math:`(e, n)`.

**Poniżej przykład generowania kluczy oraz zaszyfrowania liczby.**

.. sagecellserver::

    sage: number=1234567   #You can change this number (message). What will be if number larger then n?
    sage: a=89             #you can change the numbers: a, b, a1, b1
    sage: b=45
    sage: a1=98
    sage: b1=55
    sage: M=a*b-1
    sage: e=a1*M+a
    sage: d=b1*M+b
    sage: n=(e*d-1)/M
    sage: print " public key:", (d, n)
    sage: print "private key:",(e, n)
    sage: # encryption
    sage: szyfr = (number*d) % n
    sage: print "encryption:", szyfr
    sage: # decryption
    sage: deszyfr = (szyfr*e) % n
    sage: print "decryption:", deszyfr
 


**Co zrobić gdy liczba jest więsza od n?**

1. Obliczamy resztę z dzielenia przez n (otrzymujemy "porcję" do zaszyfrowania).

2. Szyfrujemy otrzymaną "porcję".

3. Do szyfru dodajemy zaszyfrowaną "porcję" w kolejnej potędze liczby n.

4. Dzielimy liczbę przez n.

5. Jeśli otrzymana liczba jest większa od 0, to powtarzamy kroki 1-4


.. sagecellserver::

    number=123456567675635352364213879879797996743546789435345241234324234235 #Big number(message)
    szyfr = 0
    i=0
    while number>0:                           # 5
        pomoc=number%n                        # 1 
        szyfr = szyfr + ((pomoc*d) % n)*n^i   # 2, 3
        i=i+1
        number=int(number/n)                  # 4
    print szyfr


W podobny sposób deszyfrujemy wiadomość:

Pomoc:

============== =============== ======
number → szyfr szyfr → deszyfr d→e
============== =============== ======

Spróbuj poniżej odszyfrować liczbę:

.. sagecellserver::

    i=0
    while number>0:                           # 5
        pomoc=number%n                        # 1 
        szyfr = szyfr + ((pomoc*d) % n)*n^i   # 2, 3
        i=i+1
        number=int(number/n)                  # 4
    print szyfr


Zazwyczaj chcemy zaszyfrować tekst, a nie liczbę, czyli musimy zamienić litery (znaki) na liczbę. Do tego posłużymy się kodem ASCII.

Każdej literze, znakowi przyporządkowana jest liczba z przedziału od 1 do 128.

Poniżej algorytm szyfrowania wiadomości tekstowej (ten kod został napisany i wprowadzony przez uczniów na zajęciach).

.. sagecellserver::

    number=0
    i=0
    tekst="This is the secret message or anything."
    for x in tekst:
        i=i+1
        print x,"->", ord(x)," ",
        if (i%10==0):
            print 
        number=number + ord(x)*128^i
    print
    print "number =", number
  

Pełny algorytm szyfrujący.
""""""""""""""""""""""""""

Po złożeniu powyższych programów otrzymujemy pełny algorytm szyfrowania i deszyfrowania wiadomości tekstowych.

.. sagecellserver::

    sage: number=0
    sage: i=0
    sage: tekst="This is the secret message or anything." #message
    sage: tekst2=""
    sage: print "message:", tekst
    sage: # change text to number
    sage: for x in tekst:
    sage:     i=i+1
    sage:     number=number + ord(x)*128^i
    sage: print "number:", number
    sage: print ""
    sage: # encription
    sage: szyfr = 0
    sage: i=0
    sage: while number>0:
    sage:     pomoc=number%n
    sage:     szyfr = szyfr + ((pomoc*d) % n)*n^i
    sage:     i=i+1
    sage:     number=int(number/n)
    sage: print "encription:", szyfr


Pełny algorytm deszyfrujący.
""""""""""""""""""""""""""""

.. sagecellserver::

    sage: tekst2=""
    sage: deszyfr = 0
    sage: i=0
    sage: print "encription:", szyfr
    sage: # decription
    sage: while szyfr>0:
    sage:     pomoc=szyfr%n
    sage:     deszyfr = deszyfr + ((pomoc*e) % n)*n^i
    sage:     i=i+1
    sage:     szyfr=int(szyfr/n)
    sage: print "decription: ", deszyfr
    sage: ## change number to text
    sage: i=0
    sage: while deszyfr>0:
    sage:     i=i+1
    sage:     deszyfr=int(deszyfr/128)
    sage:     tekst2 = tekst2 + chr(deszyfr%128)
    sage: print "message: ", tekst2
 

3. Szyfrowanie asymetryczne RSA.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**RSA** jeden z pierwszych i najpopularniejszy asymetryczny algorytm kryptograficzny z kluczem publicznym, zaprojektowany w 1977 przez Rona Rivesta, Adi Szamira oraz Leonarda Adlemana (jego nazwa pochodzi od pierwszych liter nazwisk jego twórców).

Bezpieczeństwo szyfru RSA opiera się na rozkładzie dużych (ponad dwustucyfrowych) liczb złożonych na liczby pierwsze (trudność faktoryzacji).

Poniżej przykład
""""""""""""""""

1. Wybieramy liczby pierwsze 20-34 cyfrowe.

2. Mnożymy je i wyznaczamy podział otrzymanej liczby złożonej na czynniki pierwsze (to trwa bardzo długo).


.. sagecellserver::

    sage: %time
    sage: @interact 
    sage: def _(n=slider( srange(20,32,2))):
    sage:     a=int(random()*10^n)
    sage:     a=next_prime(a)
    sage:     print a
    sage:     b=int(random()*10^n)
    sage:     b=next_prime(b)
    sage:     print b
    sage:     n=a*b
    sage:     print(factor(n))

**Zobacz jeszcze przewidywania dla dłuższych liczb.**

.. sagecellserver::

    sage: @interact 
    sage: def _(n=slider( range(34,101,2))):
    sage:     t=2^((n-34)/2)
    sage:     print n,"-digits prime numbers, factoring time:", t, "minutes"
    sage:     if t>100 and t<60*24:
    sage:         print n,"-digits prime numbers, factoring time:", int(t/60), "hours"
    sage:     elif t>60*24 and t<60*24*365:
    sage:         print n,"-digits prime numbers, factoring time:", int(t/60/24), "days"
    sage:     elif t>60*24*365:
    sage:         print n,"-digits prime numbers, factoring time:", int(t/60/24/365), "year"


Generowanie szyfru RSA.
"""""""""""""""""""""""

1. Wybieramy dwie duże liczby pierwsze: :math:`p, q` (w praktyce wykorzystuje się liczby ponad stocyfrowe, ale dla naszych porzeb wystarczą liczby trzycyfrowe).

2. Obliczamy:  :math:`n=p \cdot q, \hspace{2mm} f=(p-1)(q-1)`.

3. Wybieramy dowolną nieparzystą liczbę *e*, taką że::math:`1  < e < f` and :math:`gcd(d,\hspace{2mm} f) = 1`.

4. Wyznaczamy liczbę :math:`d` as: :math:`de=1 \hspace{1mm} (mod \hspace{1mm} f)`.

Klucz publiczny to para liczb: :math:`(d, n)`.

Klucz prywatny to para liczb:  :math:`(e, n)`.


.. sagecellserver::

    sage: los=int(100*random())
    sage: p=nth_prime(30+los)
    sage: los=int(100*random())
    sage: q=nth_prime(30+los)
    sage: n=p*q
    sage: f=(p-1)*(q-1)
    sage: los=int(f*random())
    sage: e=next_prime(los)
    sage: print "p =",p, ", q =",q, ", e =",e, ", n =", n, ", f =", f

Ostatecznie należy wyznaczyć liczbę :math:`e` taką, że: :math:`(d \cdot e) \hspace{1mm} mod f=1`.

Możemy użyć rozszerzonego algorytmu Euklidesa do wyznaczenia liczby e.
Moi uczniowie zmieniając istniejący program w Internecie napisali poniższy program, ale nie zawsze generuje on prawidłową liczbę.
Spróbuj poprawić ten kod!

.. sagecellserver::

    sage: a = e
    sage: p0 = 0
    sage: p1 = 1
    sage: a0 = a
    sage: n0 = f
    sage: q  = int(n0/a0) 
    sage: r  = n0 % a0
    sage: while (r > 0):
    sage:     t = p0 - q * p1
    sage:     if (t >= 0):
    sage:         t = t % n
    sage:     else:
    sage:         t = n - ((-t) % n)
    sage:     p0 = p1
    sage:     p1 = t
    sage:     n0 = a0
    sage:     a0 = r
    sage:     q  = int(n0/a0)
    sage:     r  = n0 % a0
    sage: d = p1
    sage: print "verification : (d*e)%f =", (d*e)%f
    sage: print " public key:", d, n
    sage: print "private key:", e, n

 
Pełny algorytm szyfrowania RSA.
"""""""""""""""""""""""""""""""

Wystarczy skopiować algorytm szyfrowania z punktu 2 i zamienić: pomoc*d na pomoc^d.

.. sagecellserver::

    sage: number=0
    sage: i=0
    sage: tekst="This is secret message or anything." #message
    sage: tekst2=""
    sage: print "message:", tekst
    sage: # change text to number
    sage: for x in tekst:
    sage:     i=i+1
    sage:     number=number + ord(x)*128^i
    sage: print "number:", number
    sage: print ""
    sage: # encription
    sage: szyfr = 0
    sage: i=0
    sage: while number>0:
    sage:     pomoc=number%n
    sage:     szyfr = szyfr + ((pomoc^d) % n)*n^i
    sage:     i=i+1
    sage:     number=int(number/n)
    sage: print "encription:", szyfr


Pełen algorym deszyfrujący RSA.
"""""""""""""""""""""""""""""""

Wystarczy skopiować algorytm deszyfrowania z punktu 2 i zamienić: pomoc*d na pomoc^d.

.. sagecellserver::

    sage: tekst2=""
    sage: deszyfr = 0
    sage: i=0
    sage: print "encription:", szyfr
    sage: # decription
    sage: while szyfr>0:
    sage:     pomoc=szyfr%n
    sage:     deszyfr = deszyfr + ((pomoc^e) % n)*n^i
    sage:     i=i+1
    sage:     szyfr=int(szyfr/n)
    sage: print "decription: ", deszyfr
    sage: ## change number to text
    sage: i=0
    sage: while deszyfr>0:
    sage:     i=i+1
    sage:     deszyfr=int(deszyfr/128)
    sage:     tekst2 = tekst2 + chr(deszyfr%128)
    sage: print "message: ", tekst2
 



