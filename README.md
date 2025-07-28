# React State Management Project Series

## Project Description

This project series demonstrates different approaches to state management in React applications by building an interactive counter application. Starting with React's built-in `useState` hook, we progressively implement more sophisticated state management solutions including Context API and Redux. The project showcases how to share state across multiple components and maintain application-wide data consistency.

## Learning Objectives

By completing these projects, you will:

- Understand fundamental React state management using `useState`
- Learn to implement global state management with Context API
- Master Redux for complex state management scenarios
- Compare different state management solutions
- Implement state persistence across components
- Understand the concept of single source of truth
- Learn to structure applications for scalable state management

## Requirements

### Technical Requirements

- Node.js (v14 or later)
- npm or yarn package manager
- React (v18 or later)
- TypeScript
- Next.js framework
- Redux Toolkit (for the Redux implementation)
- React-Redux bindings

### Development Environment

- Code editor (VS Code recommended)
- Terminal/command line access
- Modern web browser (Chrome, Firefox, or Edge)

## Best Practices

### General React Practices

- **Component Organization**: Keep components small and focused
- **Type Safety**: Utilize TypeScript for type checking
- **Separation of Concerns**: Separate state management from UI components
- **Immutability**: Always treat state as immutable
- **Single Responsibility**: Each component/file should have one primary responsibility

### State Management Specific

#### Context API
- Create context providers at the appropriate level in the component tree
- Use custom hooks for context consumption
- Provide proper TypeScript interfaces for context values

#### Redux
- Follow Redux Toolkit's recommended patterns
- Use slices for modular state management
- Type your Redux store and actions
- Create typed hooks for dispatch and selector usage

#### Performance
- Memoize selectors when necessary
- Avoid unnecessary re-renders with proper state selection
- Consider using Redux middleware for complex side effects

## Project Structure

### Common Files

```
pages/
├── counter-app.tsx          # Main counter application
components/
├── layouts/
│   └── Header.tsx          # Shared header component
└── ...                     # Reusable UI components
```

### Variant-Specific Files

#### useState Version (0x04)
- Simple state management within a single component

#### Context API Version (0x05)
- `context/CountContext.tsx`: Context provider and hooks
- Modified `_app.tsx` to wrap application with provider

#### Redux Version (0x06)
- `store/store.ts`: Redux store configuration
- Updated components to use Redux hooks

## Expected Outcomes

After completing all versions, you will have:

1. A working counter application with three different state management implementations
2. Understanding of when to use each state management solution
3. Practical experience with modern React state management patterns
4. A foundation for building more complex stateful applications
5. Ability to make informed decisions about state management in your projects

> **⚠️ Note:**  
> While copying and pasting code may seem quick and convenient, it often hinders true understanding. To get the most out of this learning experience, we strongly recommend that you:
> 
> - Carefully read and understand the instructions for each task
> - Type the code yourself to internalize the logic and structure
> - Experiment and test your code to see how it works in practice
> 
> Hands-on practice leads to deeper learning and long-term retention. Keep coding!

## Quiz Questions

Great! You've completed the quiz successfully! Keep going! [Show quiz]

## Tasks

### 0. Adding a counter app to our platform with useState Hook
**mandatory**

**Objectives:** We are going to modify our project to include actual functionalities. Duplicate this project `alx-project-0x03` with a new name of `alx-project-0x04`. The Counter App uses react `useState` hook, to maintain an initial state of your count. This helps keep tracking of the counts, and manages the behavior of the counting effect.

**Instructions:**

1. Create a new file `pages/counter-app.tsx`
2. Replace the content of this file with the code below:

```tsx
import { useState } from 'react';

const CounterApp: React.FC = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count > 0 ? count - 1 : 0);
  };

  return (
    <div className="min-h-screen bg-gradient-to-r from-yellow-400 to-pink-500 flex flex-col justify-center items-center text-white">
      {/* Title */}
      <h1 className="text-6xl font-extrabold mb-6">🤖 Fun Counter App 🎉</h1>

      {/* Funny message */}
      <p className="text-lg font-medium mb-4">
        Current count: {count} {count === 0 ? "🙈 No clicks yet!" : count % 10 === 0 && count !== 0 ? "🔥 You're on fire!" : ""}
      </p>

      {/* Counter Display */}
      <div className="text-6xl font-bold mb-8">
        {count}
      </div>

      {/* Buttons */}
      <div className="flex space-x-4">
        <button
          onClick={increment}
          className="bg-green-500 hover:bg-green-600 text-white font-semibold py-3 px-8 rounded-full text-lg transition duration-300 shadow-lg transform hover:scale-105"
        >
          Increment 🚀
        </button>
        <button
          onClick={decrement}
          className="bg-red-500 hover:bg-red-600 text-white font-semibold py-3 px-8 rounded-full text-lg transition duration-300 shadow-lg transform hover:scale-105"
        >
          Decrement 👎
        </button>
      </div>

      {/* Footer message */}
      <p className="mt-8 text-sm text-white opacity-75">
        Keep clicking, who knows what happens at 100? 😏
      </p>
    </div>
  );
}

export default CounterApp;
```

3. Save and close your files
4. Run `npm run dev -- -p 3000` from the terminal
5. From a tab in your browser type `http://localhost:3000` to see the changes made
6. Click on the Counter App button from the home screen
7. Play around with the button and observe the behavior

**Repo:**
- GitHub repository: `alx-project-0x04-setup`
- Directory: `alx-project-0x04`
- File: `pages/counter-app.tsx`

### 1. Include a ContextAPI for global state management
**mandatory**

**Objective:** ContextAPI allows us to maintain a global state for our applications that can be used across multiple components without prop drilling. For our application, we have a Header component, which is completely different from our CounterApp component (mapped to the route `/counter-app`). We want to achieve a feature such that when a button is clicked in one component, the effect can reflect in another component. E.g: Our counter will reflect in both Header and CounterApp Components.

**Instructions:**

1. Duplicate this project `alx-project-0x04` with a new name of `alx-project-0x05`
2. Create a new directory `context/` in the project root directory
3. Create an empty file `CountContext.tsx` under the `context/` directory
4. Replace the content of the `CountContext.tsx` with the following:

```tsx
import { createContext, useContext,  useState, ReactNode } from "react"

interface CountContextProps {
  count: number
  increment: () => void
  decrement: () => void
}

export const CountContext = createContext<CountContextProps | undefined>(undefined)

export const CountProvider = ({ children }: { children: ReactNode}) => {

  const [count, setCount] = useState<number>(0)

  const increment = () => setCount((count ) =>count + 1)
  const decrement = () => setCount((count) => count > 0 ? count - 1 : 0)

  return (
    <CountContext.Provider value={{ count, increment, decrement }}>
      {children}
    </CountContext.Provider>
  )
}

export const useCount = () => {
  const context = useContext(CountContext)

  if (!context) {
    throw new Error("useCount must be within a Count Provider")
  }

  return context
}
```

5. Modify your App Document (`_app.tsx`) to look like this:

```tsx
import Layout from "@/components/layouts/Layout";
import "@/styles/globals.css";
import type { AppProps } from "next/app";
import { CountProvider } from "@/context/CountContext";

export default function App({ Component, pageProps }: AppProps) {
  return (
    <CountProvider>
      <Layout>
        <Component {...pageProps} />
      </Layout>
    </CountProvider>
  )
}
```

6. Modify your `components/layouts/Header.tsx` to look like this:

```tsx
import Link from "next/link";
import Button from "../common/Button";
import { usePathname } from "next/navigation";
import { useCount } from "@/context/CountContext";

const Header: React.FC = () => {

  const pathname = usePathname()
  const { count } = useCount()

  return (
    <header className="fixed w-full bg-white shadow-md">
      <div className="container mx-auto flex justify-between items-center py-6 px-4 md:px-8">
        <Link href="/" className="text-3xl md:text-5xl font-bold text-gray-800 tracking-tight">
          Splash App
        </Link>

        {/* Button Group */}
        <div className="flex gap-4">
          {
            !["/counter-app"].includes(pathname) ? (
              <>
              <Button
            buttonLabel="Sign In"
            buttonBackgroundColor="red"
          />
          <Button
            buttonLabel="Sign Up"
            buttonBackgroundColor="blue"
          /></>
            ) : (
              <p className=" font-semibold text-lg">Current count : {count}</p>
            )
          }

        </div>
      </div>
    </header>
  );
};

export default Header;
```

7. Save and close your files
8. Run `npm run dev -- -p 3000` from the terminal
9. From a tab in your browser type `http://localhost:3000` to see the changes made
10. Click on the Counter App button from the home screen
11. Play around with the button and observe the behavior

**Repo:**
- GitHub repository: `alx-project-0x04-setup`
- Directory: `alx-project-0x05`
- File: `context/CountContext.tsx`, `components/layout/Header.tsx`, `pages/_app.tsx`

### 2. Include a Redux for global state management
**mandatory**

**Objective:** Redux allows us to maintain a global state for our applications that can be used across multiple components without prop drilling just like ContextAPI, but for large scale applications. For our application, we have a Header component, which is completely different from our CounterApp component (mapped to the route `/counter-app`). We want to achieve a feature such that when a button is clicked in one component, the effect can reflect in another component. E.g: Our counter will reflect in both Header and CounterApp Components.

**Instructions:**

1. Duplicate this project `alx-project-0x05` with a new name of `alx-project-0x06`
2. Install redux using the following command:

```bash
npm install redux react-redux @reduxjs/toolkit
npm install @types/react-redux
```

3. Create a new directory `store/` in the project root directory
4. Create an empty file `store.ts` under the `store/` directory
5. Replace the content of the `store.ts` with the following:

```tsx
import { configureStore, createSlice  } from "@reduxjs/toolkit";
import { useDispatch } from "react-redux";

const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    increment: (state) =>{
      state.value += 1
    },
    decrement: (state) => {
      state.value > 0 ? state.value -= 1 : 0
    }
  }
});

const store = configureStore({
  reducer: {
    counter: counterSlice.reducer
  }
})

export const { increment, decrement } = counterSlice.actions

export type RootState = ReturnType<typeof store.getState>
export type AppDispatch = typeof store.dispatch

export const useAppDispatch = () => useDispatch<AppDispatch>()

export default store
```

6. Update the content of `pages/counter-app.tsx` to look like this:

```tsx
import { useSelector } from "react-redux";
import { RootState, useAppDispatch, AppDispatch, increment, decrement } from "@/store/store";

const CounterApp: React.FC = () => {

  const count = useSelector((state: RootState) => state.counter.value)
  const dispatch: AppDispatch = useAppDispatch()

  return (
    <div className="min-h-screen bg-gradient-to-r from-yellow-400 to-pink-500 flex flex-col justify-center items-center text-white">
      {/* Title */}
      <h1 className="text-6xl font-extrabold mb-6">🤖 Fun Counter App 🎉</h1>

      {/* Funny message */}
      <p className="text-lg font-medium mb-4">
        Current count: {count} {count === 0 ? "🙈 No clicks yet!" : count % 10 === 0 && count !== 0 ? "🔥 You're on fire!" : ""}
      </p>

      {/* Counter Display */}
      <div className="text-6xl font-bold mb-8">
        {count}
      </div>

      {/* Buttons */}
      <div className="flex space-x-4">
        <button
          onClick={() => dispatch(increment())}
          className="bg-green-500 hover:bg-green-600 text-white font-semibold py-3 px-8 rounded-full text-lg transition duration-300 shadow-lg transform hover:scale-105"
        >
          Increment 🚀
        </button>
        <button
          onClick={() => dispatch(decrement())}
          className="bg-red-500 hover:bg-red-600 text-white font-semibold py-3 px-8 rounded-full text-lg transition duration-300 shadow-lg transform hover:scale-105"
        >
          Decrement 👎
        </button>
      </div>

      {/* Footer message */}
      <p className="mt-8 text-sm text-white opacity-75">
        Keep clicking, who knows what happens at 100? 😏
      </p>
    </div>
  );
}

export default CounterApp;
```

7. Update the content of `components/layouts/Header.tsx` to look like this:

```tsx
import Link from "next/link";
import Button from "../common/Button";
import { usePathname } from "next/navigation";
import { RootState } from "@/store/store";
import { useSelector } from "react-redux";

const Header: React.FC = () => {

  const pathname = usePathname()
  const count = useSelector((state: RootState) => state.counter.value)

  return (
    <header className="fixed w-full bg-white shadow-md">
      <div className="container mx-auto flex justify-between items-center py-6 px-4 md:px-8">
        <Link href="/" className="text-3xl md:text-5xl font-bold text-gray-800 tracking-tight">
          Splash App
        </Link>

        {/* Button Group */}
        <div className="flex gap-4">
          {
            !["/counter-app"].includes(pathname) ? (
              <>
              <Button
            buttonLabel="Sign In"
            buttonBackgroundColor="red"
          />
          <Button
            buttonLabel="Sign Up"
            buttonBackgroundColor="blue"
          /></>
            ) : (
              <p className=" font-semibold text-lg">Current count : {count}</p>
            )
          }

        </div>
      </div>
    </header>
  );
};

export default Header;
```

8. Save and close your files
9. Run `npm run dev -- -p 3000` from the terminal
10. From a tab in your browser type `http://localhost:3000` to see the changes made
11. Click on the Counter App button from the home screen
12. Play around with the button and observe the behavior

**Repo:**
- GitHub repository: `alx-project-0x04-setup`
- Directory: `alx-project-0x06`