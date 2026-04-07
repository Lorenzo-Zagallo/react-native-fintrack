# 💰 FinTrack: Personal Finance Assistance (Android Mobile Project)

## 🎯 About the Project

**FinTrack** is a mobile financial control application developed in **React Native + Expo** for the Android environment. The goal is to provide a complete tool for managing personal finances (income and expenses), utilizing the **React Context API** for global state management and **Firebase Firestore** for cloud data persistence with real-time synchronization.

This project was created for the course **Development on Android Mobile Devices**, demonstrating proficiency in using cross-platform technologies, architectural patterns (DAO), and cloud service integration to create robust and scalable solutions.

### Core Features

The application covers the following key areas:

1. **Transaction CRUD:** Add, list (ordered by date), edit, and delete incomes and expenses with real-time synchronization.
2. **Category CRUD:** Add, edit, and delete custom categories using Ionicons.
3. **Budget Management:** Set spending limits (goals) per category and track progress with a visual progress bar and over-limit alerts.
4. **Dashboard (Overview):** Dynamically displays total balance, total income, and total expenses.
5. **Reports & Charts:** Features pie charts showing the distribution of income and expenses for the current month, as well as a financial evolution line chart.
6. **Cloud Persistence:** Uses **Firebase Firestore** to securely save all transactions, categories, and budgets.
7. **Real-time Synchronization:** Data changes appear instantly across all connected devices.
8. **Offline Support:** Firestore maintains a local cache, allowing offline use with automatic sync once the connection is restored.

---

## 🛠️ Tech Stack

| Category | Technology | Justification |
| :--- | :--- | :--- |
| **Mobile Framework** | React Native (with Expo) | Allows building native apps for Android (and iOS) using JavaScript. |
| **Language** | JavaScript (ES6+) | The standard for React Native development. |
| **Navigation** | React Navigation | Manages the flow between screens (Tabs & Stacks). |
| **State Management** | React Context API | Centralizes and provides global state (transactions, categories) and CRUD functions. |
| **Backend/Database** | Firebase Firestore | Cloud NoSQL database with real-time sync and offline support. |
| **Data Access Layer** | DAO Pattern | Isolates database access logic into dedicated modules (e.g., `CategoriaDAO`). |
| **Authentication** | Firebase Auth | *(Planned)* User management and access control. |
| **Hosting** | Firebase Hosting | Deployment for the web version of the application. |
| **Visualization** | `react-native-chart-kit` | Library for creating interactive pie and line charts. |
| **UI Components** | `@react-native-picker/picker` | Native component for category selection in forms. |
| **Icons** | `@expo/vector-icons` | Vector icon library (Ionicons) for the user interface. |

---

## 🚀 How to Run the Project

### Prerequisites

Ensure you have the following installed:

1. **Node.js (LTS):** JavaScript runtime environment - [Download](https://nodejs.org/)
2. **npm** (or Yarn): Package manager (comes with Node.js)
3. **Expo CLI (Globally):** `npm install -g expo-cli`
4. **Visual Studio Code (VS Code):** Recommended code editor
5. **Expo Go App:** Installed on your Android phone (Google Play Store) or on an Android emulator
6. **Google/Firebase Account:** To set up the project in the Firebase Console

### Firebase Setup

1. **Create a project in the [Firebase Console](https://console.firebase.google.com/).**
2. **Enable Firestore Database:**
   - Go to "Build" → "Firestore Database" → "Create database".
   - Choose Test Mode (temporarily) or configure your security rules.
3. **Register the Web App:**
   - Go to "Project Overview" → "Add app" → Select Web (`</>`).
   - Copy the `firebaseConfig` credentials.
4. **Configure Credentials:**
   - Open the `config/firebaseConfig.js` file in the project.
   - Replace the placeholder credentials with your actual Firebase credentials.
5.  **Configure Firestore Rules (optional):**
   - In the Firebase Console, go to "Firestore Database" → "Rules"
   - For development, use:
      ```javascript
      rules_version = '2';
      service cloud.firestore {
        match /databases/{database}/documents {
          match /{document=**} {
            allow read, write: true;
          }
        }
      }
      ```
   - **⚠️ Important:** For production, implement appropriate security rules

### Execution Steps

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/Lorenzo-Zagallo/react-native-fintrack.git
    cd react-native-fintrack
    ```

2.  **Install Dependencies:**
    ```bash
    npm install
    ```

3.  **Configure Firebase:**
    - Edit `config/firebaseConfig.js` with your credentials

4.  **Start the Development Server:**
    ```bash
    npm start
    ```
    or to clear the cache:
    ```bash
    npm start -c
    ```

5.  **Run on Android Device/Emulador:**

    * **Android Device:** Open the **Expo Go** app and scan the QR Code displayed in the terminal.
    * **Android Emulator:** Press `a` in the terminal where Expo is running.
    * **Web Navigator:** Press `w` in the terminal (for testing purposes only, some native features may not work)

### Useful Commands

```bash
npm start          # Starts the development server
npm start -c       # Starts with a clean cache
npx expo start     # Alternative to npm start
```

### Troubleshooting

- **Firebase connection error:** Verify that the credentials in `firebaseConfig.js` are correct.
- **Cache error:** Run `npm start -c` to clear the Metro Bundler cache.
- **QR Code not working:** Make sure your phone and computer are on the same Wi-Fi network.
- **Missing dependencies:** Delete `node_modules` and `package-lock.json`, then run `npm install` again.

---

## 📂 Folder Structure

```
.
├── assets/             # Icons, fonts, and static images
├── config/             # Firebase configuration
│   └── firebaseConfig.js
├── context/            # Global state management logic
│   └── ContextoFinancas.js
├── dao/                # Data Access Object layer
│   ├── CategoriaDAO.js
│   ├── TransacaoDAO.js
│   ├── OrcamentoDAO.js
│   └── MetaDAO.js
├── navigation/         # Navigation setup (Tabs and Stacks)
│   ├── TabNavigation.js
│   ├── StackTransacao.js
│   ├── StackOrcamento.js
│   ├── StackPainel.js
│   └── StackRelatorio.js
├── screens/            # Main application screens and forms
│   ├── Dashboard/      # Main overview screen
│   │   └── TelaPainel.js
│   ├── Transactions/   # Transaction management
│   │   ├── TelaTransacao.js
│   │   └── TelaAddTransacao.js
│   ├── Budgets/        # Budget and category management
│   │   ├── TelaOrcamento.js
│   │   └── TelaAddCategoria.js
│   └── Reports/        # Charts and financial reports
│       └── TelaRelatorio.js
├── .firebaserc         # Firebase project configuration
├── firebase.json       # Firebase Hosting configuration
├── App.js              # Main entry point
├── index.js            # Application bootstrap
└── package.json        # Project dependencies
```

---

## 🔥 Project Architecture

### DAO Pattern (Data Access Object)

The project implements the **DAO** pattern to separate business logic from data access logic, ensuring clean architecture:

- **CategoriaDAO.js**: CRUD for categories (add, update, delete, listen)
- **TransacaoDAO.js**: CRUD for transactions sorted by date
- **OrcamentoDAO.js**: CRUD for budgets by category
- **MetaDAO.js**: CRUD for financial goals

### Real-Time Data Flow

1. **UI (Screens)** → triggers an action (e.g., Add Transaction).
2. **Context (ContextoFinancas.js)** → validates the input and calls the appropriate DAO.
3. **DAO** → executes the operation on Firestore (`addDoc`, `updateDoc`, `deleteDoc`)
4. **Firestore** → persists the data and notifies all active listeners.
5. **DAO (`onSnapshot`)** → detects the change and triggers a callback.
6. **Context** → updates the global state (`setState`).
7. **UI** → automatically re-renders with the fresh data.

### Real-Time Synchronization

The Context establishes **listeners** (via `onSnapshot`) for all Firestore collections:

```javascript
useEffect(() => {
    const unsubscribeCategorias = CategoriaDAO.ouvirCategorias(setCategorias);
    const unsubscribeTransacoes = TransacaoDAO.ouvirTransacoes(setTransacoes);
    const unsubscribeOrcamentos = OrcamentoDAO.ouvirOrcamentos(setOrcamentos);
    
    return () => {
        unsubscribeCategorias();
        unsubscribeTransacoes();
        unsubscribeOrcamentos();
    };
}, []);
```

Any change to the database (from any device) automatically updates all connected clients.

---

## 🗄️ Database Structure (Firebase Firestore)

### Collections:

#### 1. `categorias`
```javascript
{
  id: "auto-gerado",        // Unique Firestore ID
  nome: "Alimentação",      // Category name
  tipo: "expense",          // "income" | "expense"
  cor: "#FF6384"            // Color for graphics (hex)
}
```

#### 2. `transacoes`
```javascript
{
  id: "auto-gerado",        // Unique Firestore ID
  tipo: "expense",          // "income" | "expense"
  valor: 150.50,            // Transaction value
  descricao: "Almoço",      // Description
  categoriaId: "ref-id",    // Category reference
  data: "2025-11-24"        // Date format YYYY-MM-DD
}
```

#### 3. `orcamentos`
```javascript
{
  id: "categoria-id",       // Category ID (document)
  categoriaId: "ref-id",    // Category reference
  valorLimite: 500.00       // Spending limit
}
```

#### 4. `metas`
```javascript
{
  id: "auto-gerado",        // Unique Firestore ID
  nome: "Viagem 2025",      // Goal name
  descricao: "...",         // Detailed description
  valorAlvo: 5000.00,       // Target value
  valorAtual: 1200.00,      // Amount already saved
  prazo: "2025-12-31"       // Deadline
}
```

---

## 🎨 Detailed Features Detalhadas

### 1. Transaction Management
- ✅ Add income and expenses with field validation
- ✅ List all transactions sorted by date (newest first)
- ✅ Edit existing transactions
- ✅ Delete transactions with confirmation
- ✅ Filter by category in the picker (only categories of the selected type)
- ✅ Date input mask (DD/MM/YYYY)

### 2. Category Management
- ✅ Create custom categories (income or expense)
- ✅ Ionicons icon selection
- ✅ Edit existing categories
- ✅ Cascading deletion (removes transactions and associated budgets)
- ✅ Duplicate name validation

### 3. Budget Management
- ✅ Set spending limit per category
- ✅ Visual progress bar (% spent vs. budget)
- ✅ Visual alerts (80% = yellow, >100% = red)
- ✅ Automatic calculation of expenses by category in the current month

### 4. Dashboard
- ✅ Total balance (income - expenses)
- ✅ Total income for the period
- ✅ Total expenses for the period
- ✅ Real-time updates

### 5. Reports and Graphs
- ✅ Pie chart: distribution of expenses by category (current month)
- ✅ Pie chart: distribution of income by category (current month)
- ✅ Line graph: financial evolution (last 6 months)
- ✅ Custom colors by category

---

## 🔒 Security and Best Practices

### Implemented:

- ✅ Input validation in all forms
- ✅ Error handling with try/catch in Firebase operations
- ✅ Global error state (`error`) exposed in the Context
- ✅ Normalization of numeric values ​​before saving
- ✅ Cascading deletion to maintain data consistency

### To Implement (Roadmap):
- 🔜 Firebase Authentication (login with Google/email)
- 🔜 Firestore security rules (access only to own data)
- 🔜 User permission validation
- 🔜 Encryption of sensitive data
- 🔜 Rate limiting and abuse protection

---

## 👥 Development Team

### **Lorenzo Zagallo**
- Frontend & User Interface Development
- Screen implementation and navigation flow
- Presentation logic and UI/UX Design

### **Matheus Almeida**
- Full Firebase integration (Firestore + Hosting)
- Implementation of the DAO architecture
- Real-time synchronization and documentation

---

## 📚 Resources and Documentation

- [React Native Documentation](https://reactnative.dev/)
- [Expo Documentation](https://docs.expo.dev/)
- [Firebase Documentation](https://firebase.google.com/docs)
- [React Navigation](https://reactnavigation.org/)
- [Firestore Guide](https://firebase.google.com/docs/firestore)

---

## 📄 License & Credits

This project was developed for academic purposs during the **Mobile Device Programming (Android)** course at UNESA (2025).

---

## 👨‍💻 Contact and Author

* **Students:** Lorenzo Zagallo & Matheus Fonseca
* **Subject:** Programação Para Dispositivos Móveis em Android
* **Institution:** UNESA - Universidade Estácio de Sá
* **Year:** 2025
  
This project is no longer under development. Contributions, suggestions, and constructive criticism are welcome!
