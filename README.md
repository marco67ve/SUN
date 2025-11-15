# SUN

# I quattro Soli di Marco (anzi: sei)

## Dal QuickBASIC 4.5 al QB64: un viaggio tra pixel e plasma solare (1989--2025)

Questa raccolta di programmi BASIC racconta l'evoluzione di un'idea:
simulare il Sole, la sua fotosfera ribollente e la sua futura fase di
gigante rossa. Dal primo esperimento su un 8086 con grafica CGA, fino
alle versioni moderne in QB64, ogni codice è un frammento di storia
personale e tecnologica.

------------------------------------------------------------------------

## 1989 --- `SUN-G.BAS`

**Titolo:** Sole in fase gigante\
**Linguaggio:** QuickBASIC 4.5\
**Descrizione:** Primo esperimento di espansione solare per schede VGA.
I primi tre pianeti interni, rappresentati da un singolo pixel, vengono
inglobati e distrutti. Il Sole è reso con un cerchio giallo crescente,
disegnato per evitare i tipici pixel neri prodotti dalla geometria
regolare VGA con CIRCLE in ciclo. Un test sulla velocità della CPU rende
fluida l'animazione dall'8086 al 486.\
**Valore:** Documento pionieristico; un concetto scientifico espresso
con mezzi minimi.

------------------------------------------------------------------------

## 1990 --- `SUN-VGA.BAS`

**Titolo:** Aspetto del Sole in QB 4.5 per 486\
**Linguaggio:** QuickBASIC 4.5\
**Descrizione:** Rappresentazione della superficie solare ottimizzata
per VGA sui PC più potenti dell'epoca. La rapidità della CPU determina
la velocità con cui il plasma ribollente riempie la superficie tramite
un ciclo infinito.\
**Valore:** Testimonianza dell'adattamento ai nuovi hardware emergenti.

------------------------------------------------------------------------

## 1990--2025 --- `Sun-QB64-1.0.bas`

**Titolo:** Aspetto del Sole in QB64\
**Linguaggio:** QB64\
**Descrizione:** Prima versione moderna: la superficie solare appare
immediatamente grazie alla potenza dei PC contemporanei.

Rispetto alla versione originale a 16 bit, il Sole in QB64 è stato adattato ai
PC moderni con le seguenti modifiche principali:

- Ciclo rallentato per consentire una visualizzazione fluida della fotosfera
  ribollente anche su processori molto veloci.
- Tavolozza a 256 colori per una resa cromatica più realistica e dettagliata.
- Colore extra nell’array per distinguere i flare bianchi brillanti dai pixel
  generali.
- Pixel sostituiti da piccoli cerchi (Circle) per preservare l’effetto
  “granulosità” tipico delle versioni DOS.
- Dimensionamento dinamico della finestra in base alla risoluzione del desktop,
  con posizionamento nell’angolo in alto a sinistra.

Questi accorgimenti permettono di conservare l’aspetto “ribollente” del Sole
originale, ma con fluidità e dettagli migliorati adatti ai PC moderni,
mantenendo fede alla resa storica.\

**Valore:** Ponte tra passato e presente: il concetto del 1990 rinasce
con un realismo superiore.

------------------------------------------------------------------------

## 1990--2025 --- `Sun-QB64-1.1.bas`

**Descrizione:** Aggiunti flare bianchi (70%) e macchie solari scure
(30%) per un maggiore realismo.

------------------------------------------------------------------------

## 1989--2025 --- `Sun-QB64-2.1.bas`

**Descrizione:** Aggiunta una progressiva colorazione verso il rosso man
mano che il Sole si espande.\
**Valore:** Effetti visivi degni di una simulazione da supercomputer.

------------------------------------------------------------------------

# Sezione scientifica --- Il destino della Terra

-   Il Sole entrerà nella fase di gigante rossa tra circa **5 miliardi
    di anni**.\
-   Perderà circa **il 30% della sua massa**, allargando le orbite
    planetarie.\
-   Il raggio solare crescerà fino a circa **1,2 UA**, inglobando
    Mercurio, Venere e quasi certamente la Terra, che spiraleggerà nel
    Sole a causa dell'attrito con la fotosfera.\
-   I programmi simulano questo destino tramite cerchi e porzioni di
    cerchio.

------------------------------------------------------------------------

# Filosofia del pixel

-   **1989--1990:** i Soli QuickBASIC sono rudimentali ma
    pionieristici.\
-   **1990--2025:** i Soli QB64 serie 1.x introducono fluidità, flare e
    macchie.\
-   **1989--2025:** i Soli giganti serie 2.x chiudono il cerchio con
    un'esperienza visiva completa.

------------------------------------------------------------------------

# Autore

**Marco da Venezia (1989--1990)**\
**Revisione:** Bishop (2025)

------------------------------------------------------------------------

# Codici sorgente

```
' SUN-G.BAS - Sole in fase gigante
' PD Marco da Venezia, 1989
'
DECLARE SUB Sole (rSole)

SCREEN 12

' --- Sole
rSoleGigante = 240 ' 240 = 1 UA = raggio massimo
Sole 3             ' Disegno Sole giallo con raggio iniziale ~0.01 UA

' --- UA pianeti dal Sole
uaMercurio = .34
uaVenere = .67
uaTerra = .98

' Disegno pianeti (un pixel, in scala sarebbe 0.02ö0.03 pixel)
PSET (320 + uaMercurio * 240, 240), 9
PSET (320 + uaVenere * 240, 240), 15
PSET (320 + uaTerra * 240, 240), 11

' --- Test empirico CPU per incremento raggio in fase gigante
t = TIMER
WHILE TIMER - t < 1
    n = n + 1
WEND
stepSole = 480 / n ' n = 480 in .exe su 8088

RANDOMIZE TIMER

DO
    IF rSoleGigante > rSole THEN
        rSole = rSole + stepSole ' Espansione
        Sole rSole
    END IF

    FlarePianeta = 0

    '  Flare al contatto
    IF rSole > uaMercurio * 240 AND rSole < uaMercurio * 240 + 9 THEN
        xF = uaMercurio * 240
        FlarePianeta = 1
    END IF

    IF rSole > uaVenere * 240 AND rSole < uaVenere * 240 + 6 THEN
        xF = uaVenere * 240
        FlarePianeta = 1
    END IF

    IF rSole > uaTerra * 240 AND rSole < uaTerra * 240 + 3 THEN
        xF = uaTerra * 240
        FlarePianeta = 1
    END IF

    IF FlarePianeta THEN
        FOR cf = 0 TO 14 STEP 7 ' colori flare: 0, 7, 14
            FOR r = 0 TO 4
                CIRCLE (xF + 320, 480 / 2), r, cf
            NEXT
        NEXT
    END IF

    IF INKEY$ = CHR$(27) THEN EXIT DO
LOOP

END

SUB Sole (rSole)
    CIRCLE (320, 240), rSole, 6
    PAINT (320, 240), 14, 6
    CIRCLE (320, 240), rSole, 12
END SUB
```

```
' SUN-VGA.BAS - Aspetto del Sole in QB 4.5 per 486
' Marco da Venezia, 1990

DIM Colori(1 TO 5)
Colori(1) = 14 ' giallo
Colori(2) = 12 ' rosso intenso
Colori(3) = 4  ' rosso
Colori(4) = 6  ' ocra
Colori(5) = 7  ' bianco

SCREEN 12

rSun = 200 ' raggio del Sole
CIRCLE (320, 240), rSun, 14
PAINT (320, 240), 14

RANDOMIZE TIMER

DO
    FOR j = 1 TO 1000 ' min. 486 DX 33 MHz
        DO
            x = INT(RND * 480 + 1) - 240
            y = INT(RND * 480 + 1) - 240
        LOOP WHILE x ^ 2 + y ^ 2 > rSun ^ 2
        c = Colori(INT(RND * 5 + 1))
        PSET (320 + x, 240 + y), c
    NEXT
LOOP UNTIL INKEY$ = CHR$(27)
```

```
' Sun-QB64-1.0.bas - Aspetto del Sole in QB64
' Marco da Venezia, 1990 e 2025 rev. Bishop

Const cicliFotoni = 60000
Const coloriRGB = 6

Dim Shared colori(1 To coloriRGB)
colori(1) = _RGB(255, 255, 0) ' giallo
colori(2) = _RGB(255, 0, 0) ' rosso intenso
colori(3) = _RGB(255, 128, 0) ' arancio
colori(4) = _RGB(255, 192, 0) ' ocra
colori(5) = _RGB(200, 200, 200) ' bianco
colori(6) = _RGB(255, 255, 255) ' bianco intenso

yDisplay = _DesktopHeight - 32
xDisplay = yDisplay * 16 / 9
Screen _NewImage(xDisplay, yDisplay, 256)
_ScreenMove 0, 0

xCentro = xDisplay / 2
yCentro = yDisplay / 2
rSole = yDisplay / 2

Randomize Timer

Do
    For j = 1 To cicliFotoni
        Do
            x = Int(Rnd * yDisplay + 1) - yDisplay / 2
            y = Int(Rnd * yDisplay + 1) - yDisplay / 2
        Loop While x ^ 2 + y ^ 2 > rSole ^ 2
        c = colori(Int(Rnd * coloriRGB + 1))
        Circle (xCentro + x, yCentro + y), 1, c
    Next
    _Delay .1
    _Limit 30
Loop Until InKey$ = Chr$(27)
```

```
' Sun-QB64-1.1.bas - Aspetto del Sole con flare bianchi e macchie nere
' PD Marco da Venezia, 1990-2025

Const cicliFotoni = 60000
Const coloriRGB = 6

Dim Shared colori(1 To coloriRGB)
colori(1) = _RGB(255, 255, 0) ' giallo
colori(2) = _RGB(255, 0, 0) ' rosso intenso
colori(3) = _RGB(255, 128, 0) ' arancio
colori(4) = _RGB(255, 192, 0) ' ocra
colori(5) = _RGB(200, 200, 200) ' bianco
colori(6) = _RGB(255, 255, 255) ' bianco intenso

yDisplay = _DesktopHeight - 32
xDisplay = yDisplay * 16 / 9
Screen _NewImage(xDisplay, yDisplay, 256)
_ScreenMove 0, 0

xCentro = xDisplay / 2
yCentro = yDisplay / 2
rSole = yDisplay / 2

Randomize Timer

Do
    For j = 1 To cicliFotoni
        Do
            x = Int(Rnd * yDisplay + 1) - yDisplay / 2
            y = Int(Rnd * yDisplay + 1) - yDisplay / 2
        Loop While x ^ 2 + y ^ 2 > rSole ^ 2
        c = colori(Int(Rnd * 5 + 1))
        Circle (xCentro + x, yCentro + y), 1, c
    Next

    ' Flare e macchie solari casuali
    If Rnd < .33 Then ' 33% di probabilità per ciclo
        ' --- Posizione interna ---
        Do
            xOff = Int(Rnd * 2 * rSole + 1) - rSole
            yOff = Int(Rnd * 2 * rSole + 1) - rSole
            dist2 = xOff * xOff + yOff * yOff
        Loop While dist2 > (rSole * 0.85) ^ 2 ' 85% del raggio per evitare il bordo

        xF = xCentro + xOff
        yF = yCentro + yOff

        maxR = (rSole / 15) * Rnd

        ' --- Scelta casuale tra flare o macchia ---
        If Rnd < .7 Then
            c = colori(6) ' flare bianco brillante (70% probabilità)
        Else
            c = 0 ' macchia solare (30%)
        End If

        ' --- Disegno ---
        For r = 1 To maxR
            Circle (xF, yF), r, c, , Rnd
        Next
    End If

    _Delay 0.1
    _Limit 30

Loop Until InKey$ = Chr$(27)
```

```
' Sun-QB64-2.0.bas - Sole gigante rossa
' PD Marco da Venezia (1989) & Bishop (2025)

Dim colori(1 To 5)

colori(1) = 14 ' giallo
colori(2) = 12 ' rosso intenso
colori(3) = 4 ' rosso
colori(4) = 6 ' arancio ocra
colori(5) = 15 ' bianco

yDisplay = _DesktopHeight - 32
xDisplay = yDisplay * 16 / 9
Screen _NewImage(xDisplay, yDisplay, 256)
_ScreenMove 0, 0

xCentro = xDisplay / 2
yCentro = yDisplay / 2

' --- Parametri Sole
Dim rSole As Single, rMax As Single, passo As Single
rSole = yCentro / 100 ' diametro iniziale ~0.01 UA
rMax = yCentro ' 1 UA = raggio massimo
passo = 1 ' 0.5 ' incremento per ciclo

' --- Colori pianeti
coloreMercurio = 6
coloreVenere = 12
coloreTerra = 11

' --- Pianeti (a 0.9 UA per rSole a 1 UA)
uaMercurio = 0.3
uaVenere = 0.6
uaTerra = 0.9
xMercurio = xCentro + uaMercurio * yCentro
xVenere = xCentro + uaVenere * yCentro
xTerra = xCentro + uaTerra * yCentro
yPianeti = yCentro

' Disegno pianeti
PSet (xMercurio, yPianeti), coloreMercurio
PSet (xVenere, yPianeti), coloreVenere
PSet (xTerra, yPianeti), coloreTerra

Randomize Timer

Do
    ' Disegna Sole crescente
    Circle (xCentro, yCentro), rSole, colori(1)
    Paint (xCentro, yCentro), colori(1)

    For j = 1 To 60000
        Do
            x = (2 * Rnd * rSole + 1) - rSole
            y = (2 * Rnd * rSole + 1) - rSole
        Loop While x ^ 2 + y ^ 2 > rSole ^ 2
        c = colori(Int(Rnd * 5 + 1))
        PSet (xCentro + x, yCentro + y), c
    Next

    FlarePianeta = 0

    ' --- Flare al contatto
    If rSole >= uaMercurio * yCentro And rSole < uaMercurio * yCentro + rSole / 20 Then
        xF = uaMercurio * yCentro
        FlarePianeta = 1
    End If
    If rSole >= uaVenere * yCentro And rSole < uaVenere * yCentro + rSole / 20 Then
        xF = uaVenere * yCentro
        FlarePianeta = 1
    End If
    If rSole >= uaTerra * yCentro And rSole < uaTerra * yCentro + rSole / 20 Then
        xF = uaTerra * yCentro
        FlarePianeta = 1
    End If

    If FlarePianeta Then
        rFlare = (rSole / 10) * Rnd
        For r = 1 To rFlare
            Circle (xF + xCentro, yDisplay / 2), r, 0, , Rnd
        Next
    End If

    ' Espansione continua
    rSole = rSole + passo
    If rSole > rMax Then rSole = rMax

    _Delay .1
    _Limit 30
    If InKey$ = Chr$(27) Then Exit Do
Loop

End
```

```
' Sun-QB64-2.1.bas - Sole gigante rossa con arrossamento dinamico
' PD Marco da Venezia (1989) & Bishop (2025)

Dim colori(1 To 5)

colori(1) = 14 ' giallo
colori(2) = 12 ' rosso intenso
colori(3) = 4 ' rosso
colori(4) = 6 ' arancio ocra
colori(5) = 15 ' bianco

yDisplay = _DesktopHeight - 32
xDisplay = yDisplay * 16 / 9
Screen _NewImage(xDisplay, yDisplay, 256)
_ScreenMove 0, 0

xCentro = xDisplay / 2
yCentro = yDisplay / 2

' --- Parametri Sole
Dim rSole As Single, rMax As Single, passo As Single
rSole = yCentro / 100 ' raggio iniziale ~0.01 UA
rMax = yCentro ' raggio massimo ~1 UA
passo = 1 ' incremento per ciclo

' --- Colori e raggio pianeti
coloreMercurio = 6: rMercurio = 3 ' non in scala
coloreVenere = 15: rVenere = 3 ' non in scala
coloreTerra = 11: rTerra = 3 ' non in scala

' --- Pianeti (a 0.9 UA per rSole a 1 UA)
uaMercurio = 0.3
uaVenere = 0.6
uaTerra = 0.9
xMercurio = xCentro + uaMercurio * yCentro
xVenere = xCentro + uaVenere * yCentro
xTerra = xCentro + uaTerra * yCentro
yPianeti = yCentro

' Disegno pianeti
Circle (xMercurio, yPianeti), rMercurio, coloreMercurio: Paint (xMercurio, yPianeti), coloreMercurio
Circle (xVenere, yPianeti), rVenere, coloreVenere: Paint (xVenere, yPianeti), coloreVenere
Circle (xTerra, yPianeti), rTerra, coloreTerra: Paint (xTerra, yPianeti), coloreTerra

Randomize Timer

Do
    ' Disegna Sole crescente
    Circle (xCentro, yCentro), rSole, colori(1)
    f = .2 * rSole / rMax ' fattore dinamico per flare diffuso e cicli fotonici
    If Rnd < f Then Paint (xCentro, yCentro), colori(1) ' provoca flare solare diffuso
    cicliFotoni = f * 60000

    ' Pixel caldi
    For j = 1 To cicliFotoni
        Do
            x = (2 * Rnd * rSole + 1) - rSole
            y = (2 * Rnd * rSole + 1) - rSole
        Loop While x ^ 2 + y ^ 2 > rSole ^ 2

        ' Arrossamento progressivo
        rossoProb = rSole / rMax
        c = colori(Int(Rnd * 5 + 1))
        If Rnd < rossoProb Then c = colori(2) ' rosso
        If Rnd < rossoProb / 2 Then c = colori(4) ' ocra
        PSet (xCentro + x, yCentro + y), c
    Next


    FlarePianeta = 0

    ' --- Flare al contatto
    If rSole >= uaMercurio * yCentro And rSole < uaMercurio * yCentro + rSole / 20 Then
        xF = uaMercurio * yCentro
        rMercurio = 0
        FlarePianeta = 1
    End If
    If rSole >= uaVenere * yCentro And rSole < uaVenere * yCentro + rSole / 20 Then
        xF = uaVenere * yCentro
        rVenere = 0
        FlarePianeta = 1
    End If
    If rSole >= uaTerra * yCentro And rSole < uaTerra * yCentro + rSole / 20 Then
        xF = uaTerra * yCentro
        rTerra = 0
        FlarePianeta = 1
    End If

    If FlarePianeta Then
        rFlare = (rSole / 10) * Rnd
        For r = 1 To rFlare
            Circle (xF + xCentro, yDisplay / 2), r, 0, , Rnd
        Next
    End If

    ' Espansione continua
    rSole = rSole + passo
    If rSole > rMax Then rSole = rMax

    _Delay .1
    _Limit 30
    If InKey$ = Chr$(27) Then Exit Do
Loop

End
```
