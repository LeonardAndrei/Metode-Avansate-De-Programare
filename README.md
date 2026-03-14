# Laborator 2: Modelare UML - Sistem Gestiune Comenzi

Acest repository contine diagramele UML pentru faza de analiza si design a sistemului. Scopul a fost identificarea problemelor de arhitectura dintr-un sistem mostenit si propunerea unei solutii decuplate.

## Artefacte (in folderul `docs/`)
* `use-case.png` - Actorii si functionalitatile sistemului.
* `class-diagram-before.png` - Designul initial defectuos.
* `class-diagram-after.png` - Designul refactorizat.
* `sequence-place-order.md` - Fluxul de plasare a comenzii (Mermaid).
* `sequence-cancel-order.md` - Fluxul de anulare a comenzii (Mermaid).

## Decizii de Modelare

**1. Diagrama Use Case**
Am delimitat clar actorii (Client, Admin, Sistem de Plata). Am folosit `«include»` pentru actiunile dependente obligatorii (procesarea platii la comanda) si `«extend»` pentru actiunile conditionale (aplicarea unui cod de reducere).

**2. Diagramele de Clasa**
* **Before:** Am modelat problema clasica de *God Class* si *Cuplare Mare*. `OrderManager` facea de toate si depindea direct de implementari concrete (`SqlDatabase`, `SmtpMailer`).
* **After:** Am refactorizat folosind abstractii. Am introdus interfetele `IOrderRepository` si `IEmailService`. Noua clasa `OrderService` depinde doar de aceste interfete, obtinand astfel o **cuplare mica** si o **coeziune mare**.

**3. Diagramele de Secventa**
Modelate in Mermaid pentru a fi usor versionabile. Am ilustrat ordinea apelurilor si am folosit fragmente de tip `alt` pentru deciziile critice de business:
* Oprirea fluxului daca plata este respinsa.
* Interzicerea anularii daca statusul comenzii este deja "Expediata".