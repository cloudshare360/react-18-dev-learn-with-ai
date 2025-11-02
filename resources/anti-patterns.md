# React Anti-Patterns & Common Pitfalls
*Avoiding common mistakes and bad practices in React development*

## 🚨 **Critical Anti-Patterns**

### **1. Mutating State Directly**
```javascript
// ❌ WRONG: Direct state mutation
const [users, setUsers] = useState([]);

// Don't do this!
const addUser = (newUser) => {
  users.push(newUser); // Mutates existing array
  setUsers(users); // React won't detect the change
};

// ✅ CORRECT: Immutable updates
const addUser = (newUser) => {
  setUsers(prevUsers => [...prevUsers, newUser]);
  // or
  setUsers(prevUsers => prevUsers.concat(newUser));
};

// For objects
const [user, setUser] = useState({ name: '', email: '' });

// ❌ WRONG
const updateEmail = (email) => {
  user.email = email; // Direct mutation
  setUser(user);
};

// ✅ CORRECT
const updateEmail = (email) => {
  setUser(prevUser => ({ ...prevUser, email }));
};
```

**Why it's bad**: React uses Object.is() comparison to detect state changes. Mutating state directly means React won't trigger re-renders.

---

### **2. Incorrect useEffect Dependencies**
```javascript
// ❌ WRONG: Missing dependencies
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetchUser(userId).then(setUser); // Uses userId but not in deps
  }, []); // Missing userId dependency
  
  return <div>{user?.name}</div>;
}

// ✅ CORRECT: Include all dependencies
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // Include userId
  
  return <div>{user?.name}</div>;
}

// ❌ WRONG: Stale closure
function Counter() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const timer = setInterval(() => {
      setCount(count + 1); // Always uses initial count value
    }, 1000);
    
    return () => clearInterval(timer);
  }, []); // Missing count dependency
  
  return <div>{count}</div>;
}

// ✅ CORRECT: Functional update
function Counter() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const timer = setInterval(() => {
      setCount(prevCount => prevCount + 1); // Uses current value
    }, 1000);
    
    return () => clearInterval(timer);
  }, []); // No dependencies needed with functional update
  
  return <div>{count}</div>;
}
```

**Why it's bad**: Missing dependencies cause stale closures and bugs. Extra dependencies cause unnecessary re-runs.

---

### **3. Using Array Index as Key**
```javascript
// ❌ WRONG: Using array index as key
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}> {/* Don't use index as key */}
          {todo.text}
        </li>
      ))}
    </ul>
  );
}

// ✅ CORRECT: Using stable, unique identifiers
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}> {/* Use unique, stable ID */}
          {todo.text}
        </li>
      ))}
    </ul>
  );
}

// ❌ WRONG: When list order can change
const items = ['apple', 'banana', 'cherry'];
// If user can reorder items, index keys will cause issues

// ✅ CORRECT: Generate stable keys if no ID exists
function ItemList({ items }) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={`${item}-${index}`}> {/* Better but not ideal */}
          {item}
        </li>
      ))}
    </ul>
  );
}

// ✅ BEST: Use proper IDs from data
const itemsWithIds = [
  { id: 'apple-1', name: 'apple' },
  { id: 'banana-2', name: 'banana' }
];
```

**Why it's bad**: Index keys can cause performance issues and bugs when list order changes.

---

## 💥 **Performance Anti-Patterns**

### **4. Creating Objects/Functions in Render**
```javascript
// ❌ WRONG: Creating new objects/functions every render
function UserCard({ user, onUpdate }) {
  return (
    <div>
      <h3>{user.name}</h3>
      {/* New object created every render */}
      <button 
        style={{ color: 'blue', padding: '10px' }}
        onClick={() => onUpdate(user.id)} // New function every render
      >
        Update
      </button>
    </div>
  );
}

// ✅ CORRECT: Move static objects outside component
const buttonStyle = { color: 'blue', padding: '10px' };

function UserCard({ user, onUpdate }) {
  // Memoize the click handler
  const handleClick = useCallback(() => {
    onUpdate(user.id);
  }, [onUpdate, user.id]);
  
  return (
    <div>
      <h3>{user.name}</h3>
      <button style={buttonStyle} onClick={handleClick}>
        Update
      </button>
    </div>
  );
}

// ❌ WRONG: Expensive calculations in render
function ProductList({ products, filters }) {
  // This runs on every render!
  const filteredProducts = products.filter(product => 
    product.category === filters.category &&
    product.price >= filters.minPrice &&
    product.price <= filters.maxPrice
  );
  
  return (
    <div>
      {filteredProducts.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}

// ✅ CORRECT: Memoize expensive calculations
function ProductList({ products, filters }) {
  const filteredProducts = useMemo(() => {
    return products.filter(product => 
      product.category === filters.category &&
      product.price >= filters.minPrice &&
      product.price <= filters.maxPrice
    );
  }, [products, filters]);
  
  return (
    <div>
      {filteredProducts.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}
```

**Why it's bad**: Creates unnecessary work for React and can cause child components to re-render unnecessarily.

---

### **5. Overusing useEffect**
```javascript
// ❌ WRONG: Using useEffect for derived state
function UserProfile({ user }) {
  const [displayName, setDisplayName] = useState('');
  
  // Unnecessary useEffect!
  useEffect(() => {
    setDisplayName(user.firstName + ' ' + user.lastName);
  }, [user.firstName, user.lastName]);
  
  return <div>{displayName}</div>;
}

// ✅ CORRECT: Calculate during render
function UserProfile({ user }) {
  const displayName = user.firstName + ' ' + user.lastName;
  
  return <div>{displayName}</div>;
}

// ❌ WRONG: Using useEffect to update state based on props
function Counter({ initialCount }) {
  const [count, setCount] = useState(initialCount);
  
  // This creates synchronization issues
  useEffect(() => {
    setCount(initialCount);
  }, [initialCount]);
  
  return <div>{count}</div>;
}

// ✅ CORRECT: Use key to reset component state
function App() {
  return (
    <Counter 
      key={initialCount} // New key creates new component instance
      initialCount={initialCount} 
    />
  );
}

// Or derive state from props
function Counter({ initialCount }) {
  const [countOffset, setCountOffset] = useState(0);
  const count = initialCount + countOffset;
  
  return <div>{count}</div>;
}
```

**Why it's bad**: useEffect creates additional render cycles and synchronization complexity.

---

## 🔀 **State Management Anti-Patterns**

### **6. Prop Drilling Hell**
```javascript
// ❌ WRONG: Excessive prop drilling
function App() {
  const [user, setUser] = useState(null);
  
  return (
    <Layout user={user} setUser={setUser}>
      <Dashboard user={user} setUser={setUser} />
    </Layout>
  );
}

function Layout({ user, setUser, children }) {
  return (
    <div>
      <Header user={user} setUser={setUser} />
      {children}
    </div>
  );
}

function Header({ user, setUser }) {
  return (
    <nav>
      <UserMenu user={user} setUser={setUser} />
    </nav>
  );
}

function UserMenu({ user, setUser }) {
  // Finally using the props 4 levels deep!
  return <div>Welcome, {user?.name}</div>;
}

// ✅ CORRECT: Use Context for deeply shared state
const UserContext = createContext();

function App() {
  const [user, setUser] = useState(null);
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Layout>
        <Dashboard />
      </Layout>
    </UserContext.Provider>
  );
}

function UserMenu() {
  const { user } = useContext(UserContext);
  return <div>Welcome, {user?.name}</div>;
}
```

**Why it's bad**: Makes components tightly coupled and harder to maintain.

---

### **7. Context Value Recreation**
```javascript
// ❌ WRONG: Creating new context value every render
function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  
  // This object is recreated every render!
  return (
    <AuthContext.Provider value={{ user, setUser }}>
      {children}
    </AuthContext.Provider>
  );
}

// ✅ CORRECT: Memoize context value
function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  
  const contextValue = useMemo(() => ({
    user,
    setUser
  }), [user]);
  
  return (
    <AuthContext.Provider value={contextValue}>
      {children}
    </AuthContext.Provider>
  );
}

// ✅ EVEN BETTER: Separate state and actions
function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  
  const actions = useMemo(() => ({
    setUser,
    login: (credentials) => {/* login logic */},
    logout: () => {/* logout logic */}
  }), []); // Actions don't change
  
  return (
    <AuthStateContext.Provider value={user}>
      <AuthActionsContext.Provider value={actions}>
        {children}
      </AuthActionsContext.Provider>
    </AuthStateContext.Provider>
  );
}
```

**Why it's bad**: Causes all context consumers to re-render unnecessarily.

---

## 🎨 **Component Design Anti-Patterns**

### **8. God Components**
```javascript
// ❌ WRONG: Massive component doing everything
function UserDashboard() {
  const [user, setUser] = useState(null);
  const [posts, setPosts] = useState([]);
  const [comments, setComments] = useState([]);
  const [notifications, setNotifications] = useState([]);
  const [settings, setSettings] = useState({});
  const [friends, setFriends] = useState([]);
  
  // 50+ lines of useEffect hooks
  useEffect(() => {
    // Fetch user data
  }, []);
  
  useEffect(() => {
    // Fetch posts
  }, [user]);
  
  // ... more effects
  
  // 100+ lines of event handlers
  const handleUserUpdate = () => { /* ... */ };
  const handlePostCreate = () => { /* ... */ };
  const handleCommentAdd = () => { /* ... */ };
  // ... more handlers
  
  // 200+ lines of JSX
  return (
    <div>
      {/* Massive amount of JSX */}
    </div>
  );
}

// ✅ CORRECT: Break into smaller components
function UserDashboard() {
  return (
    <div className="dashboard">
      <UserProfile />
      <PostsList />
      <NotificationPanel />
      <SettingsPanel />
    </div>
  );
}

function UserProfile() {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetchUser().then(setUser);
  }, []);
  
  return <div>{/* User profile UI */}</div>;
}

function PostsList() {
  const [posts, setPosts] = useState([]);
  
  useEffect(() => {
    fetchPosts().then(setPosts);
  }, []);
  
  return <div>{/* Posts list UI */}</div>;
}
```

**Why it's bad**: Hard to test, maintain, and reason about. Violates single responsibility principle.

---

### **9. Mixed Concerns**
```javascript
// ❌ WRONG: Business logic mixed with presentation
function ProductCard({ product }) {
  const [discounted, setDiscounted] = useState(false);
  
  // Business logic in UI component
  const calculateDiscount = () => {
    const now = new Date();
    const productDate = new Date(product.createdAt);
    const daysDiff = (now - productDate) / (1000 * 60 * 60 * 24);
    
    if (daysDiff > 30 && product.category === 'electronics') {
      return product.price * 0.8;
    }
    if (daysDiff > 60) {
      return product.price * 0.7;
    }
    return product.price;
  };
  
  // API calls in UI component
  const handlePurchase = async () => {
    try {
      const response = await fetch('/api/purchase', {
        method: 'POST',
        body: JSON.stringify({ productId: product.id })
      });
      // Handle response...
    } catch (error) {
      // Handle error...
    }
  };
  
  return (
    <div>
      <h3>{product.name}</h3>
      <p>${calculateDiscount()}</p>
      <button onClick={handlePurchase}>Buy Now</button>
    </div>
  );
}

// ✅ CORRECT: Separate concerns
// Business logic in custom hook
function useProductPricing(product) {
  return useMemo(() => {
    const now = new Date();
    const productDate = new Date(product.createdAt);
    const daysDiff = (now - productDate) / (1000 * 60 * 60 * 24);
    
    if (daysDiff > 30 && product.category === 'electronics') {
      return product.price * 0.8;
    }
    if (daysDiff > 60) {
      return product.price * 0.7;
    }
    return product.price;
  }, [product]);
}

// API logic in service/hook
function usePurchase() {
  return useMutation({
    mutationFn: (productId) => purchaseProduct(productId),
    onSuccess: () => {
      // Handle success
    },
    onError: () => {
      // Handle error
    }
  });
}

// Clean presentation component
function ProductCard({ product }) {
  const price = useProductPricing(product);
  const { mutate: purchase, isLoading } = usePurchase();
  
  return (
    <div>
      <h3>{product.name}</h3>
      <p>${price}</p>
      <button 
        onClick={() => purchase(product.id)}
        disabled={isLoading}
      >
        {isLoading ? 'Processing...' : 'Buy Now'}
      </button>
    </div>
  );
}
```

**Why it's bad**: Makes components hard to test and reuse. Violates separation of concerns.

---

## 🐛 **Common Bug Patterns**

### **10. Memory Leaks**
```javascript
// ❌ WRONG: Missing cleanup in useEffect
function Timer() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);
    
    // Missing cleanup! Memory leak when component unmounts
  }, []);
  
  return <div>{count}</div>;
}

// ❌ WRONG: Event listeners without cleanup
function WindowSize() {
  const [size, setSize] = useState({ width: 0, height: 0 });
  
  useEffect(() => {
    const handleResize = () => {
      setSize({ width: window.innerWidth, height: window.innerHeight });
    };
    
    window.addEventListener('resize', handleResize);
    // Missing cleanup!
  }, []);
  
  return <div>{size.width} x {size.height}</div>;
}

// ✅ CORRECT: Proper cleanup
function Timer() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);
    
    return () => clearInterval(interval); // Cleanup
  }, []);
  
  return <div>{count}</div>;
}

function WindowSize() {
  const [size, setSize] = useState({ width: 0, height: 0 });
  
  useEffect(() => {
    const handleResize = () => {
      setSize({ width: window.innerWidth, height: window.innerHeight });
    };
    
    window.addEventListener('resize', handleResize);
    
    return () => {
      window.removeEventListener('resize', handleResize); // Cleanup
    };
  }, []);
  
  return <div>{size.width} x {size.height}</div>;
}
```

**Why it's bad**: Causes memory leaks and performance degradation.

---

### **11. Race Conditions**
```javascript
// ❌ WRONG: Race condition with async operations
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    // If userId changes quickly, multiple requests can be in flight
    fetchUser(userId).then(userData => {
      setUser(userData); // May set wrong user data!
    });
  }, [userId]);
  
  return <div>{user?.name}</div>;
}

// ✅ CORRECT: Cancel previous requests
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    let cancelled = false;
    
    fetchUser(userId).then(userData => {
      if (!cancelled) {
        setUser(userData);
      }
    });
    
    return () => {
      cancelled = true; // Cancel if effect re-runs
    };
  }, [userId]);
  
  return <div>{user?.name}</div>;
}

// ✅ BETTER: Use AbortController
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    const abortController = new AbortController();
    
    fetchUser(userId, { signal: abortController.signal })
      .then(userData => setUser(userData))
      .catch(error => {
        if (error.name !== 'AbortError') {
          console.error('Fetch error:', error);
        }
      });
    
    return () => {
      abortController.abort();
    };
  }, [userId]);
  
  return <div>{user?.name}</div>;
}
```

**Why it's bad**: Can cause incorrect data to be displayed and state inconsistencies.

---

## 🛡️ **Security Anti-Patterns**

### **12. XSS Vulnerabilities**
```javascript
// ❌ WRONG: Using dangerouslySetInnerHTML carelessly
function BlogPost({ post }) {
  return (
    <div>
      <h1>{post.title}</h1>
      {/* Dangerous! Can execute scripts */}
      <div dangerouslySetInnerHTML={{ __html: post.content }} />
    </div>
  );
}

// ❌ WRONG: Not sanitizing user input
function CommentForm() {
  const [comment, setComment] = useState('');
  
  const handleSubmit = () => {
    // Sending unsanitized user input to API
    submitComment(comment);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <textarea 
        value={comment}
        onChange={e => setComment(e.target.value)}
      />
      <button type="submit">Submit</button>
    </form>
  );
}

// ✅ CORRECT: Sanitize HTML content
import DOMPurify from 'dompurify';

function BlogPost({ post }) {
  const sanitizedContent = DOMPurify.sanitize(post.content);
  
  return (
    <div>
      <h1>{post.title}</h1>
      <div dangerouslySetInnerHTML={{ __html: sanitizedContent }} />
    </div>
  );
}

// ✅ CORRECT: Validate and sanitize input
function CommentForm() {
  const [comment, setComment] = useState('');
  const [error, setError] = useState('');
  
  const handleSubmit = () => {
    // Client-side validation (server-side validation is also needed!)
    if (comment.trim().length < 3) {
      setError('Comment must be at least 3 characters');
      return;
    }
    
    if (comment.length > 500) {
      setError('Comment must be less than 500 characters');
      return;
    }
    
    // Sanitize input
    const sanitizedComment = comment.trim();
    submitComment(sanitizedComment);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <textarea 
        value={comment}
        onChange={e => setComment(e.target.value)}
        maxLength={500}
      />
      {error && <div className="error">{error}</div>}
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Why it's bad**: Can allow malicious scripts to execute in users' browsers.

---

## 📋 **Anti-Pattern Detection Checklist**

### **Code Review Checklist**
```markdown
## State Management
- [ ] No direct state mutations
- [ ] Proper useEffect dependencies
- [ ] No unnecessary useEffect usage
- [ ] Context values are memoized

## Performance
- [ ] No object/function creation in render
- [ ] Expensive calculations are memoized
- [ ] Proper key props for lists
- [ ] Components are properly memoized when needed

## Component Design
- [ ] Components have single responsibility
- [ ] Business logic separated from presentation
- [ ] No god components (>200 lines)
- [ ] Proper error boundaries

## Memory & Resources
- [ ] useEffect cleanup functions present
- [ ] Event listeners are removed
- [ ] Timers and intervals are cleared
- [ ] AbortController used for async operations

## Security
- [ ] User input is validated and sanitized
- [ ] dangerouslySetInnerHTML used safely
- [ ] No sensitive data in client-side code
- [ ] API endpoints are properly secured
```

### **Automated Detection Tools**
```javascript
// ESLint rules to catch anti-patterns
{
  "rules": {
    "react-hooks/exhaustive-deps": "error",
    "react/no-direct-mutation-state": "error",
    "react/no-array-index-key": "warn",
    "react/no-danger": "warn",
    "react/jsx-no-bind": "warn",
    "react/jsx-no-literals": "off",
    "react/no-multi-comp": "warn"
  }
}

// Custom hook for detecting performance issues
function useRenderCount() {
  const renderCount = useRef(0);
  
  useEffect(() => {
    renderCount.current += 1;
    console.log(`Component rendered ${renderCount.current} times`);
  });
  
  return renderCount.current;
}
```

---

**Remember**: Anti-patterns often emerge gradually as applications grow. Regular code reviews and automated tooling help catch these issues early. Always consider the trade-offs and context when applying these guidelines!