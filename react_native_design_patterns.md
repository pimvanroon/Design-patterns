# Complete React Native Design Patterns Guide

A comprehensive guide combining React Native-specific patterns, architectural decisions, and practical JavaScript/TypeScript code examples. This guide covers everything from basic React Native patterns to advanced mobile application architectures.

## Table of Contents

### React Native-Specific Patterns
- [1. Component Patterns](#1-component-patterns)
- [2. Navigation Patterns](#2-navigation-patterns)
- [3. State Management Patterns](#3-state-management-patterns)
- [4. Native Module Patterns](#4-native-module-patterns)
- [5. Performance Patterns](#5-performance-patterns)
- [6. Styling Patterns](#6-styling-patterns)
- [7. Testing Patterns](#7-testing-patterns)
- [8. Platform-Specific Patterns](#8-platform-specific-patterns)

### Advanced Patterns
- [9. Offline & Sync Patterns](#9-offline--sync-patterns)
- [10. Push Notification Patterns](#10-push-notification-patterns)
- [11. Security Patterns](#11-security-patterns)
- [12. Error Handling Patterns](#12-error-handling-patterns)
- [13. Code Splitting & Bundle Optimization](#13-code-splitting--bundle-optimization)

### Decision Tools
- [React Native Pattern Decision Tree](#react-native-pattern-decision-tree)
- [Pattern Selection Matrix](#pattern-selection-matrix)
- [Best Practices Checklist](#best-practices-checklist)

---

## React Native Pattern Decision Tree

### When building React Native applications...

```
┌─────────────────────────────────────────────────────────────┐
│                REACT NATIVE CHALLENGE                        │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   COMPONENT   │ │ NAVIGATION│ │ STATE       │
        │   STRUCTURE   │ │           │ │ MANAGEMENT  │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ CONTAINER/    │ │ STACK     │ │ CONTEXT API │
        │ PRESENTATIONAL │ │ NAVIGATOR │ │ REDUX       │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ NATIVE        │ │ ASYNC     │ │ ZUSTAND     │
        │ COMPONENTS    │ │ STORAGE   │ │ MOBILET     │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## 1. Component Patterns

### Container/Presentational Components
**Description:** Separates components into container components (with logic) and presentational components (pure UI). Essential for maintainable React Native applications.

```jsx
import React, { useState, useEffect } from 'react';
import { View, FlatList, ActivityIndicator } from 'react-native';
import UserCard from './UserCard';
import { fetchUsers } from './api/userService';

// Container Component
const UserListContainer = () => {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    loadUsers();
  }, []);

  const loadUsers = async () => {
    try {
      setLoading(true);
      const data = await fetchUsers();
      setUsers(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  const handleUserPress = (user) => {
    // Navigate to user detail
    navigation.navigate('UserDetail', { userId: user.id });
  };

  return (
    <UserList 
      users={users}
      loading={loading}
      error={error}
      onUserPress={handleUserPress}
      onRefresh={loadUsers}
    />
  );
};

// Presentational Component
const UserList = ({ users, loading, error, onUserPress, onRefresh }) => {
  if (loading) {
    return (
      <View style={styles.center}>
        <ActivityIndicator size="large" />
      </View>
    );
  }

  if (error) {
    return (
      <View style={styles.center}>
        <Text style={styles.error}>{error}</Text>
      </View>
    );
  }

  return (
    <FlatList
      data={users}
      renderItem={({ item }) => (
        <UserCard user={item} onPress={() => onUserPress(item)} />
      )}
      keyExtractor={(item) => item.id.toString()}
      refreshing={loading}
      onRefresh={onRefresh}
    />
  );
};
```

### Platform-Specific Components
**Description:** Components that adapt to iOS and Android platforms.

```jsx
import { Platform } from 'react-native';

const CustomButton = ({ title, onPress }) => {
  if (Platform.OS === 'ios') {
    return (
      <TouchableOpacity onPress={onPress} style={styles.iosButton}>
        <Text style={styles.iosButtonText}>{title}</Text>
      </TouchableOpacity>
    );
  }

  return (
    <TouchableNativeFeedback onPress={onPress}>
      <View style={styles.androidButton}>
        <Text style={styles.androidButtonText}>{title}</Text>
      </View>
    </TouchableNativeFeedback>
  );
};

// Or using Platform.select
const styles = StyleSheet.create({
  button: Platform.select({
    ios: {
      backgroundColor: '#007AFF',
      borderRadius: 8,
    },
    android: {
      backgroundColor: '#6200EE',
      borderRadius: 4,
    },
  }),
});
```

### Custom Hooks Pattern
**Description:** Extract reusable logic into custom hooks.

```jsx
import { useState, useEffect, useCallback } from 'react';
import { Alert } from 'react-native';
import AsyncStorage from '@react-native-async-storage/async-storage';

// Custom hook for API calls
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

  return { data, loading, error, refetch: fetchData };
};

// Custom hook for async storage
const useAsyncStorage = (key, initialValue) => {
  const [storedValue, setStoredValue] = useState(initialValue);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    loadStoredValue();
  }, []);

  const loadStoredValue = async () => {
    try {
      const item = await AsyncStorage.getItem(key);
      if (item !== null) {
        setStoredValue(JSON.parse(item));
      }
    } catch (error) {
      console.error('Error loading from AsyncStorage:', error);
    } finally {
      setLoading(false);
    }
  };

  const setValue = async (value) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      await AsyncStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error('Error saving to AsyncStorage:', error);
    }
  };

  return [storedValue, setValue, loading];
};

// Usage
const UserProfile = () => {
  const { data: user, loading } = useApi('/api/user/profile');
  const [theme, setTheme] = useAsyncStorage('theme', 'light');

  if (loading) return <ActivityIndicator />;

  return (
    <View style={styles.container}>
      <Text>{user?.name}</Text>
      <Button title="Toggle Theme" onPress={() => setTheme(theme === 'light' ? 'dark' : 'light')} />
    </View>
  );
};
```

---

## 2. Navigation Patterns

### React Navigation Stack Pattern
**Description:** Stack-based navigation for mobile apps.

```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

const Stack = createStackNavigator();
const Tab = createBottomTabNavigator();

// Tab Navigator
const HomeTabs = () => {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Feed" component={FeedScreen} />
      <Tab.Screen name="Profile" component={ProfileScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  );
};

// Stack Navigator
const AppNavigator = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeTabs} />
        <Stack.Screen name="UserDetail" component={UserDetailScreen} />
        <Stack.Screen name="EditProfile" component={EditProfileScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
};

// Screen with navigation
const FeedScreen = ({ navigation }) => {
  const handleUserPress = (userId) => {
    navigation.navigate('UserDetail', { userId });
  };

  return (
    <View>
      <FlatList
        data={users}
        renderItem={({ item }) => (
          <TouchableOpacity onPress={() => handleUserPress(item.id)}>
            <Text>{item.name}</Text>
          </TouchableOpacity>
        )}
      />
    </View>
  );
};
```

### Deep Linking Pattern
**Description:** Handle deep links and universal links in React Native.

```jsx
import { useEffect } from 'react';
import { Linking } from 'react-native';
import { useNavigation } from '@react-navigation/native';

const useDeepLinking = () => {
  const navigation = useNavigation();

  useEffect(() => {
    // Handle initial URL if app opened from link
    Linking.getInitialURL().then((url) => {
      if (url) {
        handleDeepLink(url);
      }
    });

    // Handle URL when app is already running
    const subscription = Linking.addEventListener('url', ({ url }) => {
      handleDeepLink(url);
    });

    return () => {
      subscription.remove();
    };
  }, []);

  const handleDeepLink = (url) => {
    const route = url.replace(/.*?:\/\//g, '');
    const [routeName, paramString] = route.split('/');
    const params = paramString ? { id: paramString } : {};

    if (routeName === 'user') {
      navigation.navigate('UserDetail', params);
    } else if (routeName === 'post') {
      navigation.navigate('PostDetail', params);
    }
  };
};
```

---

## 3. State Management Patterns

### Context API Pattern
**Description:** Built-in state management for React Native.

```jsx
import React, { createContext, useContext, useState, useCallback } from 'react';

const AuthContext = createContext();

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
      await AsyncStorage.setItem('token', userData.token);
    } catch (error) {
      throw error;
    } finally {
      setLoading(false);
    }
  }, []);

  const logout = useCallback(async () => {
    setUser(null);
    await AsyncStorage.removeItem('token');
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

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};
```

### Redux Pattern
**Description:** Predictable state container for React Native apps.

```jsx
import { createStore, combineReducers } from 'redux';
import { Provider, useSelector, useDispatch } from 'react-redux';

// Actions
const ADD_TODO = 'ADD_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';

export const addTodo = (text) => ({
  type: ADD_TODO,
  payload: { id: Date.now(), text, completed: false }
});

export const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
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
    default:
      return state;
  }
};

const rootReducer = combineReducers({
  todos: todosReducer
});

const store = createStore(rootReducer);

// Component
const TodoList = () => {
  const todos = useSelector(state => state.todos);
  const dispatch = useDispatch();

  return (
    <FlatList
      data={todos}
      renderItem={({ item }) => (
        <TouchableOpacity onPress={() => dispatch(toggleTodo(item.id))}>
          <Text style={item.completed && styles.completed}>
            {item.text}
          </Text>
        </TouchableOpacity>
      )}
    />
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
**Description:** Small, fast state management solution for React Native.

```jsx
import create from 'zustand';

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
  }))
}));

// Usage
const TodoApp = () => {
  const { todos, addTodo, toggleTodo } = useStore();

  return (
    <View>
      <TodoInput onAdd={addTodo} />
      <FlatList
        data={todos}
        renderItem={({ item }) => (
          <TodoItem todo={item} onToggle={() => toggleTodo(item.id)} />
        )}
      />
    </View>
  );
};
```

---

## 4. Native Module Patterns

### Native Module Bridge Pattern
**Description:** Bridge between JavaScript and native code.

```jsx
// JavaScript side
import { NativeModules } from 'react-native';

const { CalendarModule } = NativeModules;

const createCalendarEvent = async (name, location) => {
  try {
    const result = await CalendarModule.createCalendarEvent(name, location);
    console.log('Event created:', result);
  } catch (error) {
    console.error('Error creating event:', error);
  }
};

// Usage
<Button
  title="Create Calendar Event"
  onPress={() => createCalendarEvent('Meeting', 'Office')}
/>
```

### Event Emitter Pattern
**Description:** Communicate from native to JavaScript.

```jsx
import { NativeEventEmitter, NativeModules } from 'react-native';
import { useEffect, useState } from 'react';

const { CalendarModule } = NativeModules;
const calendarEmitter = new NativeEventEmitter(CalendarModule);

const EventScreen = () => {
  const [eventCount, setEventCount] = useState(0);

  useEffect(() => {
    const subscription = calendarEmitter.addListener(
      'EventReminder',
      (event) => {
        setEventCount(event.count);
      }
    );

    return () => {
      subscription.remove();
    };
  }, []);

  return <Text>Event Count: {eventCount}</Text>;
};
```

---

## 5. Performance Patterns

### FlatList Optimization Pattern
**Description:** Optimize list rendering for performance.

```jsx
import React, { useMemo, useCallback } from 'react';
import { FlatList } from 'react-native';

const OptimizedList = ({ items }) => {
  // Memoize list item component
  const renderItem = useCallback(({ item }) => {
    return <ListItem item={item} />;
  }, []);

  // Memoize key extractor
  const keyExtractor = useCallback((item) => item.id.toString(), []);

  // Memoize getItemLayout for better performance
  const getItemLayout = useCallback(
    (data, index) => ({
      length: ITEM_HEIGHT,
      offset: ITEM_HEIGHT * index,
      index,
    }),
    []
  );

  return (
    <FlatList
      data={items}
      renderItem={renderItem}
      keyExtractor={keyExtractor}
      getItemLayout={getItemLayout}
      removeClippedSubviews={true}
      maxToRenderPerBatch={10}
      windowSize={10}
      initialNumToRender={10}
    />
  );
};

// Memoized list item
const ListItem = React.memo(({ item }) => {
  return (
    <View style={styles.item}>
      <Text>{item.name}</Text>
    </View>
  );
});
```

### Image Optimization Pattern
**Description:** Optimize image loading and caching.

```jsx
import { Image } from 'react-native';
import FastImage from 'react-native-fast-image';

const OptimizedImage = ({ uri, style }) => {
  return (
    <FastImage
      style={style}
      source={{
        uri,
        priority: FastImage.priority.normal,
        cache: FastImage.cacheControl.immutable,
      }}
      resizeMode={FastImage.resizeMode.cover}
    />
  );
};

// Or using built-in Image with optimization
const OptimizedImageBuiltIn = ({ uri, style }) => {
  return (
    <Image
      source={{ uri }}
      style={style}
      resizeMode="cover"
      // Enable image caching
      cache="force-cache"
    />
  );
};
```

### Memoization Pattern
**Description:** Prevent unnecessary re-renders.

```jsx
import React, { memo, useMemo, useCallback } from 'react';
import { View, Text, TouchableOpacity } from 'react-native';

// Memoized component
const UserCard = memo(({ user, onPress }) => {
  return (
    <TouchableOpacity onPress={onPress} style={styles.card}>
      <Text>{user.name}</Text>
      <Text>{user.email}</Text>
    </TouchableOpacity>
  );
}, (prevProps, nextProps) => {
  // Custom comparison
  return prevProps.user.id === nextProps.user.id &&
         prevProps.user.name === nextProps.user.name;
});

// Component with memoized callbacks
const UserList = ({ users, onUserPress }) => {
  const filteredUsers = useMemo(() => {
    return users.filter(user => user.active);
  }, [users]);

  const handlePress = useCallback((user) => {
    onUserPress(user);
  }, [onUserPress]);

  return (
    <FlatList
      data={filteredUsers}
      renderItem={({ item }) => (
        <UserCard user={item} onPress={() => handlePress(item)} />
      )}
      keyExtractor={(item) => item.id.toString()}
    />
  );
};
```

---

## 6. Styling Patterns

### StyleSheet Pattern
**Description:** Use StyleSheet for optimized styling.

```jsx
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    padding: 16,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 16,
  },
  button: {
    backgroundColor: '#007AFF',
    padding: 12,
    borderRadius: 8,
  },
  buttonText: {
    color: '#fff',
    fontSize: 16,
    textAlign: 'center',
  },
});

// Usage
<View style={styles.container}>
  <Text style={styles.title}>Title</Text>
  <TouchableOpacity style={styles.button}>
    <Text style={styles.buttonText}>Button</Text>
  </TouchableOpacity>
</View>
```

### Theme Pattern
**Description:** Centralized theming system.

```jsx
// theme.js
export const theme = {
  colors: {
    primary: '#007AFF',
    secondary: '#5856D6',
    background: '#FFFFFF',
    text: '#000000',
    error: '#FF3B30',
  },
  spacing: {
    xs: 4,
    sm: 8,
    md: 16,
    lg: 24,
    xl: 32,
  },
  typography: {
    h1: { fontSize: 32, fontWeight: 'bold' },
    h2: { fontSize: 24, fontWeight: 'bold' },
    body: { fontSize: 16 },
  },
};

// Theme context
import { createContext, useContext } from 'react';

const ThemeContext = createContext(theme);

export const ThemeProvider = ({ children, theme: customTheme }) => {
  return (
    <ThemeContext.Provider value={customTheme || theme}>
      {children}
    </ThemeContext.Provider>
  );
};

export const useTheme = () => useContext(ThemeContext);

// Usage
const ThemedButton = ({ title, onPress }) => {
  const theme = useTheme();

  return (
    <TouchableOpacity
      onPress={onPress}
      style={{
        backgroundColor: theme.colors.primary,
        padding: theme.spacing.md,
        borderRadius: 8,
      }}
    >
      <Text style={{ color: theme.colors.background }}>
        {title}
      </Text>
    </TouchableOpacity>
  );
};
```

---

## 7. Testing Patterns

### Component Testing Pattern
**Description:** Test React Native components.

```jsx
import React from 'react';
import { render, fireEvent } from '@testing-library/react-native';
import UserCard from './UserCard';

describe('UserCard', () => {
  it('renders user information', () => {
    const user = { id: 1, name: 'John Doe', email: 'john@example.com' };
    const { getByText } = render(<UserCard user={user} />);
    
    expect(getByText('John Doe')).toBeTruthy();
    expect(getByText('john@example.com')).toBeTruthy();
  });

  it('calls onPress when pressed', () => {
    const user = { id: 1, name: 'John Doe' };
    const onPress = jest.fn();
    const { getByText } = render(<UserCard user={user} onPress={onPress} />);
    
    fireEvent.press(getByText('John Doe'));
    expect(onPress).toHaveBeenCalledWith(user);
  });
});
```

---

## 8. Platform-Specific Patterns

### Platform Detection Pattern
**Description:** Handle platform-specific logic.

```jsx
import { Platform } from 'react-native';

const PlatformSpecificComponent = () => {
  const platformStyles = Platform.select({
    ios: {
      paddingTop: 20,
      backgroundColor: '#FFFFFF',
    },
    android: {
      paddingTop: 0,
      backgroundColor: '#F5F5F5',
    },
  });

  return (
    <View style={platformStyles}>
      <Text>{Platform.OS === 'ios' ? 'iOS' : 'Android'}</Text>
    </View>
  );
};
```

---

## 9. Offline & Sync Patterns

### Offline First Pattern
**Description:** Handle offline functionality and data synchronization.

```jsx
import NetInfo from '@react-native-community/netinfo';
import AsyncStorage from '@react-native-async-storage/async-storage';

const useOfflineSync = () => {
  const [isOnline, setIsOnline] = useState(true);
  const [pendingActions, setPendingActions] = useState([]);

  useEffect(() => {
    const unsubscribe = NetInfo.addEventListener(state => {
      setIsOnline(state.isConnected);
      if (state.isConnected) {
        syncPendingActions();
      }
    });

    return () => unsubscribe();
  }, []);

  const syncPendingActions = async () => {
    const stored = await AsyncStorage.getItem('pendingActions');
    if (stored) {
      const actions = JSON.parse(stored);
      for (const action of actions) {
        try {
          await executeAction(action);
        } catch (error) {
          console.error('Sync failed:', error);
        }
      }
      await AsyncStorage.removeItem('pendingActions');
      setPendingActions([]);
    }
  };

  const queueAction = async (action) => {
    if (isOnline) {
      await executeAction(action);
    } else {
      const stored = await AsyncStorage.getItem('pendingActions');
      const actions = stored ? JSON.parse(stored) : [];
      actions.push(action);
      await AsyncStorage.setItem('pendingActions', JSON.stringify(actions));
      setPendingActions(actions);
    }
  };

  return { isOnline, queueAction };
};
```

---

## 10. Push Notification Patterns

### Push Notification Handler Pattern
**Description:** Handle push notifications in React Native.

```jsx
import { useEffect, useRef } from 'react';
import messaging from '@react-native-firebase/messaging';
import { Platform } from 'react-native';

const usePushNotifications = () => {
  const notificationListener = useRef();
  const backgroundListener = useRef();

  useEffect(() => {
    // Request permission
    requestPermission();

    // Handle foreground notifications
    notificationListener.current = messaging().onMessage(async remoteMessage => {
      console.log('Foreground notification:', remoteMessage);
      // Show local notification
    });

    // Handle background notifications
    messaging().onNotificationOpenedApp(remoteMessage => {
      console.log('Background notification opened:', remoteMessage);
      // Navigate to screen
    });

    // Handle notification when app is opened from quit state
    messaging()
      .getInitialNotification()
      .then(remoteMessage => {
        if (remoteMessage) {
          console.log('Notification opened app:', remoteMessage);
        }
      });

    return () => {
      notificationListener.current?.();
      backgroundListener.current?.();
    };
  }, []);

  const requestPermission = async () => {
    if (Platform.OS === 'ios') {
      const authStatus = await messaging().requestPermission();
      const enabled =
        authStatus === messaging.AuthorizationStatus.AUTHORIZED ||
        authStatus === messaging.AuthorizationStatus.PROVISIONAL;
      
      if (enabled) {
        console.log('Authorization status:', authStatus);
      }
    }
  };

  return { requestPermission };
};
```

---

## Pattern Selection Matrix

| **Scenario** | **Primary Pattern** | **Secondary Pattern** | **When to Use** |
|---------------|-------------------|---------------------|-----------------|
| **Navigation** | React Navigation Stack | Tab Navigator | Standard mobile navigation |
| **State Management** | Context API | Redux/Zustand | Global vs Local state |
| **Offline Support** | AsyncStorage | Redux Persist | Data persistence |
| **Performance** | FlatList Optimization | Memoization | List rendering |
| **Native Features** | Native Modules | Third-party Libraries | Platform-specific needs |

---

## Best Practices Checklist

### Component Design
- [ ] Use functional components with hooks
- [ ] Separate container and presentational components
- [ ] Handle platform-specific code with Platform.select
- [ ] Use StyleSheet for optimized styling
- [ ] Implement proper error boundaries

### Performance
- [ ] Use FlatList for long lists
- [ ] Implement memoization (React.memo, useMemo, useCallback)
- [ ] Optimize images with caching
- [ ] Use getItemLayout for FlatList
- [ ] Remove clipped subviews

### State Management
- [ ] Start with local state (useState)
- [ ] Use Context API for global state
- [ ] Consider Redux/Zustand for complex state
- [ ] Persist important state with AsyncStorage

### Navigation
- [ ] Use React Navigation for routing
- [ ] Implement deep linking
- [ ] Handle navigation state properly
- [ ] Use proper screen options

---

## Conclusion

This comprehensive guide provides React Native-specific patterns for building scalable mobile applications. Remember:

- **Optimize for mobile** - Use FlatList, memoization, and proper image handling
- **Handle platforms** - Use Platform.select for iOS/Android differences
- **Manage state appropriately** - Choose the right state management solution
- **Handle offline** - Implement offline-first patterns
- **Test thoroughly** - Test on both iOS and Android devices
- **Follow React Native best practices** - Use native components when possible

Good luck with your React Native development! 🚀

