# 🚀 FOPA - Function-Oriented Programming Architecture

**The architectural standard for the modern web: No classes, no `this`, no hidden state.**

FOPA (*Function-Oriented Programming Architecture*) is a paradigm shift in how we build Full-Stack JavaScript and TypeScript applications. It moves away from the legacy Object-Oriented Programming (OOP) model and embraces a pure, pragmatic functional approach tailored for modern runtimes like Serverless, Edge, and React Server Actions.

---

## ⚠️ The Problem: The OOP Illusion in JS
For years, developers have forced Object-Oriented patterns into Node.js and web environments, leading to:
- **The `this` trap:** Unpredictable context binding and chaotic execution.
- **Hidden State:** Variables mutating silently inside monolithic class instances.
- **Serverless Inefficiency:** Heavy class instantiations that increase memory usage and Cold Start times in environments like AWS Lambda or Vercel.
- **Frontend/Backend Disconnect:** Building UI with functional React Hooks while forcing the backend to use legacy Class-based services.

## 💡 The Solution: FOPA
FOPA completely eliminates classes and hidden state. It relies strictly on **functions and closures** to create scalable, predictable, and cloud-native applications. 

---

## 🏛️ The 3 Core Principles

### 1. State is a Function (or its byproduct)
Inspired by the React Hooks philosophy (`useState`), state in FOPA is never trapped inside a class property or mutated directly. Accessing or modifying state (whether in memory or a database) is strictly a functional instruction. If it is not a function or the result of one, it is not FOPA.

### 2. Closure Injection (Zero `this`)
When dependencies are needed (e.g., a database client), FOPA uses Function Factories (Closures). The parent function receives the dependency and returns the operational child functions. There are no constructors. The `class` and `this` keywords are strictly forbidden.

### 3. Explicit Data Flow (Data-Driven)
Data flows transparently: arguments go in, transformed data comes out. Complex, persistent state lives entirely outside the application layer (in PostgreSQL, Redis, or the URL). The codebase acts purely as a stateless transformation pipeline.

---

## 🥊 Code Comparison: OOP vs FOPA

### ❌ The Legacy Way (OOP)
```typescript
// Tightly coupled, relies on 'this', requires instantiation
export class SubscriptionManager {
  private db: PrismaClient;

  constructor(prisma: PrismaClient) {
    this.db = prisma; // State is trapped inside the instance
  }

  async extendSubscription(userId: string, newDate: Date) {
    return await this.db.user.update({
      where: { id: userId },
      data: { expiration_date: newDate },
    });
  }
}

// Forced execution:
const manager = new SubscriptionManager(new PrismaClient());
await manager.extendSubscription('123', new Date());
```

### ✅ The FOPA Way
```typescript
// Clean, modular, encapsulated by closures, ready to export
export const createSubscriptionActions = (db: PrismaClient) => {
  
  // 'db' lives in the lexical scope (closure), no 'this' required
  const extendSubscription = async (userId: string, newDate: Date) => {
    return await db.user.update({
      where: { id: userId },
      data: { expiration_date: newDate },
    });
  };

  // Return only the behavior
  return { extendSubscription };
};

// Pure injection and destructuring:
const { extendSubscription } = createSubscriptionActions(new PrismaClient());
await extendSubscription('123', new Date());
```

---

## 📂 Architecture Schema

FOPA ditches the traditional MVC structure in favor of Feature-Based isolation. 

```text
fopa-architecture/
├── 📂 src/
│   ├── 📂 app/                  # Frontend Layer (UI). Only consumes functions.
│   │
│   ├── 📂 features/             # 🧠 The Core. Grouped by domain.
│   │   ├── 📂 subscriptions/
│   │   │   ├── 📄 actions.ts    # Next.js Server Actions (Callable endpoints)
│   │   │   ├── 📄 factory.ts    # Closures: Dependency injection and logic
│   │   │   └── 📄 types.ts      # Pure data structures (Interfaces)
│   │
│   ├── 📂 core/                 # Infrastructure & External State
│   │   ├── 📄 db.ts             # Prisma / SQL instances
│   │   └── 📄 cache.ts          # External memory (Redis)
│   │
│   └── 📂 shared/               # Pure utility functions (Math, formatting)
│       └── 📄 utils.ts
```

---

## 🛠️ Step-by-Step Guide

### 1. Clone the Boilerplate
```bash
git clone https://github.com/your-username/fopa-architecture.git
cd fopa-architecture
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Build your first Feature
1. Define your data structures in `types.ts`.
2. Create your logic inside a closure in `factory.ts`.
3. Inject your database from `core/db.ts` into your factory inside `actions.ts`.
4. Export the resulting function and consume it directly in your React components.

---

## 📚 References & Inspiration
This architecture is the natural evolution and unification of several battle-tested concepts:
*   **Data-Oriented Design (DOD):** Separating data structures from logic execution.
*   **The Module Pattern:** Douglas Crockford's approach to JavaScript privacy using closures.
*   **Functional Core, Imperative Shell:** Keeping the core logic pure and pushing side-effects to the boundaries.
*   **React Hooks:** The paradigm shift that proved state management doesn't require classes.

---
*Join the function-oriented resistance. Zero classes. Zero `this`. Just code that works.*
