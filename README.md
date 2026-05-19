# Design Patterns — Riepilogo per Esame (Singleton, Strategy, Observer)

Questo file contiene teoria, esempi e diagrammi UML ASCII per i 3 pattern principali richiesti all'esame.  
Utile per domande a crocette, riconoscimento pattern ed esercizi di disegno UML.

---

## Indice
1. Singleton
2. Strategy
3. Observer
4. Tabella riassuntiva
5. Consigli per l'esame

---

## 1. Singleton

### Obiettivo
Garantire che una classe abbia **una sola istanza** e fornire un **punto di accesso globale** a tale istanza.

### Quando usarlo
- Logger
- Connessione a database
- Printer spooler
- Window manager
- Gestore di configurazione

### Struttura chiave
- Costruttore **privato** (`-`)
- Attributo **statico e privato** che tiene l'unica istanza
- Metodo **statico e pubblico** `getInstance()`

### Diagramma UML (ASCII)

┌─────────────────┐
│ Singleton │
├─────────────────┤
│ - instance │
│ : Singleton │
├─────────────────┤
│ - Singleton() │
│ + getInstance() │
│ : Singleton │
└─────────────────┘

### Esempio mentale (Java)
```java
public class Singleton {
    private static Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) instance = new Singleton();
        return instance;
    }
}
Attenzione in esame
❌ Non confondere con variabile globale

✅ Il costruttore deve essere privato

✅ getInstance() è statico

2. Strategy
Obiettivo
Definire una famiglia di algoritmi intercambiabili, incapsularli e renderli intercambiabili a runtime.

Quando usarlo
Navigatore (macchina/treno/bici)

Pagamenti (carta/PayPal/crypto)

Notifiche (email/SMS/push)

Struttura chiave
Context → ha un riferimento a Strategy

Strategy → interfaccia con un metodo (es. esegui())

ConcreteStrategy → implementano Strategy

Diagramma UML (ASCII)
┌─────────────┐          ┌─────────────────────┐
│  Context    │          │   <<interface>>     │
├─────────────┤          │   Strategy          │
│ - strategy  │─────────►├─────────────────────┤
├─────────────┤          │ + esegui()          │
│ + setStrategy()        └─────────────────────┘
│ + esegui()                     △
└─────────────┘                   │
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
              ▼                   ▼                   ▼
      ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
      │ ConcreteS1   │    │ ConcreteS2   │    │ ConcreteS3   │
      ├──────────────┤    ├──────────────┤    ├──────────────┤
      │ + esegui()   │    │ + esegui()   │    │ + esegui()   │
      └──────────────┘    └──────────────┘    └──────────────┘
Esempio mentale (Java)
java
interface StrategiaPagamento {
    void paga(int importo);
}

class Carta implements StrategiaPagamento { 
    public void paga(int importo) { /* paga con carta */ }
}

class PayPal implements StrategiaPagamento { 
    public void paga(int importo) { /* paga con PayPal */ }
}

class Carrello {
    private StrategiaPagamento strategia;
    public void setStrategia(StrategiaPagamento s) { this.strategia = s; }
    public void paga(int importo) { strategia.paga(importo); }
}

Attenzione in esame
❌ "Notifica" NON è Strategy (è Observer)

✅ Le ConcreteStrategy implementano l'interfaccia

3. Observer
Obiettivo
Definire una dipendenza uno-a-molti tra oggetti: quando un oggetto cambia stato, tutti i dipendenti vengono notificati automaticamente.

Quando usarlo
GUI (pulsante che notifica finestre)

Spreadsheet (grafico che si aggiorna quando cambiano i dati)

Struttura chiave
Subject → tiene lista di Observer, metodi attach(), detach(), notify()

Observer → interfaccia con update()

ConcreteObserver → implementa update() e si registra al Subject

Diagramma UML (ASCII)
text
┌─────────────┐          ┌─────────────────────┐
│  Subject    │          │   <<interface>>     │
├─────────────┤          │   Observer          │
│ - observers │─────────►├─────────────────────┤
├─────────────┤          │ + update()          │
│ + attach()  │          └─────────────────────┘
│ + detach()  │                    △
│ + notify()  │                    │
└─────────────┘                    │
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                    │
              ▼                    ▼                    ▼
      ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
      │ ConcreteObs1 │     │ ConcreteObs2 │     │ ConcreteObs3 │
      ├──────────────┤     ├──────────────┤     ├──────────────┤
      │ + update()   │     │ + update()   │     │ + update()   │
      └──────────────┘     └──────────────┘     └──────────────┘
Esempio mentale (Java)
java
interface Observer {
    void update(String messaggio);
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    public void attach(Observer o) { observers.add(o); }
    public void detach(Observer o) { observers.remove(o); }
    public void notify(String msg) {
        for (Observer o : observers) o.update(msg);
    }
}

class ConcreteObserver implements Observer {
    private String nome;
    public ConcreteObserver(String nome) { this.nome = nome; }
    public void update(String msg) { 
        System.out.println(nome + " ha ricevuto: " + msg); 
    }
}
Attenzione in esame
✅ Parole chiave: notifica, aggiornamento automatico, subscribe

✅ Observer non sa chi lo notifica (low coupling)

4. Tabella riassuntiva (da memorizzare)
Singleton	una sola istanza	costruttore privato + metodo statico	getInstance()
Strategy	algoritmi intercambiabili	Context → Strategy (interfaccia)	implementa, runtime
Observer	notifica automatica	Subject → Observer (interfaccia)	attach, notify, update

5. Consigli per l'esame
Domande a crocette
Se leggo "un'istanza" → Singleton

Se leggo "algoritmi intercambiabili" → Strategy

Se leggo "notifica automatica a più oggetti" → Observer

Esercizi di disegno UML
Singleton: una sola casella, costruttore -, attributo statico -, metodo statico +
Strategy: tre elementi: Context → interfaccia ← ConcreteStrategies
Observer: Subject con lista, freccia verso interfaccia Observer, ConcreteObserver con update()

Errori comuni da evitare
❌ Mettere costruttore public in Singleton

❌ Chiamare new per ottenere l'istanza Singleton

❌ Confondere "notifica" (Observer) con "algoritmo intercambiabile" (Strategy)


6. Riepilogo finale visivo (stampa mentale)
SINGLETON                               STRATEGY
┌─────────────┐                        ┌─────────┐         ┌────────────┐
│ Singleton   │                        │ Context │────────►│ <<interface│
├─────────────┤                        └─────────┘         │ Strategy   │
│ -instance   │                                              └────────────┘
│ -Singleton()│        OBSERVER                                 △
│ +getInstance│        ┌─────────┐         ┌────────────┐       │
└─────────────┘        │ Subject │────────►│ <<interface│       │
                       └─────────┘         │ Observer   │       │
                       │ -observers│       └────────────┘       │
                       │ +attach() │              △             │
                       │ +notify() │              ┼─────────────┘
                       └───────────┘              │
                                            ┌─────┴─────┐
                                            │ Concrete  │
                                            │ Observer  │
                                            └───────────┘

