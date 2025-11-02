# React Developer Interview Questions
*Comprehensive technical Q&A for React positions*

## 🎯 **React Fundamentals** (Entry Level)

### **Core Concepts**

#### **Q1: What is React and why would you use it?**
**A**: React is a JavaScript library for building user interfaces, particularly web applications. Key benefits:
- **Component-based architecture**: Reusable, modular code
- **Virtual DOM**: Efficient updates and better performance
- **Unidirectional data flow**: Predictable state management
- **Large ecosystem**: Rich community and library support
- **Developer experience**: Great tooling and debugging capabilities

#### **Q2: Explain the Virtual DOM and its benefits.**
**A**: The Virtual DOM is a JavaScript representation of the actual DOM. React uses it to:
- **Batch updates**: Group multiple changes for efficiency
- **Diff algorithm**: Compare current and previous Virtual DOM trees
- **Minimal updates**: Only update changed elements in the real DOM
- **Performance**: Avoid expensive DOM operations
- **Predictability**: Easier to reason about UI state changes

#### **Q3: What's the difference between functional and class components?**
**A**: 
- **Functional Components**: 
  - Simple functions that return JSX
  - Use hooks for state and lifecycle
  - Preferred in modern React
  - Better performance and testing
- **Class Components**: 
  - ES6 classes extending React.Component
  - Use this.state and lifecycle methods
  - Legacy approach (still supported)
  - More verbose syntax

```javascript
// Functional Component
function Welcome({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Class Component
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

### **JSX & Components**

#### **Q4: What are the rules of JSX?**
**A**: JSX rules include:
- **Single parent**: Must return one parent element (or Fragment)
- **Close all tags**: Self-closing tags need `/>`
- **camelCase attributes**: `className` instead of `class`
- **JavaScript expressions**: Use `{}` for dynamic content
- **Reserved words**: Avoid JavaScript keywords

```javascript
// Valid JSX
return (
  <div className="container">
    <img src={url} alt="description" />
    <p>{message}</p>
  </div>
);
```

#### **Q5: How do you handle conditional rendering in React?**
**A**: Multiple approaches:
```javascript
// Ternary operator
{isLoggedIn ? <Dashboard /> : <Login />}

// Logical AND
{isLoading && <Spinner />}

// If-else with variables
const content = user ? <Profile user={user} /> : <Guest />;
return <div>{content}</div>;

// Function approach
const renderContent = () => {
  if (isLoading) return <Spinner />;
  if (error) return <Error message={error} />;
  return <Data items={items} />;
};
```

---

## 🔧 **State Management & Hooks** (Intermediate)

### **useState & useEffect**

#### **Q6: Explain useState and provide examples of different state types.**
**A**: useState manages component state in functional components:

```javascript
// Primitive state
const [count, setCount] = useState(0);

// Object state
const [user, setUser] = useState({ name: '', email: '' });

// Array state
const [items, setItems] = useState([]);

// Functional updates (important for closures)
const increment = () => setCount(prev => prev + 1);

// State with callback
const [data, setData] = useState(() => {
  return expensiveCalculation();
});
```

#### **Q7: When does useEffect run and how do you control it?**
**A**: useEffect runs after render, controlled by dependency array:

```javascript
// Every render
useEffect(() => {
  console.log('Runs after every render');
});

// Only on mount
useEffect(() => {
  console.log('Runs once on mount');
}, []);

// When dependencies change
useEffect(() => {
  console.log('Runs when count changes');
}, [count]);

// Cleanup function
useEffect(() => {
  const timer = setInterval(() => {}, 1000);
  return () => clearInterval(timer);
}, []);
```

### **Advanced Hooks**

#### **Q8: When would you use useCallback vs useMemo?**
**A**: Both optimize performance through memoization:

**useCallback**: Memoizes functions
```javascript
const handleClick = useCallback(() => {
  onClick(id);
}, [onClick, id]);

// Prevents child re-renders when passed as prop
<ExpensiveChild onEvent={handleClick} />
```

**useMemo**: Memoizes values
```javascript
const expensiveValue = useMemo(() => {
  return items.reduce((sum, item) => sum + item.value, 0);
}, [items]);
```

**When to use**:
- Passing callbacks to memoized components
- Expensive calculations
- Preventing unnecessary re-computations

#### **Q9: How do you create and use custom hooks?**
**A**: Custom hooks extract reusable stateful logic:

```javascript
// Custom hook
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = useCallback(() => setCount(c => c + 1), []);
  const decrement = useCallback(() => setCount(c => c - 1), []);
  const reset = useCallback(() => setCount(initialValue), [initialValue]);
  
  return { count, increment, decrement, reset };
}

// Usage
function Counter() {
  const { count, increment, decrement, reset } = useCounter(10);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

---

## 🚀 **Advanced React** (Senior Level)

### **Performance Optimization**

#### **Q10: How do you optimize React application performance?**
**A**: Multiple optimization strategies:

**1. Component Memoization**:
```javascript
// React.memo for functional components
const ExpensiveComponent = React.memo(({ data }) => {
  return <div>{/* expensive rendering */}</div>;
});

// With custom comparison
const MyComponent = React.memo(({ user }) => {
  return <div>{user.name}</div>;
}, (prevProps, nextProps) => prevProps.user.id === nextProps.user.id);
```

**2. Code Splitting**:
```javascript
// Dynamic imports
const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```

**3. Bundle Analysis**:
- Use webpack-bundle-analyzer
- Remove unused dependencies
- Optimize imports

#### **Q11: Explain React 18's concurrent features.**
**A**: React 18 introduces concurrent rendering:

**startTransition**: Mark non-urgent updates
```javascript
import { startTransition } from 'react';

function handleClick() {
  // Urgent update
  setCount(count + 1);
  
  // Non-urgent update
  startTransition(() => {
    setSearchResults(filterResults(query));
  });
}
```

**useDeferredValue**: Defer expensive updates
```javascript
function SearchResults({ query }) {
  const deferredQuery = useDeferredValue(query);
  const results = useMemo(() => 
    searchData(deferredQuery), [deferredQuery]
  );
  
  return <ResultsList results={results} />;
}
```

**Suspense for Data Fetching**:
```javascript
function App() {
  return (
    <Suspense fallback={<Loading />}>
      <DataComponent />
    </Suspense>
  );
}
```

### **State Management**

#### **Q12: Compare Context API vs Redux. When would you use each?**
**A**: 

**Context API**:
- **Best for**: Simple global state, theme, auth
- **Pros**: Built-in, no extra dependencies, simple setup
- **Cons**: Can cause unnecessary re-renders, no DevTools

```javascript
const UserContext = createContext();

function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
}
```

**Redux/Redux Toolkit**:
- **Best for**: Complex state, time-travel debugging, middleware
- **Pros**: Predictable updates, great DevTools, ecosystem
- **Cons**: More boilerplate, learning curve

```javascript
// Redux Toolkit slice
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1; // Immer makes this immutable
    }
  }
});
```

**Decision Matrix**:
- **Small apps**: Context API
- **Medium apps**: Context + useReducer
- **Large apps**: Redux Toolkit
- **Complex async**: Redux Toolkit + RTK Query

---

## 🔄 **React Patterns & Best Practices**

#### **Q13: Explain the render props pattern.**
**A**: Render props pattern shares code between components using a prop whose value is a function:

```javascript
// Render props component
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  useEffect(() => {
    const handleMouseMove = (e) => {
      setPosition({ x: e.clientX, y: e.clientY });
    };
    
    document.addEventListener('mousemove', handleMouseMove);
    return () => document.removeEventListener('mousemove', handleMouseMove);
  }, []);
  
  return render(position);
}

// Usage
function App() {
  return (
    <MouseTracker
      render={({ x, y }) => (
        <p>Mouse position: {x}, {y}</p>
      )}
    />
  );
}
```

#### **Q14: How do you handle error boundaries in React?**
**A**: Error boundaries catch JavaScript errors in component trees:

```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught:', error, errorInfo);
    // Log to error reporting service
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    
    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary>
      <Header />
      <MainContent />
    </ErrorBoundary>
  );
}
```

**React 18 Alternative** (experimental):
```javascript
function ErrorBoundary({ children, fallback }) {
  return (
    <Suspense fallback={fallback}>
      {children}
    </Suspense>
  );
}
```

---

## 🧪 **Testing**

#### **Q15: How do you test React components?**
**A**: Multiple testing approaches:

**Unit Testing with React Testing Library**:
```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('increments count when button clicked', () => {
  render(<Counter />);
  
  const button = screen.getByRole('button', { name: /increment/i });
  const count = screen.getByText('Count: 0');
  
  fireEvent.click(button);
  
  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

**Testing Custom Hooks**:
```javascript
import { renderHook, act } from '@testing-library/react';
import useCounter from './useCounter';

test('should increment counter', () => {
  const { result } = renderHook(() => useCounter());
  
  act(() => {
    result.current.increment();
  });
  
  expect(result.current.count).toBe(1);
});
```

**Mocking API Calls**:
```javascript
import { rest } from 'msw';
import { setupServer } from 'msw/node';

const server = setupServer(
  rest.get('/api/users', (req, res, ctx) => {
    return res(ctx.json([{ id: 1, name: 'John' }]));
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

---

## 🎨 **Styling & UI**

#### **Q16: What are different ways to style React components?**
**A**: Multiple styling approaches:

**1. CSS Modules**:
```javascript
// Button.module.css
.button {
  background: blue;
  color: white;
}

// Button.js
import styles from './Button.module.css';

function Button() {
  return <button className={styles.button}>Click me</button>;
}
```

**2. Styled Components**:
```javascript
import styled from 'styled-components';

const StyledButton = styled.button`
  background: ${props => props.primary ? 'blue' : 'gray'};
  color: white;
  padding: 8px 16px;
`;

// Usage
<StyledButton primary>Primary Button</StyledButton>
```

**3. Tailwind CSS**:
```javascript
function Button({ primary, children }) {
  const baseClasses = 'px-4 py-2 rounded font-medium';
  const colorClasses = primary 
    ? 'bg-blue-500 text-white' 
    : 'bg-gray-200 text-gray-800';
    
  return (
    <button className={`${baseClasses} ${colorClasses}`}>
      {children}
    </button>
  );
}
```

---

## 💡 **Coding Challenges**

### **Challenge 1: Build a Todo List Component**
**Requirements**:
- Add, edit, delete todos
- Mark as complete/incomplete
- Filter (all, active, completed)
- Local storage persistence

**Solution Approach**:
```javascript
function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [filter, setFilter] = useState('all');
  
  const addTodo = (text) => {
    setTodos(prev => [...prev, { 
      id: Date.now(), 
      text, 
      completed: false 
    }]);
  };
  
  const toggleTodo = (id) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id 
        ? { ...todo, completed: !todo.completed }
        : todo
    ));
  };
  
  const filteredTodos = todos.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });
  
  // Component JSX...
}
```

### **Challenge 2: Implement a Custom useDebounce Hook**
**Requirements**:
- Delay function execution
- Cancel previous timeouts
- Return debounced value

**Solution**:
```javascript
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);
  
  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);
  
  return debouncedValue;
}

// Usage
function SearchInput() {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 500);
  
  useEffect(() => {
    if (debouncedSearchTerm) {
      // Perform search
    }
  }, [debouncedSearchTerm]);
  
  return (
    <input 
      value={searchTerm}
      onChange={e => setSearchTerm(e.target.value)}
    />
  );
}
```

---

## 🎯 **System Design Questions**

#### **Q17: How would you structure a large React application?**
**A**: Recommended folder structure:

```
src/
├── components/          # Reusable UI components
│   ├── common/         # Shared components
│   └── forms/          # Form components
├── pages/              # Page-level components
├── hooks/              # Custom hooks
├── services/           # API calls, external services
├── store/              # State management
├── utils/              # Helper functions
├── types/              # TypeScript types
└── __tests__/          # Test files
```

**Key principles**:
- **Feature-based folders** for large features
- **Barrel exports** for clean imports
- **Separation of concerns** (UI, logic, data)
- **Consistent naming conventions**

#### **Q18: How would you handle state in a large application?**
**A**: Multi-layered approach:

```javascript
// 1. Local state for component-specific data
const [isOpen, setIsOpen] = useState(false);

// 2. Shared state with Context for feature areas
const UserContext = createContext();

// 3. Global state with Redux for app-wide data
const store = configureStore({
  reducer: {
    auth: authSlice,
    ui: uiSlice,
    api: apiSlice
  }
});

// 4. Server state with TanStack Query
function useUserProfile(userId) {
  return useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId)
  });
}
```

---

## 📚 **Quick Reference: Common Patterns**

### **Event Handling**
```javascript
// Prevent default
const handleSubmit = (e) => {
  e.preventDefault();
  // Handle form submission
};

// Pass additional data
const handleClick = (id) => (e) => {
  // Handle click with id
};

// Or using data attributes
const handleClick = (e) => {
  const id = e.target.dataset.id;
};
```

### **List Rendering**
```javascript
// Basic list
{items.map(item => (
  <li key={item.id}>{item.name}</li>
))}

// With conditional rendering
{items.map(item => (
  item.isVisible && (
    <li key={item.id}>{item.name}</li>
  )
))}

// With error boundaries
{items.map(item => (
  <ErrorBoundary key={item.id}>
    <ListItem item={item} />
  </ErrorBoundary>
))}
```

### **Form Patterns**
```javascript
// Controlled input
const [value, setValue] = useState('');

<input 
  value={value}
  onChange={e => setValue(e.target.value)}
/>

// Multiple inputs
const [formData, setFormData] = useState({});

const handleChange = (e) => {
  setFormData(prev => ({
    ...prev,
    [e.target.name]: e.target.value
  }));
};
```

---

**Next**: Practice with [Behavioral Questions](behavioral.md) and [STAR Templates](star-templates.md)