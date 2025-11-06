# Complete React Design Patterns Guide

A comprehensive guide combining React-specific patterns, architectural decisions, and practical JavaScript/TypeScript code examples. This guide covers everything from basic React patterns to advanced enterprise-level architectures.

## Table of Contents

### React-Specific Patterns
- [1. Component Patterns](#1-component-patterns)
- [2. Hook Patterns](#2-hook-patterns)
- [3. State Management Patterns](#3-state-management-patterns)
- [4. Data Flow Patterns](#4-data-flow-patterns)
- [5. Routing Patterns](#5-routing-patterns)
- [6. Form Patterns](#6-form-patterns)
- [7. HTTP & API Patterns](#7-http--api-patterns)
- [8. Testing Patterns](#8-testing-patterns)

### Advanced Patterns
- [9. Performance Patterns](#9-performance-patterns)
- [10. Security Patterns](#10-security-patterns)
- [11. Error Handling Patterns](#11-error-handling-patterns)
- [12. Internationalization Patterns](#12-internationalization-patterns)
- [13. Code Splitting & Lazy Loading](#13-code-splitting--lazy-loading)
- [14. Server-Side Rendering Patterns](#14-server-side-rendering-patterns)

### Decision Tools
- [React Pattern Decision Tree](#react-pattern-decision-tree)
- [Pattern Selection Matrix](#pattern-selection-matrix)
- [Best Practices Checklist](#best-practices-checklist)

---

## React Pattern Decision Tree

### When building React applications...

```
┌─────────────────────────────────────────────────────────────┐
│                    REACT CHALLENGE                           │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   COMPONENT   │ │   STATE    │ │ DATA FLOW   │
        │   STRUCTURE   │ │ MANAGEMENT │ │ PATTERNS    │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ CONTAINER/     │ │ LOCAL     │ │ PROP DRILLING│
        │ PRESENTATIONAL │ │ STATE     │ │ ISSUES       │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ HOOKS          │ │ CONTEXT   │ │ REDUX/      │
        │ PATTERNS       │ │ API       │ │ ZUSTAND     │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## 1. Component Patterns

### Container/Presentational Components (Smart/Dumb)
**Description:** Separates components into container components (with logic) and presentational components (pure UI). Essential for maintainable React applications.

```jsx
// Container Component (Smart)
import React, { useState, useEffect } from 'react';
import UserTable from './UserTable';
import UserSearch from './UserSearch';
import { getUserList, searchUsers, deleteUser } from './api/userService';

const UserListContainer = () => {
  const [users, setUsers] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [searchTerm, setSearchTerm] = useState('');

  useEffect(() => {
    loadUsers();
  }, []);

  const loadUsers = async () => {
    setIsLoading(true);
    try {
      const data = await getUserList();
      setUsers(data);
    } finally {
      setIsLoading(false);
    }
  };

  const handleSearch = async (term) => {
    setSearchTerm(term);
    setIsLoading(true);
    try {
      const data = await searchUsers(term);
      setUsers(data);
    } finally {
      setIsLoading(false);
    }
  };

  const handleEdit = (user) => {
    // Navigate to edit page
    console.log('Edit user:', user);
  };

  const handleDelete = async (user) => {
    await deleteUser(user.id);
    loadUsers();
  };

  return (
    <div className="user-list-container">
      <h2>Users</h2>
      <UserSearch 
        onSearch={handleSearch}
        isLoading={isLoading}
      />
      <UserTable 
        users={users}
        isLoading={isLoading}
        onEdit={handleEdit}
        onDelete={handleDelete}
      />
    </div>
  );
};

export default UserListContainer;

// Presentational Component (Dumb)
const UserTable = ({ users, isLoading, onEdit, onDelete }) => {
  if (isLoading) {
    return <div className="loading">Loading...</div>;
  }

  return (
    <div className="user-table">
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {users.map(user => (
            <tr key={user.id}>
              <td>{user.name}</td>
              <td>{user.email}</td>
              <td>
                <button onClick={() => onEdit(user)}>Edit</button>
                <button onClick={() => onDelete(user)}>Delete</button>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
};

// Presentational Component (Search)
const UserSearch = ({ onSearch, isLoading }) => {
  const [searchTerm, setSearchTerm] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    onSearch(searchTerm);
  };

  return (
    <div className="search-container">
      <form onSubmit={handleSubmit}>
        <input 
          type="text" 
          placeholder="Search users..."
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
          disabled={isLoading}
        />
        <button type="submit" disabled={isLoading}>
          {isLoading ? 'Searching...' : 'Search'}
        </button>
      </form>
    </div>
  );
};
```

### Compound Components Pattern
**Description:** Components that work together to form a complete UI. Allows flexible composition while maintaining a clear API.

```jsx
import React, { createContext, useContext, useState } from 'react';

// Context for compound component
const TabsContext = createContext();

const Tabs = ({ children, defaultTab }) => {
  const [activeTab, setActiveTab] = useState(defaultTab);

  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
};

const TabsList = ({ children }) => {
  return <div className="tabs-list">{children}</div>;
};

const TabsTrigger = ({ value, children }) => {
  const { activeTab, setActiveTab } = useContext(TabsContext);
  const isActive = activeTab === value;

  return (
    <button
      className={`tab-trigger ${isActive ? 'active' : ''}`}
      onClick={() => setActiveTab(value)}
    >
      {children}
    </button>
  );
};

const TabsContent = ({ value, children }) => {
  const { activeTab } = useContext(TabsContext);
  
  if (activeTab !== value) return null;

  return <div className="tab-content">{children}</div>;
};

// Usage
const App = () => {
  return (
    <Tabs defaultTab="profile">
      <TabsList>
        <TabsTrigger value="profile">Profile</TabsTrigger>
        <TabsTrigger value="settings">Settings</TabsTrigger>
        <TabsTrigger value="security">Security</TabsTrigger>
      </TabsList>
      <TabsContent value="profile">
        <p>Profile content</p>
      </TabsContent>
      <TabsContent value="settings">
        <p>Settings content</p>
      </TabsContent>
      <TabsContent value="security">
        <p>Security content</p>
      </TabsContent>
    </Tabs>
  );
};

export { Tabs, TabsList, TabsTrigger, TabsContent };
```

### Higher-Order Component (HOC) Pattern
**Description:** A function that takes a component and returns a new component with additional functionality.

```jsx
import React, { useState, useEffect } from 'react';

// HOC for data fetching
const withDataFetching = (WrappedComponent, fetchFunction) => {
  return (props) => {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
      const loadData = async () => {
        try {
          setLoading(true);
          const result = await fetchFunction(props);
          setData(result);
        } catch (err) {
          setError(err.message);
        } finally {
          setLoading(false);
        }
      };

      loadData();
    }, []);

    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error}</div>;

    return <WrappedComponent {...props} data={data} />;
  };
};

// Usage
const UserList = ({ data }) => {
  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};

const UserListWithData = withDataFetching(
  UserList,
  () => fetch('/api/users').then(res => res.json())
);
```

### Render Props Pattern
**Description:** A component that receives a function as a prop and calls it with data. Provides flexibility in component composition.

```jsx
import React, { useState, useEffect } from 'react';

// Render prop component for data fetching
const DataFetcher = ({ url, children }) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return children({ data, loading, error });
};

// Usage
const UserList = () => {
  return (
    <DataFetcher url="/api/users">
      {({ data, loading, error }) => {
        if (loading) return <div>Loading...</div>;
        if (error) return <div>Error: {error}</div>;
        
        return (
          <ul>
            {data.map(user => (
              <li key={user.id}>{user.name}</li>
            ))}
          </ul>
        );
      }}
    </DataFetcher>
  );
};
```

---

## 2. Hook Patterns

### Custom Hooks Pattern
**Description:** Extract component logic into reusable functions. Promotes code reuse and separation of concerns.

```jsx
import { useState, useEffect } from 'react';

// Custom hook for data fetching
const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
};

// Custom hook for form handling
const useForm = (initialValues, onSubmit) => {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues(prev => ({
      ...prev,
      [name]: value
    }));
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }));
    }
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    setIsSubmitting(true);
    try {
      await onSubmit(values);
    } catch (err) {
      setErrors({ submit: err.message });
    } finally {
      setIsSubmitting(false);
    }
  };

  const setFieldError = (field, error) => {
    setErrors(prev => ({
      ...prev,
      [field]: error
    }));
  };

  return {
    values,
    errors,
    isSubmitting,
    handleChange,
    handleSubmit,
    setFieldError
  };
};

// Custom hook for local storage
const useLocalStorage = (key, initialValue) => {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });

  const setValue = (value) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue];
};

// Usage
const UserProfile = () => {
  const { data, loading, error } = useFetch('/api/user/profile');
  const form = useForm(
    { name: '', email: '' },
    async (values) => {
      await fetch('/api/user/profile', {
        method: 'POST',
        body: JSON.stringify(values)
      });
    }
  );

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <form onSubmit={form.handleSubmit}>
      <input
        name="name"
        value={form.values.name}
        onChange={form.handleChange}
      />
      <input
        name="email"
        value={form.values.email}
        onChange={form.handleChange}
      />
      <button type="submit" disabled={form.isSubmitting}>
        Submit
      </button>
    </form>
  );
};
```

### useReducer Pattern
**Description:** Manages complex state logic with a reducer function. Similar to Redux but local to component.

```jsx
import { useReducer } from 'react';

// Reducer function
const todoReducer = (state, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        ...state,
        todos: [...state.todos, action.payload]
      };
    case 'TOGGLE_TODO':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };
    case 'DELETE_TODO':
      return {
        ...state,
        todos: state.todos.filter(todo => todo.id !== action.payload)
      };
    case 'SET_FILTER':
      return {
        ...state,
        filter: action.payload
      };
    default:
      return state;
  }
};

const TodoApp = () => {
  const [state, dispatch] = useReducer(todoReducer, {
    todos: [],
    filter: 'all'
  });

  const addTodo = (text) => {
    dispatch({
      type: 'ADD_TODO',
      payload: {
        id: Date.now(),
        text,
        completed: false
      }
    });
  };

  const toggleTodo = (id) => {
    dispatch({ type: 'TOGGLE_TODO', payload: id });
  };

  const deleteTodo = (id) => {
    dispatch({ type: 'DELETE_TODO', payload: id });
  };

  const filteredTodos = state.todos.filter(todo => {
    if (state.filter === 'completed') return todo.completed;
    if (state.filter === 'active') return !todo.completed;
    return true;
  });

  return (
    <div>
      <TodoInput onAdd={addTodo} />
      <TodoFilter 
        filter={state.filter}
        onFilterChange={(filter) => dispatch({ type: 'SET_FILTER', payload: filter })}
      />
      <TodoList 
        todos={filteredTodos}
        onToggle={toggleTodo}
        onDelete={deleteTodo}
      />
    </div>
  );
};
```

---

## 3. State Management Patterns

### Context API Pattern
**Description:** Provides a way to pass data through the component tree without prop drilling. Great for global state.

```jsx
import React, { createContext, useContext, useState, useCallback } from 'react';

// Create context
const AuthContext = createContext();

// Provider component
export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);

  const login = useCallback(async (email, password) => {
    setLoading(true);
    try {
      const response = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email, password })
      });
      const userData = await response.json();
      setUser(userData);
      localStorage.setItem('token', userData.token);
    } catch (error) {
      throw error;
    } finally {
      setLoading(false);
    }
  }, []);

  const logout = useCallback(() => {
    setUser(null);
    localStorage.removeItem('token');
  }, []);

  const value = {
    user,
    loading,
    login,
    logout,
    isAuthenticated: !!user
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
};

// Custom hook to use context
export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};

// Usage
const LoginForm = () => {
  const { login, loading } = useAuth();
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      await login(email, password);
    } catch (error) {
      console.error('Login failed:', error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button type="submit" disabled={loading}>
        {loading ? 'Logging in...' : 'Login'}
      </button>
    </form>
  );
};
```

### Redux Pattern
**Description:** Predictable state container for JavaScript apps. Excellent for complex state management.

```jsx
// Redux store setup
import { createStore, combineReducers } from 'redux';
import { Provider, useSelector, useDispatch } from 'react-redux';

// Actions
const ADD_TODO = 'ADD_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';
const DELETE_TODO = 'DELETE_TODO';

// Action creators
export const addTodo = (text) => ({
  type: ADD_TODO,
  payload: { id: Date.now(), text, completed: false }
});

export const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  payload: id
});

export const deleteTodo = (id) => ({
  type: DELETE_TODO,
  payload: id
});

// Reducer
const todosReducer = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return [...state, action.payload];
    case TOGGLE_TODO:
      return state.map(todo =>
        todo.id === action.payload
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    case DELETE_TODO:
      return state.filter(todo => todo.id !== action.payload);
    default:
      return state;
  }
};

const rootReducer = combineReducers({
  todos: todosReducer
});

const store = createStore(rootReducer);

// Component using Redux
const TodoList = () => {
  const todos = useSelector(state => state.todos);
  const dispatch = useDispatch();

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <span
            style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}
            onClick={() => dispatch(toggleTodo(todo.id))}
          >
            {todo.text}
          </span>
          <button onClick={() => dispatch(deleteTodo(todo.id))}>
            Delete
          </button>
        </li>
      ))}
    </ul>
  );
};

// App setup
const App = () => {
  return (
    <Provider store={store}>
      <TodoList />
    </Provider>
  );
};
```

### Zustand Pattern (Lightweight State Management)
**Description:** Small, fast, and scalable state management solution.

```jsx
import create from 'zustand';

// Store definition
const useStore = create((set) => ({
  user: null,
  todos: [],
  setUser: (user) => set({ user }),
  addTodo: (text) => set((state) => ({
    todos: [...state.todos, { id: Date.now(), text, completed: false }]
  })),
  toggleTodo: (id) => set((state) => ({
    todos: state.todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    )
  })),
  deleteTodo: (id) => set((state) => ({
    todos: state.todos.filter(todo => todo.id !== id)
  }))
}));

// Usage in component
const TodoApp = () => {
  const { todos, addTodo, toggleTodo, deleteTodo } = useStore();

  return (
    <div>
      <TodoInput onAdd={addTodo} />
      <TodoList 
        todos={todos}
        onToggle={toggleTodo}
        onDelete={deleteTodo}
      />
    </div>
  );
};
```

---

## 4. Data Flow Patterns

### Unidirectional Data Flow
**Description:** Data flows in one direction: from parent to child via props, and events flow up via callbacks.

```jsx
// Parent component
const App = () => {
  const [users, setUsers] = useState([]);
  const [filter, setFilter] = useState('');

  const filteredUsers = users.filter(user =>
    user.name.toLowerCase().includes(filter.toLowerCase())
  );

  const handleUserAdd = (newUser) => {
    setUsers([...users, newUser]);
  };

  const handleUserDelete = (userId) => {
    setUsers(users.filter(user => user.id !== userId));
  };

  return (
    <div>
      <UserFilter 
        filter={filter}
        onFilterChange={setFilter}
      />
      <UserList 
        users={filteredUsers}
        onUserDelete={handleUserDelete}
      />
      <UserForm onUserAdd={handleUserAdd} />
    </div>
  );
};

// Child components
const UserFilter = ({ filter, onFilterChange }) => {
  return (
    <input
      value={filter}
      onChange={(e) => onFilterChange(e.target.value)}
      placeholder="Filter users..."
    />
  );
};

const UserList = ({ users, onUserDelete }) => {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>
          {user.name}
          <button onClick={() => onUserDelete(user.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
};
```

### Lifting State Up Pattern
**Description:** Move shared state to the closest common ancestor component.

```jsx
// Before: State in child component
const TemperatureInput = () => {
  const [temperature, setTemperature] = useState('');

  return (
    <input
      value={temperature}
      onChange={(e) => setTemperature(e.target.value)}
    />
  );
};

// After: State lifted to parent
const Calculator = () => {
  const [celsius, setCelsius] = useState('');
  const [fahrenheit, setFahrenheit] = useState('');

  const handleCelsiusChange = (value) => {
    setCelsius(value);
    setFahrenheit(value ? (value * 9/5 + 32).toFixed(2) : '');
  };

  const handleFahrenheitChange = (value) => {
    setFahrenheit(value);
    setCelsius(value ? ((value - 32) * 5/9).toFixed(2) : '');
  };

  return (
    <div>
      <TemperatureInput
        scale="celsius"
        temperature={celsius}
        onTemperatureChange={handleCelsiusChange}
      />
      <TemperatureInput
        scale="fahrenheit"
        temperature={fahrenheit}
        onTemperatureChange={handleFahrenheitChange}
      />
    </div>
  );
};

const TemperatureInput = ({ scale, temperature, onTemperatureChange }) => {
  return (
    <fieldset>
      <legend>Enter temperature in {scale}:</legend>
      <input
        value={temperature}
        onChange={(e) => onTemperatureChange(e.target.value)}
      />
    </fieldset>
  );
};
```

---

## 5. Routing Patterns

### React Router Pattern
**Description:** Declarative routing for React applications.

```jsx
import { BrowserRouter, Routes, Route, Link, useParams, useNavigate } from 'react-router-dom';

// Route configuration
const App = () => {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/users">Users</Link>
        <Link to="/about">About</Link>
      </nav>
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/users" element={<UserList />} />
        <Route path="/users/:id" element={<UserDetail />} />
        <Route path="/users/:id/edit" element={<UserEdit />} />
        <Route path="/about" element={<About />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
};

// Protected route pattern
const ProtectedRoute = ({ children, isAuthenticated }) => {
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  return children;
};

// Usage
<Route
  path="/dashboard"
  element={
    <ProtectedRoute isAuthenticated={isAuthenticated}>
      <Dashboard />
    </ProtectedRoute>
  }
/>

// Component using route params
const UserDetail = () => {
  const { id } = useParams();
  const navigate = useNavigate();
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetchUser(id).then(setUser);
  }, [id]);

  const handleEdit = () => {
    navigate(`/users/${id}/edit`);
  };

  return (
    <div>
      <h1>{user?.name}</h1>
      <button onClick={handleEdit}>Edit</button>
    </div>
  );
};
```

### Dynamic Route Loading Pattern
**Description:** Load routes dynamically to improve initial load time.

```jsx
import { lazy, Suspense } from 'react';
import { Routes, Route } from 'react-router-dom';

// Lazy load components
const Home = lazy(() => import('./pages/Home'));
const UserList = lazy(() => import('./pages/UserList'));
const UserDetail = lazy(() => import('./pages/UserDetail'));

const App = () => {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/users" element={<UserList />} />
        <Route path="/users/:id" element={<UserDetail />} />
      </Routes>
    </Suspense>
  );
};
```

---

## 6. Form Patterns

### Controlled Components Pattern
**Description:** Form data is handled by React component state.

```jsx
import { useState } from 'react';

const UserForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    password: ''
  });
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
    // Clear error when user types
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }));
    }
  };

  const validate = () => {
    const newErrors = {};
    if (!formData.name) newErrors.name = 'Name is required';
    if (!formData.email) newErrors.email = 'Email is required';
    if (!formData.password || formData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }
    return newErrors;
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    const validationErrors = validate();
    if (Object.keys(validationErrors).length > 0) {
      setErrors(validationErrors);
      return;
    }

    setIsSubmitting(true);
    try {
      await submitForm(formData);
      // Handle success
    } catch (error) {
      setErrors({ submit: error.message });
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          name="name"
          value={formData.name}
          onChange={handleChange}
          placeholder="Name"
        />
        {errors.name && <span className="error">{errors.name}</span>}
      </div>
      
      <div>
        <input
          name="email"
          type="email"
          value={formData.email}
          onChange={handleChange}
          placeholder="Email"
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      
      <div>
        <input
          name="password"
          type="password"
          value={formData.password}
          onChange={handleChange}
          placeholder="Password"
        />
        {errors.password && <span className="error">{errors.password}</span>}
      </div>
      
      {errors.submit && <div className="error">{errors.submit}</div>}
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Submitting...' : 'Submit'}
      </button>
    </form>
  );
};
```

### React Hook Form Pattern
**Description:** Performant, flexible form library with easy-to-use validation.

```jsx
import { useForm } from 'react-hook-form';

const UserForm = () => {
  const { register, handleSubmit, formState: { errors, isSubmitting } } = useForm();

  const onSubmit = async (data) => {
    try {
      await fetch('/api/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(data)
      });
    } catch (error) {
      console.error('Submission error:', error);
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register('name', { required: 'Name is required' })}
        placeholder="Name"
      />
      {errors.name && <span>{errors.name.message}</span>}
      
      <input
        {...register('email', {
          required: 'Email is required',
          pattern: {
            value: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i,
            message: 'Invalid email address'
          }
        })}
        placeholder="Email"
      />
      {errors.email && <span>{errors.email.message}</span>}
      
      <input
        type="password"
        {...register('password', {
          required: 'Password is required',
          minLength: {
            value: 6,
            message: 'Password must be at least 6 characters'
          }
        })}
        placeholder="Password"
      />
      {errors.password && <span>{errors.password.message}</span>}
      
      <button type="submit" disabled={isSubmitting}>
        Submit
      </button>
    </form>
  );
};
```

---

## 7. HTTP & API Patterns

### Custom Hook for API Calls
**Description:** Encapsulate API logic in reusable hooks.

```jsx
import { useState, useEffect, useCallback } from 'react';

// Generic API hook
const useApi = (url, options = {}) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await fetch(url, options);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  }, [url, JSON.stringify(options)]);

  useEffect(() => {
    if (options.autoFetch !== false) {
      fetchData();
    }
  }, [fetchData, options.autoFetch]);

  const refetch = useCallback(() => {
    fetchData();
  }, [fetchData]);

  return { data, loading, error, refetch };
};

// Mutation hook
const useMutation = (mutationFn) => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const mutate = useCallback(async (variables) => {
    setLoading(true);
    setError(null);
    try {
      const result = await mutationFn(variables);
      return result;
    } catch (err) {
      setError(err.message);
      throw err;
    } finally {
      setLoading(false);
    }
  }, [mutationFn]);

  return { mutate, loading, error };
};

// Usage
const UserList = () => {
  const { data: users, loading, error, refetch } = useApi('/api/users');
  const { mutate: createUser, loading: creating } = useMutation(
    async (userData) => {
      const response = await fetch('/api/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(userData)
      });
      return response.json();
    }
  );

  const handleCreateUser = async (userData) => {
    await createUser(userData);
    refetch(); // Refresh list after creation
  };

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      <UserForm onSubmit={handleCreateUser} isSubmitting={creating} />
      <ul>
        {users.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
};
```

### API Service Pattern
**Description:** Centralized API service layer for organizing API calls.

```jsx
// API service
class ApiService {
  constructor(baseURL) {
    this.baseURL = baseURL;
  }

  async request(endpoint, options = {}) {
    const url = `${this.baseURL}${endpoint}`;
    const config = {
      headers: {
        'Content-Type': 'application/json',
        ...options.headers
      },
      ...options
    };

    try {
      const response = await fetch(url, config);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      return await response.json();
    } catch (error) {
      throw error;
    }
  }

  get(endpoint) {
    return this.request(endpoint, { method: 'GET' });
  }

  post(endpoint, data) {
    return this.request(endpoint, {
      method: 'POST',
      body: JSON.stringify(data)
    });
  }

  put(endpoint, data) {
    return this.request(endpoint, {
      method: 'PUT',
      body: JSON.stringify(data)
    });
  }

  delete(endpoint) {
    return this.request(endpoint, { method: 'DELETE' });
  }
}

// User API service
class UserService extends ApiService {
  constructor() {
    super('/api');
  }

  getUsers() {
    return this.get('/users');
  }

  getUser(id) {
    return this.get(`/users/${id}`);
  }

  createUser(userData) {
    return this.post('/users', userData);
  }

  updateUser(id, userData) {
    return this.put(`/users/${id}`, userData);
  }

  deleteUser(id) {
    return this.delete(`/users/${id}`);
  }
}

export const userService = new UserService();
```

---

## 8. Testing Patterns

### Component Testing Pattern
**Description:** Testing React components with React Testing Library.

```jsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import '@testing-library/jest-dom';
import UserForm from './UserForm';

describe('UserForm', () => {
  it('renders form fields', () => {
    render(<UserForm />);
    expect(screen.getByPlaceholderText('Name')).toBeInTheDocument();
    expect(screen.getByPlaceholderText('Email')).toBeInTheDocument();
  });

  it('validates required fields', async () => {
    render(<UserForm />);
    fireEvent.click(screen.getByText('Submit'));
    
    await waitFor(() => {
      expect(screen.getByText('Name is required')).toBeInTheDocument();
    });
  });

  it('submits form with valid data', async () => {
    const onSubmit = jest.fn();
    render(<UserForm onSubmit={onSubmit} />);
    
    fireEvent.change(screen.getByPlaceholderText('Name'), {
      target: { value: 'John Doe' }
    });
    fireEvent.change(screen.getByPlaceholderText('Email'), {
      target: { value: 'john@example.com' }
    });
    fireEvent.click(screen.getByText('Submit'));
    
    await waitFor(() => {
      expect(onSubmit).toHaveBeenCalledWith({
        name: 'John Doe',
        email: 'john@example.com'
      });
    });
  });
});
```

### Hook Testing Pattern
**Description:** Testing custom hooks with React Testing Library.

```jsx
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

describe('useCounter', () => {
  it('initializes with default value', () => {
    const { result } = renderHook(() => useCounter());
    expect(result.current.count).toBe(0);
  });

  it('increments count', () => {
    const { result } = renderHook(() => useCounter());
    act(() => {
      result.current.increment();
    });
    expect(result.current.count).toBe(1);
  });

  it('decrements count', () => {
    const { result } = renderHook(() => useCounter(5));
    act(() => {
      result.current.decrement();
    });
    expect(result.current.count).toBe(4);
  });
});
```

---

## 9. Performance Patterns

### React.memo Pattern
**Description:** Prevents unnecessary re-renders by memoizing component.

```jsx
import { memo } from 'react';

// Memoized component
const UserCard = memo(({ user, onDelete }) => {
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button onClick={() => onDelete(user.id)}>Delete</button>
    </div>
  );
}, (prevProps, nextProps) => {
  // Custom comparison function
  return prevProps.user.id === nextProps.user.id &&
         prevProps.user.name === nextProps.user.name &&
         prevProps.user.email === nextProps.user.email;
});

// Usage
const UserList = ({ users, onDelete }) => {
  return (
    <div>
      {users.map(user => (
        <UserCard key={user.id} user={user} onDelete={onDelete} />
      ))}
    </div>
  );
};
```

### useMemo Pattern
**Description:** Memoizes expensive computations.

```jsx
import { useMemo } from 'react';

const ExpensiveComponent = ({ items, filter }) => {
  const filteredItems = useMemo(() => {
    return items.filter(item => 
      item.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [items, filter]);

  const sortedItems = useMemo(() => {
    return [...filteredItems].sort((a, b) => a.name.localeCompare(b.name));
  }, [filteredItems]);

  return (
    <ul>
      {sortedItems.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
};
```

### useCallback Pattern
**Description:** Memoizes callback functions to prevent unnecessary re-renders.

```jsx
import { useState, useCallback, memo } from 'react';

const UserList = ({ users }) => {
  const [filter, setFilter] = useState('');

  // Memoized callback
  const handleDelete = useCallback((userId) => {
    // Delete logic
    console.log('Delete user:', userId);
  }, []); // Empty deps means function never changes

  const filteredUsers = users.filter(user =>
    user.name.toLowerCase().includes(filter.toLowerCase())
  );

  return (
    <div>
      <input
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
      />
      {filteredUsers.map(user => (
        <UserCard key={user.id} user={user} onDelete={handleDelete} />
      ))}
    </div>
  );
};

// UserCard won't re-render unless user data changes
const UserCard = memo(({ user, onDelete }) => {
  return (
    <div>
      <h3>{user.name}</h3>
      <button onClick={() => onDelete(user.id)}>Delete</button>
    </div>
  );
});
```

### Code Splitting Pattern
**Description:** Split code into smaller chunks loaded on demand.

```jsx
import { lazy, Suspense } from 'react';

// Lazy load components
const HeavyComponent = lazy(() => import('./HeavyComponent'));
const AnotherComponent = lazy(() => import('./AnotherComponent'));

const App = () => {
  const [showHeavy, setShowHeavy] = useState(false);

  return (
    <div>
      <button onClick={() => setShowHeavy(true)}>Load Heavy Component</button>
      {showHeavy && (
        <Suspense fallback={<div>Loading...</div>}>
          <HeavyComponent />
        </Suspense>
      )}
    </div>
  );
};
```

---

## 10. Security Patterns

### XSS Prevention Pattern
**Description:** Prevent cross-site scripting attacks.

```jsx
import { useMemo } from 'react';
import DOMPurify from 'dompurify';

const SafeHTML = ({ html }) => {
  const sanitizedHTML = useMemo(() => {
    return DOMPurify.sanitize(html);
  }, [html]);

  return <div dangerouslySetInnerHTML={{ __html: sanitizedHTML }} />;
};

// Usage
const UserContent = ({ userContent }) => {
  return <SafeHTML html={userContent} />;
};
```

### Authentication Pattern
**Description:** Secure authentication flow.

```jsx
import { createContext, useContext, useState, useEffect } from 'react';

const AuthContext = createContext();

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Check for stored token
    const token = localStorage.getItem('token');
    if (token) {
      validateToken(token);
    } else {
      setLoading(false);
    }
  }, []);

  const validateToken = async (token) => {
    try {
      const response = await fetch('/api/auth/validate', {
        headers: { Authorization: `Bearer ${token}` }
      });
      if (response.ok) {
        const userData = await response.json();
        setUser(userData);
      } else {
        localStorage.removeItem('token');
      }
    } catch (error) {
      localStorage.removeItem('token');
    } finally {
      setLoading(false);
    }
  };

  const login = async (email, password) => {
    const response = await fetch('/api/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password })
    });
    
    if (!response.ok) {
      throw new Error('Login failed');
    }

    const { token, user: userData } = await response.json();
    localStorage.setItem('token', token);
    setUser(userData);
  };

  const logout = () => {
    localStorage.removeItem('token');
    setUser(null);
  };

  return (
    <AuthContext.Provider value={{ user, loading, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => useContext(AuthContext);
```

---

## 11. Error Handling Patterns

### Error Boundary Pattern
**Description:** Catch JavaScript errors anywhere in the component tree.

```jsx
import { Component } from 'react';

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    // Log error to error reporting service
    console.error('Error caught by boundary:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-boundary">
          <h2>Something went wrong</h2>
          <p>{this.state.error?.message}</p>
          <button onClick={() => this.setState({ hasError: false, error: null })}>
            Try again
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

// Usage
const App = () => {
  return (
    <ErrorBoundary>
      <MyApp />
    </ErrorBoundary>
  );
};
```

### Async Error Handling Pattern
**Description:** Handle errors in async operations.

```jsx
import { useState, useEffect } from 'react';

const useAsyncOperation = (asyncFunction) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const execute = async (...args) => {
    setLoading(true);
    setError(null);
    try {
      const result = await asyncFunction(...args);
      setData(result);
      return result;
    } catch (err) {
      setError(err);
      throw err;
    } finally {
      setLoading(false);
    }
  };

  return { data, loading, error, execute };
};

// Usage
const UserList = () => {
  const { data: users, loading, error, execute } = useAsyncOperation(
    () => fetch('/api/users').then(res => res.json())
  );

  useEffect(() => {
    execute();
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};
```

---

## 12. Internationalization Patterns

### i18n Pattern
**Description:** Internationalization and localization.

```jsx
import { createContext, useContext } from 'react';

const I18nContext = createContext();

export const I18nProvider = ({ children, locale, translations }) => {
  const t = (key, params = {}) => {
    let translation = translations[locale][key] || key;
    // Replace parameters
    Object.keys(params).forEach(param => {
      translation = translation.replace(`{${param}}`, params[param]);
    });
    return translation;
  };

  return (
    <I18nContext.Provider value={{ t, locale }}>
      {children}
    </I18nContext.Provider>
  );
};

export const useTranslation = () => {
  const context = useContext(I18nContext);
  if (!context) {
    throw new Error('useTranslation must be used within I18nProvider');
  }
  return context;
};

// Usage
const WelcomeMessage = () => {
  const { t } = useTranslation();
  return <h1>{t('welcome', { name: 'John' })}</h1>;
};
```

---

## 13. Code Splitting & Lazy Loading

### Route-based Code Splitting
**Description:** Split code based on routes.

```jsx
import { lazy, Suspense } from 'react';
import { Routes, Route } from 'react-router-dom';

const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Contact = lazy(() => import('./pages/Contact'));

const App = () => {
  return (
    <Suspense fallback={<LoadingSpinner />}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </Suspense>
  );
};
```

### Component-based Code Splitting
**Description:** Load components on demand.

```jsx
import { lazy, Suspense, useState } from 'react';

const HeavyChart = lazy(() => import('./HeavyChart'));

const Dashboard = () => {
  const [showChart, setShowChart] = useState(false);

  return (
    <div>
      <button onClick={() => setShowChart(true)}>Show Chart</button>
      {showChart && (
        <Suspense fallback={<div>Loading chart...</div>}>
          <HeavyChart />
        </Suspense>
      )}
    </div>
  );
};
```

---

## 14. Server-Side Rendering Patterns

### Next.js SSR Pattern
**Description:** Server-side rendering with Next.js.

```jsx
// pages/users.js
export async function getServerSideProps(context) {
  const res = await fetch(`https://api.example.com/users`);
  const users = await res.json();

  return {
    props: {
      users
    }
  };
}

const UsersPage = ({ users }) => {
  return (
    <div>
      <h1>Users</h1>
      <ul>
        {users.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
};

export default UsersPage;
```

---

## Pattern Selection Matrix

| **Scenario** | **Primary Pattern** | **Secondary Pattern** | **When to Use** |
|---------------|-------------------|---------------------|-----------------|
| **Component Reuse** | Custom Hooks | HOC | Reusable logic |
| **State Management** | Context API | Redux/Zustand | Global vs Local state |
| **Form Handling** | React Hook Form | Controlled Components | Complex vs Simple forms |
| **Data Fetching** | Custom Hooks | React Query | API calls |
| **Performance** | React.memo | useMemo/useCallback | Optimization |
| **Routing** | React Router | Dynamic Routes | Navigation |
| **Code Splitting** | Lazy Loading | Route-based splitting | Bundle size |

---

## Best Practices Checklist

### Component Design
- [ ] Use functional components with hooks
- [ ] Separate container and presentational components
- [ ] Keep components small and focused
- [ ] Use proper prop types or TypeScript
- [ ] Extract reusable logic into custom hooks

### State Management
- [ ] Start with local state (useState)
- [ ] Lift state up when needed
- [ ] Use Context API for global state
- [ ] Consider Redux/Zustand for complex state
- [ ] Avoid prop drilling

### Performance
- [ ] Use React.memo for expensive components
- [ ] Use useMemo for expensive computations
- [ ] Use useCallback for callback functions
- [ ] Implement code splitting
- [ ] Optimize re-renders

### Code Quality
- [ ] Write tests for components
- [ ] Use Error Boundaries
- [ ] Handle loading and error states
- [ ] Use TypeScript for type safety
- [ ] Follow consistent naming conventions

---

## Conclusion

This comprehensive guide provides React-specific patterns for building scalable applications. Remember:

- **Start simple** - Don't over-engineer with patterns you don't need
- **Use hooks** - Prefer hooks over class components
- **Compose components** - Build complex UIs from simple components
- **Manage state appropriately** - Choose the right state management solution
- **Optimize performance** - Use memoization and code splitting
- **Test your code** - Write tests for critical components
- **Follow React best practices** - Keep components pure and predictable

The biggest mistake React developers make is not understanding when to use each pattern. Don't just apply patterns blindly - understand the problem first, then choose the appropriate solution. React provides excellent patterns out of the box - use them!

Good luck with your React development! 🚀

