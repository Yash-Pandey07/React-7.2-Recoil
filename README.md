
#React + Vite
This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

@vitejs/plugin-react uses Babel for Fast Refresh.
@vitejs/plugin-react-swc uses SWC for Fast Refresh.
React-7.2-Recoil
Week 7.2: Context API & Recoil
This lecture covers the drawbacks of the Context API for state management and introduces Recoil as an alternative. It focuses on Recoil's core elements like RecoilRoot, atoms, selectors, and hooks, demonstrating how Recoil simplifies state management in React.

State Management
State management involves handling and maintaining the state of an application. In frontend development, this refers to variables representing the app's current condition. React offers multiple ways to manage state:

Local Component State: Managed via useState hook and confined to the component.
Context API: Used for sharing global state across components without passing props.
State Management Libraries: Advanced methods using libraries like Redux, Recoil, etc.
Problem with Context API
While the Context API solves prop drilling, it triggers re-renders across all consumers, which may hurt performance. To mitigate this, memoization (useMemo or React.memo) and libraries like Recoil provide fine-grained control.

Introduction to Recoil
Recoil, developed by Facebook, is a powerful state management library that introduces features like atoms, selectors, and a global state tree, addressing prop drilling challenges and offering scalability.

Core Concepts in Recoil
1. RecoilRoot: This component is the root of the Recoil state tree, providing context for Recoil atoms and selectors. It must wrap the top-level component to allow access to Recoil states.
```
import { RecoilRoot } from 'recoil';
import App from './App';

const RootComponent = () => (
  <RecoilRoot>
    <App />
  </RecoilRoot>
);

export default RootComponent;
```
2. Atom: An atom represents a piece of state that multiple components can read or update.
```
import { atom } from 'recoil';

export const countState = atom({
  key: 'countState', // unique ID
  default: 0,        // default value
});

```
3. Recoil Hooks:

i) useRecoilState: Get and set atom state.
ii) useRecoilValue: Get atom state without setting it.
iii) useSetRecoilState: Set atom state without subscribing.
Example:
```
const [count, setCount] = useRecoilState(countState);
const count = useRecoilValue(countState);
const setCount = useSetRecoilState(countState);

```
###Selectors in Recoil
Selectors compute derived state from atoms or other selectors.

1. Creating a Selector:
```
import { selector } from 'recoil';

const doubledCountSelector = selector({
  key: 'doubledCount',
  get: ({ get }) => {
    const count = get(countState);
    return count * 2;
  },
});

```
2. Using Selectors:
```
const doubledCount = useRecoilValue(doubledCountSelector);

   ```
Recoil Code Implementation
Steps to create a Recoil-powered React app:

Install Recoil:
```
npm install recoil

```
2. Atom (countState.js):
```
import { atom } from 'recoil';

export const countState = atom({
  key: 'countState',
  default: 0,
});

```
3. Counter component (Counter.jsx):
```
   import { useRecoilState } from 'recoil';
import { countState } from '../store/atoms/countState';

const Counter = () => {
  const [count, setCount] = useRecoilState(countState);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increase</button>
      <button onClick={() => setCount(count - 1)}>Decrease</button>
    </div>
  );
};

export default Counter;

```
4. App.js:
```
import { RecoilRoot } from 'recoil';
import Counter from './components/Counter';

function App() {
  return (
    <RecoilRoot>
      <Counter />
    </RecoilRoot>
  );
}

export default App;


```
