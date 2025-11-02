# React 18 Learning Plan
*Detailed curriculum from zero to job-ready*

## 📚 **LEVEL 1: FOUNDATIONS** (Weeks 1-4, 60 hours)

### Module 1.1: Development Environment Setup (Week 1, 15 hours)

<details>
<summary><strong>Learning Objectives</strong></summary>

- Set up professional React development environment
- Master essential development tools and workflows  
- Understand version control basics for React projects
- Configure debugging and productivity tools

</details>

#### **Concepts to Master**
- **Node.js & NPM**: Package management, version management, global vs local packages
- **Code Editor**: VS Code extensions, settings, shortcuts, integrated terminal
- **Version Control**: Git basics, .gitignore patterns, commit best practices
- **Browser Tools**: DevTools, React DevTools extension, debugging techniques

#### **Hands-On Tasks**
1. Install Node.js (LTS version) and verify NPM installation
2. Set up VS Code with React development extensions
3. Create first project using `create-react-app` or Vite
4. Initialize Git repository and make first commit
5. Install and configure React DevTools
6. Practice debugging with browser DevTools

#### **Reinforcement Plan**
- **Daily**: Practice Git commands (add, commit, push)
- **Review**: Environment troubleshooting checklist
- **Flashcards**: NPM commands, VS Code shortcuts
- **Quiz**: Tool identification and usage scenarios

#### **Mini-Project: Development Environment Portfolio**
**Goal**: Create a simple "Hello World" React app and deploy it

**Acceptance Criteria**:
- ✅ React app runs locally without errors
- ✅ Git repository with meaningful commit messages
- ✅ Deployed to GitHub Pages or Netlify
- ✅ README with setup instructions
- ✅ Uses proper project structure

**Time Estimate**: 3-4 hours

#### **Interview Questions**
1. **Q**: What is NPM and how is it different from Node.js?
   **A**: NPM is the package manager for Node.js, used to install and manage JavaScript libraries and tools.

2. **Q**: Explain the purpose of .gitignore in a React project.
   **A**: .gitignore prevents unnecessary files (node_modules, build files, env files) from being committed to version control.

---

### Module 1.2: JavaScript Prerequisites (Week 2, 15 hours)

<details>
<summary><strong>Learning Objectives</strong></summary>

- Master ES6+ features essential for React development
- Understand asynchronous JavaScript patterns
- Practice functional programming concepts
- Apply modern JavaScript in React context

</details>

#### **Concepts to Master**
- **ES6+ Features**: Arrow functions, destructuring, template literals, modules
- **Array Methods**: map(), filter(), reduce(), find(), some(), every()
- **Object Manipulation**: Spread operator, object destructuring, property shorthand
- **Async Programming**: Promises, async/await, error handling
- **Functional Concepts**: Pure functions, immutability, higher-order functions

#### **Hands-On Tasks**
1. Refactor traditional functions to arrow functions
2. Practice array transformations using map, filter, reduce
3. Implement async data fetching with fetch API
4. Create utility functions using modern JavaScript
5. Practice object and array destructuring patterns
6. Build mini-exercises with each concept

#### **Reinforcement Plan**
- **Daily**: Code 3 array method exercises
- **Weekly**: Review functional programming principles
- **Flashcards**: ES6 syntax patterns and use cases
- **Practice**: Convert ES5 code to modern JavaScript

#### **Mini-Project: JavaScript Utility Library**
**Goal**: Create a collection of utility functions using modern JavaScript

**Acceptance Criteria**:
- ✅ 10+ utility functions covering different concepts
- ✅ Unit tests for each function
- ✅ Documentation with examples
- ✅ ES6+ syntax throughout
- ✅ Handles edge cases and errors

**Time Estimate**: 4-5 hours

#### **Interview Questions**
1. **Q**: What's the difference between map() and forEach()?
   **A**: map() returns a new array with transformed elements; forEach() executes a function for each element without returning anything.

2. **Q**: Explain the difference between Promise and async/await.
   **A**: async/await is syntactic sugar over Promises, making asynchronous code look more like synchronous code.

---

### Module 1.3: React Core Concepts (Week 3, 15 hours)

<details>
<summary><strong>Learning Objectives</strong></summary>

- Understand React fundamentals and Virtual DOM
- Master JSX syntax and rules
- Create and compose functional components
- Handle props and component communication
- Implement basic event handling

</details>

#### **Concepts to Master**
- **React Fundamentals**: What is React, Virtual DOM, component-based architecture
- **JSX**: Syntax rules, expressions, conditional rendering, lists
- **Components**: Functional components, component composition, reusability
- **Props**: Passing data, prop types, default props, children prop
- **Events**: Synthetic events, event handlers, event object

#### **Hands-On Tasks**
1. Create multiple functional components
2. Practice JSX expressions and conditional rendering
3. Build component hierarchy with props
4. Implement various event handlers
5. Create reusable UI components
6. Practice component composition patterns

#### **Reinforcement Plan**
- **Daily**: Build one new component
- **Review**: JSX syntax rules checklist
- **Flashcards**: React concepts and terminology
- **Practice**: Convert HTML to JSX exercises

#### **Mini-Project: Interactive Component Library**
**Goal**: Build a set of reusable UI components

**Acceptance Criteria**:
- ✅ 5+ reusable components (Button, Card, Input, etc.)
- ✅ Props for customization
- ✅ Event handling examples
- ✅ Component composition demonstration
- ✅ Clean JSX following best practices

**Time Estimate**: 5-6 hours

#### **Interview Questions**
1. **Q**: What is the Virtual DOM and why does React use it?
   **A**: Virtual DOM is a JavaScript representation of the real DOM that allows React to efficiently update the UI by diffing and batching changes.

2. **Q**: What are the rules for JSX?
   **A**: JSX must return a single parent element, use camelCase for attributes, close all tags, and use curly braces for JavaScript expressions.

---

### Module 1.4: Props and Component Communication (Week 4, 15 hours)

<details>
<summary><strong>Learning Objectives</strong></summary>

- Master prop passing and validation
- Understand component hierarchy and data flow
- Handle complex props and prop drilling
- Implement callback props for child-to-parent communication
- Learn component composition patterns

</details>

#### **Concepts to Master**
- **Props Patterns**: Object props, array props, function props, children prop
- **Prop Validation**: PropTypes, TypeScript interfaces (intro)
- **Data Flow**: One-way data flow, lifting state up concept
- **Communication**: Parent-to-child, child-to-parent via callbacks
- **Composition**: Component composition vs inheritance

#### **Hands-On Tasks**
1. Build multi-level component hierarchies
2. Practice different prop patterns
3. Implement callback props for communication
4. Create flexible components using children prop
5. Practice prop drilling and understand its limitations
6. Build components with validation

#### **Reinforcement Plan**
- **Daily**: Practice prop patterns
- **Review**: Component communication patterns
- **Flashcards**: Prop types and patterns
- **Exercise**: Debug prop-related issues

#### **Mini-Project: Data Display Dashboard**
**Goal**: Create a dashboard showing data through component hierarchy

**Acceptance Criteria**:
- ✅ Parent component managing data
- ✅ Multiple child components receiving props
- ✅ Callback props for user interactions
- ✅ Proper data flow demonstration
- ✅ Component composition examples

**Time Estimate**: 6-7 hours

#### **Interview Questions**
1. **Q**: How do you pass data from child to parent component?
   **A**: Use callback props - pass a function from parent to child, child calls the function with data.

2. **Q**: What is prop drilling and how can you avoid it?
   **A**: Prop drilling is passing props through multiple component levels. Avoid with Context API, state management, or component composition.

---

## 🔧 **LEVEL 2: INTERMEDIATE** (Weeks 5-8, 80 hours)

### Module 2.1: State Management with Hooks (Week 5, 20 hours)

<details>
<summary><strong>Learning Objectives</strong></summary>

- Master useState Hook patterns and best practices
- Understand useEffect Hook and dependency management
- Implement controlled vs uncontrolled components
- Handle complex state scenarios
- Debug common state-related issues

</details>

#### **Concepts to Master**
- **useState Patterns**: Primitive state, object state, array state, functional updates
- **useEffect**: Mounting, updating, cleanup, dependency arrays, common mistakes
- **Controlled Components**: Form inputs, validation, two-way binding
- **State Best Practices**: State structure, immutability, performance considerations
- **Common Pitfalls**: Stale closures, infinite loops, incorrect dependencies

#### **Hands-On Tasks**
1. Build forms with controlled inputs
2. Create todo list with useState
3. Implement data fetching with useEffect
4. Practice state updates and immutability
5. Debug common useEffect issues
6. Build interactive components

#### **Reinforcement Plan**
- **Daily**: Build state-driven components
- **Weekly**: Review Hook rules and best practices
- **Flashcards**: useState patterns, useEffect scenarios
- **Debug**: Common state-related problems

#### **Mini-Project: Interactive Todo Application**
**Goal**: Build a fully functional todo app with local storage

**Acceptance Criteria**:
- ✅ Add, edit, delete, toggle todos
- ✅ Filter todos (all, active, completed)
- ✅ Local storage persistence
- ✅ Proper state management
- ✅ No prop drilling

**Time Estimate**: 8-10 hours

#### **Interview Questions**
1. **Q**: When does useEffect run?
   **A**: After every render by default, or when dependencies change if a dependency array is provided.

2. **Q**: How do you update state based on previous state?
   **A**: Use functional updates: setState(prevState => newState) to avoid stale closures.

---

### Module 2.2: Advanced Hooks (Week 6, 20 hours)

<details>
<summary><strong>Learning Objectives</strong></summary>

- Master useRef for DOM access and mutable values
- Understand useCallback and useMemo for optimization
- Implement useReducer for complex state logic
- Create custom hooks for reusable logic
- Apply performance optimization techniques

</details>

#### **Concepts to Master**
- **useRef**: DOM references, mutable values, focus management
- **useCallback**: Function memoization, dependency optimization
- **useMemo**: Value memoization, expensive computations
- **useReducer**: Complex state logic, action patterns, state machines
- **Custom Hooks**: Logic extraction, reusability, hook composition

#### **Hands-On Tasks**
1. Build focus management with useRef
2. Optimize components with useCallback/useMemo
3. Implement complex state with useReducer
4. Create custom hooks for common patterns
5. Performance profile before/after optimization
6. Build hook library

#### **Reinforcement Plan**
- **Daily**: Practice one advanced hook
- **Weekly**: Performance optimization review
- **Flashcards**: Hook use cases and patterns
- **Project**: Custom hook challenges

#### **Mini-Project: Custom Hook Library**
**Goal**: Create a collection of reusable custom hooks

**Acceptance Criteria**:
- ✅ 5+ custom hooks (useLocalStorage, useCounter, useFetch, etc.)
- ✅ Proper hook composition
- ✅ Documentation and examples
- ✅ TypeScript support (optional)
- ✅ Unit tests for hooks

**Time Estimate**: 10-12 hours

#### **Interview Questions**
1. **Q**: When should you use useCallback vs useMemo?
   **A**: useCallback memoizes functions, useMemo memoizes values. Use when passing to memoized components or expensive computations.

2. **Q**: How do custom hooks enable code reuse?
   **A**: Custom hooks extract stateful logic into reusable functions that can be shared across components.

---

### Module 2.3: React Router v6+ (Week 7, 20 hours)

<details>
<summary><strong>Learning Objectives</strong></summary>

- Implement client-side routing with React Router
- Handle dynamic routes and parameters
- Create protected routes and authentication flows
- Master programmatic navigation
- Build nested routing structures

</details>

#### **Concepts to Master**
- **Router Setup**: BrowserRouter, Routes, Route components
- **Navigation**: Link, NavLink, useNavigate hook
- **Dynamic Routes**: URL parameters, query strings, useParams
- **Protected Routes**: Authentication guards, conditional routing
- **Nested Routes**: Outlet component, relative paths

#### **Hands-On Tasks**
1. Set up basic routing structure
2. Create navigation menu with active states
3. Implement dynamic product pages
4. Build protected route wrapper
5. Create nested layout components
6. Handle 404 and error pages

#### **Reinforcement Plan**
- **Daily**: Practice routing patterns
- **Weekly**: Review router concepts
- **Flashcards**: Router hooks and components
- **Exercise**: Complex routing scenarios

#### **Mini-Project: Multi-Page React Application**
**Goal**: Build a complete multi-page application with routing

**Acceptance Criteria**:
- ✅ Multiple pages with navigation
- ✅ Dynamic routes with parameters
- ✅ Protected routes with authentication
- ✅ Nested layouts
- ✅ 404 error handling

**Time Estimate**: 10-12 hours

#### **Interview Questions**
1. **Q**: How does React Router handle client-side routing?
   **A**: React Router intercepts navigation events and renders different components based on the URL, without full page reloads.

2. **Q**: How do you implement protected routes?
   **A**: Create a wrapper component that checks authentication state and redirects to login if not authenticated.

---

### Module 2.4: Forms and Validation (Week 8, 20 hours)

<details>
<summary><strong>Learning Objectives</strong></summary>

- Master controlled and uncontrolled forms
- Implement form validation strategies
- Use React Hook Form for complex forms
- Handle form submission and error states
- Create reusable form components

</details>

#### **Concepts to Master**
- **Form Patterns**: Controlled vs uncontrolled, form libraries
- **Validation**: Client-side validation, schema validation (Zod/Yup)
- **React Hook Form**: Performance benefits, validation integration
- **Error Handling**: Field errors, form errors, UX patterns
- **Accessibility**: Form labels, ARIA attributes, keyboard navigation

#### **Hands-On Tasks**
1. Build controlled forms with validation
2. Implement React Hook Form
3. Create schema validation with Zod
4. Handle complex form scenarios
5. Build reusable form components
6. Add accessibility features

#### **Reinforcement Plan**
- **Daily**: Build different form patterns
- **Weekly**: Form validation review
- **Flashcards**: Validation patterns, accessibility
- **Practice**: Form debugging exercises

#### **Mini-Project: User Registration System**
**Goal**: Build complete user registration and profile forms

**Acceptance Criteria**:
- ✅ Multi-step registration form
- ✅ Real-time validation
- ✅ Error handling and display
- ✅ Accessible form design
- ✅ Form persistence between steps

**Time Estimate**: 10-12 hours

#### **Interview Questions**
1. **Q**: What's the difference between controlled and uncontrolled components?
   **A**: Controlled components have their state managed by React, uncontrolled components manage their own state internally.

2. **Q**: Why use React Hook Form over plain controlled components?
   **A**: Better performance (fewer re-renders), built-in validation, easier handling of complex forms.

---

## 🚀 **LEVEL 3: ADVANCED** (Weeks 9-12, 90 hours)

*[Content continues with similar detailed structure for Advanced level...]*

## 💼 **LEVEL 4: PROFESSIONAL** (Weeks 13-16, 70 hours)

*[Content continues with similar detailed structure for Professional level...]*

## 🎯 **LEVEL 5: JOB-READY** (Weeks 17-20, 50 hours)

*[Content continues with similar detailed structure for Job-Ready level...]*

---

## 📊 **Total Time Investment**

| Level | Modules | Hours | Focus Areas |
|-------|---------|-------|-------------|
| Foundations | 4 | 60 | Environment, JS, React basics |
| Intermediate | 4 | 80 | State, routing, forms |
| Advanced | 4 | 90 | Data fetching, performance, testing |
| Professional | 4 | 70 | TypeScript, styling, deployment |
| Job-Ready | 4 | 50 | Portfolio, interviews, industry |
| **TOTAL** | **20** | **350** | **Complete React mastery** |

---

**Next**: Continue with [Timeline Schedule](03-timeline.md) for week-by-week planning