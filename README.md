# 🌍 Lost in Translation — T-Shirt Store

Un e-commerce fullstack per la vendita di magliette con stampe in diverse lingue, costruito con React + Vite per il frontend e Node.js + Express + PostgreSQL per il backend.

---

## 📋 Indice

- [Panoramica](#panoramica)
- [Funzionalità](#funzionalità)
- [Tecnologie utilizzate](#tecnologie-utilizzate)
- [Struttura del progetto](#struttura-del-progetto)
- [Prerequisiti](#prerequisiti)
- [Installazione e avvio](#installazione-e-avvio)
- [Configurazione del database](#configurazione-del-database)
- [Variabili d'ambiente](#variabili-dambiente)
- [API principali](#api-principali)
- [Pagine e componenti](#pagine-e-componenti)
- [Note di sicurezza](#note-di-sicurezza)

---

## Panoramica

**Lost in Translation** è uno store online che vende magliette personalizzate con scritte in più lingue (Italiano, English, Français e altre). Ogni prodotto può essere acquistato in diverse taglie e varianti linguistiche. L'applicazione include un sistema di autenticazione, un profilo utente, un carrello persistente, un flusso di checkout completo e una dashboard amministrativa.

---

## Funzionalità

### Utente
- Registrazione e login con autenticazione JWT
- Recupero e reimpostazione password via email
- Visualizzazione e modifica del profilo personale
- Navigazione del catalogo prodotti con dettaglio per prodotto
- Selezione di taglia e lingua della maglietta
- Carrello persistente (salvato in `localStorage`)
- Checkout con inserimento dati di spedizione e metodo di pagamento
- Conferma ordine via email
- Aggiunta di recensioni e valutazioni a stelle nella homepage

### Amministratore
- Dashboard dedicata accessibile solo agli admin
- Gestione prodotti (aggiunta, modifica, eliminazione)
- Visualizzazione e gestione ordini (con filtri e ricerca)
- Gestione utenti
- Paginazione su ordini, prodotti e utenti

---

## Tecnologie utilizzate

### Frontend (`client/`)
| Tecnologia | Versione | Utilizzo |
|---|---|---|
| React | 19 | Framework UI |
| Vite | 7 | Build tool e dev server |
| React Router DOM | 7 | Routing client-side |
| Axios | 1.x | Chiamate HTTP |
| Zustand | 5 | State management |
| Styled Components | 6 | Stili componenti |
| Tailwind CSS | 4 | Utility CSS |
| Lucide React | 0.542 | Icone |
| React Icons | 5 | Icone aggiuntive |
| React Country Flag | 3 | Bandiere per selezione lingua |
| bad-words | 4 | Filtro contenuti nei commenti |

### Backend (`server/`)
| Tecnologia | Versione | Utilizzo |
|---|---|---|
| Node.js + Express | 5.x | Server HTTP e routing |
| PostgreSQL | — | Database relazionale |
| Sequelize | 6 | ORM |
| bcrypt / bcryptjs | — | Hash delle password |
| jsonwebtoken | 9 | Autenticazione JWT |
| nodemailer | 7 | Invio email (recupero password, conferme ordine) |
| dotenv | 17 | Gestione variabili d'ambiente |
| cors | 2 | Cross-Origin Resource Sharing |

---

## Struttura del progetto

```
Lost-in-Translation-master/
└── t-shirt-store/
    ├── client/                    # Frontend React + Vite
    │   ├── public/
    │   │   └── Maglie/            # Immagini dei prodotti
    │   └── src/
    │       ├── components/        # Componenti riutilizzabili
    │       │   ├── CommentForm.jsx
    │       │   ├── FeedbackPopup.jsx
    │       │   ├── Footer.jsx
    │       │   ├── Header.jsx
    │       │   ├── ProductCard.jsx
    │       │   └── StarRating.jsx
    │       ├── pages/             # Pagine dell'applicazione
    │       │   ├── Home.jsx
    │       │   ├── ProductList.jsx
    │       │   ├── ProductDetail.jsx
    │       │   ├── Cart.jsx
    │       │   ├── Checkout.jsx
    │       │   ├── OrderConfirmation.jsx
    │       │   ├── Login.jsx
    │       │   ├── Register.jsx
    │       │   ├── Profilo.jsx
    │       │   ├── RecuperoPassword.jsx
    │       │   ├── ReimpostaPassword.jsx
    │       │   ├── About.jsx
    │       │   ├── Contact.jsx
    │       │   └── AdminDashboard.jsx
    │       ├── App.jsx
    │       ├── config.js          # URL base delle API
    │       └── main.jsx
    └── server/                    # Backend Node.js + Express
        ├── config/
        │   └── database.js        # Configurazione Sequelize
        ├── models/
        │   ├── User.js
        │   ├── Product.js
        │   └── index.js
        ├── routes/
        │   ├── userRoutes.js
        │   └── productRoutes.js
        ├── server.js              # Entry point del server
        └── .env                   # Variabili d'ambiente (non committare!)
```

---

## Prerequisiti

- **Node.js** v18 o superiore
- **npm** v9 o superiore
- **PostgreSQL** v14 o superiore (in esecuzione sulla porta configurata nel `.env`)

---

## Installazione e avvio

### 1. Clona il repository

```bash
git clone https://github.com/tuo-utente/Lost-in-Translation.git
cd Lost-in-Translation/t-shirt-store
```

### 2. Avvia il backend

```bash
cd server
npm install
npm start
```

Il server partirà su `http://localhost:3001`.

### 3. Avvia il frontend

```bash
cd ../client
npm install
npm run dev
```

Il client sarà disponibile su `http://localhost:5173` (o l'indirizzo mostrato da Vite).

---

## Configurazione del database

Il server crea automaticamente le tabelle al primo avvio tramite la funzione `setupDatabase()`. Le tabelle create sono:

- `users` — utenti registrati con dati anagrafici, indirizzo e flag `is_admin`
- `product` — prodotti con nome, descrizione, prezzo, taglie, lingue e immagini
- `orders` — ordini con riferimento all'utente, importo, dati cliente e stato
- `order_items` — righe d'ordine con prodotto, taglia, lingua e quantità
- `comments` — recensioni degli utenti con valutazione a stelle

> ⚠️ **Attenzione**: in `models/index.js` è presente `sync({ force: true })` che **ricrea le tabelle ad ogni riavvio**. In produzione, rimuovere l'opzione `force: true` o usare le migration di Sequelize.

---

## Variabili d'ambiente

Crea il file `server/.env` (un esempio è già incluso nel progetto). Le variabili necessarie sono:

```env
DB_USER=postgres
DB_HOST=localhost
DB_DATABASE=Lost_in_translation
DB_PASSWORD=your_password
DB_PORT=5432

EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password

JWT_SECRET=your_very_long_random_secret

URL=http://localhost:3001
FRONTEND_URL=http://localhost:5173
```

> ⚠️ **Non committare mai il file `.env`** con credenziali reali. Assicurarsi che sia nel `.gitignore`.

---

## API principali

Il server espone le seguenti rotte REST su `http://localhost:3001`:

### Autenticazione e Utenti
| Metodo | Endpoint | Descrizione |
|---|---|---|
| `POST` | `/api/register` | Registrazione nuovo utente |
| `POST` | `/api/login` | Login e generazione JWT |
| `GET` | `/api/me` | Dati dell'utente autenticato |
| `PUT` | `/api/me` | Aggiornamento profilo |
| `POST` | `/api/recupero-password` | Invio email di reset password |
| `POST` | `/api/reset-password/:token` | Reimpostazione password |

### Prodotti
| Metodo | Endpoint | Descrizione |
|---|---|---|
| `GET` | `/api/products` | Lista di tutti i prodotti |
| `GET` | `/api/products/:id` | Dettaglio prodotto |
| `POST` | `/api/products` | Creazione prodotto (admin) |
| `PUT` | `/api/products/:id` | Modifica prodotto (admin) |
| `DELETE` | `/api/products/:id` | Eliminazione prodotto (admin) |

### Ordini
| Metodo | Endpoint | Descrizione |
|---|---|---|
| `POST` | `/api/orders` | Creazione ordine + invio email di conferma |
| `GET` | `/api/orders` | Lista ordini (admin) |
| `PUT` | `/api/orders/:id/status` | Aggiornamento stato ordine (admin) |

### Commenti
| Metodo | Endpoint | Descrizione |
|---|---|---|
| `GET` | `/api/comments` | Lista commenti e valutazioni |
| `POST` | `/api/comments` | Aggiunta nuovo commento |
| `DELETE` | `/api/comments/:id` | Eliminazione commento (admin) |

---

## Pagine e componenti

| Percorso | Pagina | Descrizione |
|---|---|---|
| `/` | Home | Homepage con recensioni e valutazioni |
| `/products` | ProductList | Catalogo magliette |
| `/product/:id` | ProductDetail | Dettaglio e selezione taglia/lingua |
| `/cart` | Cart | Carrello con gestione quantità |
| `/checkout` | Checkout | Form dati di spedizione e pagamento |
| `/conferma-ordine` | OrderConfirmation | Riepilogo post-acquisto |
| `/login` | Login | Accesso account |
| `/register` | Register | Creazione account |
| `/profilo` | Profilo | Gestione dati personali |
| `/recupero-password` | RecuperoPassword | Richiesta reset via email |
| `/reset-password/:token` | ReimpostaPassword | Inserimento nuova password |
| `/about` | About | Pagina informativa |
| `/contact` | Contact | Form di contatto |
| `/admin` | AdminDashboard | Pannello amministrativo (solo admin) |

---

## Note di sicurezza

- Le password sono hashate con **bcrypt** prima di essere salvate nel database.
- L'autenticazione usa **JWT** con scadenza a 1 ora.
- L'accesso alla dashboard admin è protetto lato server verificando il flag `is_admin` dell'utente.
- Il filtro **bad-words** è applicato ai commenti degli utenti per prevenire contenuti inappropriati.
- Il file `.env` contiene credenziali sensibili: verificare che sia sempre incluso nel `.gitignore`.
- Per un ambiente di produzione, generare un nuovo `JWT_SECRET` robusto e non riutilizzare quello di sviluppo.
