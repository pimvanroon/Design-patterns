# Complete Flutter Design Patterns Guide

A comprehensive guide combining Flutter-specific patterns, architectural decisions, and practical Dart code examples. This guide covers everything from basic Flutter patterns to advanced mobile application architectures.

## Table of Contents

### Flutter-Specific Patterns
- [1. Widget Patterns](#1-widget-patterns)
- [2. State Management Patterns](#2-state-management-patterns)
- [3. Navigation Patterns](#3-navigation-patterns)
- [4. Architecture Patterns](#4-architecture-patterns)
- [5. Performance Patterns](#5-performance-patterns)
- [6. Testing Patterns](#6-testing-patterns)
- [7. Platform-Specific Patterns](#7-platform-specific-patterns)

### Advanced Patterns
- [8. Dependency Injection Patterns](#8-dependency-injection-patterns)
- [9. Error Handling Patterns](#9-error-handling-patterns)
- [10. Offline & Sync Patterns](#10-offline--sync-patterns)
- [11. Security Patterns](#11-security-patterns)

### Decision Tools
- [Flutter Pattern Decision Tree](#flutter-pattern-decision-tree)
- [Pattern Selection Matrix](#pattern-selection-matrix)
- [Best Practices Checklist](#best-practices-checklist)

---

## Flutter Pattern Decision Tree

### When building Flutter applications...

```
┌─────────────────────────────────────────────────────────────┐
│                    FLUTTER CHALLENGE                         │
└─────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │   WIDGET      │ │ STATE      │ │ ARCHITECTURE│
        │   COMPOSITION │ │ MANAGEMENT │ │ PATTERNS    │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ STATELESS     │ │ PROVIDER   │ │ CLEAN       │
        │ STATEFUL      │ │ RIVERPOD   │ │ ARCHITECTURE │
        └───────────────┘ └───────────┘ └─────────────┘
                │               │               │
        ┌───────▼───────┐ ┌─────▼─────┐ ┌──────▼──────┐
        │ BLOC/REDUX    │ │ GETX       │ │ BLOC PATTERN │
        └───────────────┘ └───────────┘ └─────────────┘
```

---

## 1. Widget Patterns

### Stateless Widget Pattern
**Description:** Widgets that don't change over time. Use for pure UI components.

```dart
import 'package:flutter/material.dart';

class UserCard extends StatelessWidget {
  final String name;
  final String email;
  final VoidCallback onTap;

  const UserCard({
    Key? key,
    required this.name,
    required this.email,
    required this.onTap,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Card(
      child: ListTile(
        leading: CircleAvatar(
          child: Text(name[0]),
        ),
        title: Text(name),
        subtitle: Text(email),
        onTap: onTap,
      ),
    );
  }
}
```

### Stateful Widget Pattern
**Description:** Widgets that can change over time. Use when widget needs to maintain state.

```dart
import 'package:flutter/material.dart';

class CounterWidget extends StatefulWidget {
  const CounterWidget({Key? key}) : super(key: key);

  @override
  State<CounterWidget> createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Counter: $_counter'),
            ElevatedButton(
              onPressed: _incrementCounter,
              child: Text('Increment'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### Widget Composition Pattern
**Description:** Build complex UIs by composing smaller widgets.

```dart
import 'package:flutter/material.dart';

class UserProfile extends StatelessWidget {
  final String name;
  final String email;
  final String avatarUrl;

  const UserProfile({
    Key? key,
    required this.name,
    required this.email,
    required this.avatarUrl,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        AvatarWidget(imageUrl: avatarUrl),
        UserInfoWidget(name: name, email: email),
        ActionButtonsWidget(),
      ],
    );
  }
}

class AvatarWidget extends StatelessWidget {
  final String imageUrl;

  const AvatarWidget({Key? key, required this.imageUrl}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CircleAvatar(
      radius: 50,
      backgroundImage: NetworkImage(imageUrl),
    );
  }
}

class UserInfoWidget extends StatelessWidget {
  final String name;
  final String email;

  const UserInfoWidget({
    Key? key,
    required this.name,
    required this.email,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text(name, style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
        Text(email, style: TextStyle(fontSize: 16)),
      ],
    );
  }
}

class ActionButtonsWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
      children: [
        ElevatedButton(onPressed: () {}, child: Text('Edit')),
        ElevatedButton(onPressed: () {}, child: Text('Share')),
      ],
    );
  }
}
```

### Builder Pattern
**Description:** Use builder widgets for complex widget construction.

```dart
import 'package:flutter/material.dart';

class CustomListView extends StatelessWidget {
  final List<String> items;

  const CustomListView({Key? key, required this.items}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: items.length,
      itemBuilder: (context, index) {
        return ListTile(
          title: Text(items[index]),
        );
      },
    );
  }
}
```

---

## 2. State Management Patterns

### Provider Pattern
**Description:** Simple state management solution recommended by Flutter team.

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

// Model
class CounterModel extends ChangeNotifier {
  int _count = 0;

  int get count => _count;

  void increment() {
    _count++;
    notifyListeners();
  }

  void decrement() {
    _count--;
    notifyListeners();
  }
}

// Provider setup
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (_) => CounterModel(),
      child: MaterialApp(
        home: CounterScreen(),
      ),
    );
  }
}

// Widget using provider
class CounterScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Consumer<CounterModel>(
              builder: (context, counter, child) {
                return Text('Count: ${counter.count}');
              },
            ),
            ElevatedButton(
              onPressed: () {
                context.read<CounterModel>().increment();
              },
              child: Text('Increment'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### BLoC Pattern
**Description:** Business Logic Component pattern for state management.

```dart
import 'package:flutter_bloc/flutter_bloc.dart';
import 'package:equatable/equatable.dart';

// Events
abstract class CounterEvent extends Equatable {
  @override
  List<Object> get props => [];
}

class IncrementEvent extends CounterEvent {}
class DecrementEvent extends CounterEvent {}

// State
class CounterState extends Equatable {
  final int count;

  CounterState(this.count);

  @override
  List<Object> get props => [count];
}

// BLoC
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterState(0)) {
    on<IncrementEvent>((event, emit) {
      emit(CounterState(state.count + 1));
    });

    on<DecrementEvent>((event, emit) {
      emit(CounterState(state.count - 1));
    });
  }
}

// Usage
class CounterScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (_) => CounterBloc(),
      child: Scaffold(
        body: BlocBuilder<CounterBloc, CounterState>(
          builder: (context, state) {
            return Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text('Count: ${state.count}'),
                  ElevatedButton(
                    onPressed: () {
                      context.read<CounterBloc>().add(IncrementEvent());
                    },
                    child: Text('Increment'),
                  ),
                ],
              ),
            );
          },
        ),
      ),
    );
  }
}
```

### Riverpod Pattern
**Description:** Modern state management solution, improved version of Provider.

```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';

// Provider
final counterProvider = StateNotifierProvider<CounterNotifier, int>((ref) {
  return CounterNotifier();
});

// Notifier
class CounterNotifier extends StateNotifier<int> {
  CounterNotifier() : super(0);

  void increment() => state++;
  void decrement() => state--;
}

// Usage
class CounterScreen extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider);

    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Count: $count'),
            ElevatedButton(
              onPressed: () => ref.read(counterProvider.notifier).increment(),
              child: Text('Increment'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### GetX Pattern
**Description:** Lightweight state management solution.

```dart
import 'package:get/get.dart';

class CounterController extends GetxController {
  var count = 0.obs;

  void increment() => count++;
  void decrement() => count--;
}

// Usage
class CounterScreen extends StatelessWidget {
  final CounterController controller = Get.put(CounterController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Obx(() => Text('Count: ${controller.count}')),
            ElevatedButton(
              onPressed: () => controller.increment(),
              child: Text('Increment'),
            ),
          ],
        ),
      ),
    );
  }
}
```

---

## 3. Navigation Patterns

### Navigator Pattern
**Description:** Navigate between screens in Flutter.

```dart
import 'package:flutter/material.dart';

class NavigationExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      routes: {
        '/': (context) => HomeScreen(),
        '/details': (context) => DetailsScreen(),
      },
    );
  }
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, '/details');
          },
          child: Text('Go to Details'),
        ),
      ),
    );
  }
}

class DetailsScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Details')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: Text('Go Back'),
        ),
      ),
    );
  }
}
```

### Named Routes Pattern
**Description:** Use named routes for better navigation management.

```dart
import 'package:flutter/material.dart';

class AppRoutes {
  static const String home = '/';
  static const String details = '/details';
  static const String profile = '/profile';
}

class AppRouter {
  static Route<dynamic> generateRoute(RouteSettings settings) {
    switch (settings.name) {
      case AppRoutes.home:
        return MaterialPageRoute(builder: (_) => HomeScreen());
      case AppRoutes.details:
        final args = settings.arguments as Map<String, dynamic>;
        return MaterialPageRoute(
          builder: (_) => DetailsScreen(userId: args['userId']),
        );
      default:
        return MaterialPageRoute(
          builder: (_) => Scaffold(
            body: Center(child: Text('Route not found')),
          ),
        );
    }
  }
}

// Usage
MaterialApp(
  onGenerateRoute: AppRouter.generateRoute,
  initialRoute: AppRoutes.home,
)
```

---

## 4. Architecture Patterns

### Clean Architecture Pattern
**Description:** Separate concerns into layers (Domain, Data, Presentation).

```dart
// Domain Layer
class User {
  final int id;
  final String name;
  final String email;

  User({required this.id, required this.name, required this.email});
}

abstract class UserRepository {
  Future<List<User>> getUsers();
  Future<User> getUserById(int id);
}

// Data Layer
class UserRepositoryImpl implements UserRepository {
  final UserApiService apiService;

  UserRepositoryImpl(this.apiService);

  @override
  Future<List<User>> getUsers() async {
    final users = await apiService.fetchUsers();
    return users.map((dto) => User(
      id: dto.id,
      name: dto.name,
      email: dto.email,
    )).toList();
  }

  @override
  Future<User> getUserById(int id) async {
    final dto = await apiService.fetchUser(id);
    return User(id: dto.id, name: dto.name, email: dto.email);
  }
}

// Presentation Layer
class UserBloc extends Bloc<UserEvent, UserState> {
  final UserRepository repository;

  UserBloc(this.repository) : super(UserInitial()) {
    on<LoadUsers>(_onLoadUsers);
  }

  Future<void> _onLoadUsers(LoadUsers event, Emitter<UserState> emit) async {
    emit(UserLoading());
    try {
      final users = await repository.getUsers();
      emit(UserLoaded(users));
    } catch (e) {
      emit(UserError(e.toString()));
    }
  }
}
```

---

## 5. Performance Patterns

### ListView Optimization Pattern
**Description:** Optimize list rendering for performance.

```dart
import 'package:flutter/material.dart';

class OptimizedListView extends StatelessWidget {
  final List<String> items;

  const OptimizedListView({Key? key, required this.items}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: items.length,
      cacheExtent: 250.0, // Cache items outside viewport
      itemBuilder: (context, index) {
        return ListTile(
          key: ValueKey(items[index]), // Stable keys
          title: Text(items[index]),
        );
      },
    );
  }
}
```

### Const Constructor Pattern
**Description:** Use const constructors for immutable widgets.

```dart
class CustomWidget extends StatelessWidget {
  final String title;

  const CustomWidget({Key? key, required this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Text(title);
  }
}

// Usage - const widgets are more efficient
const CustomWidget(title: 'Hello')
```

---

## 6. Testing Patterns

### Widget Testing Pattern
**Description:** Test Flutter widgets.

```dart
import 'package:flutter_test/flutter_test.dart';
import 'package:flutter/material.dart';

void main() {
  testWidgets('Counter increments', (WidgetTester tester) async {
    await tester.pumpWidget(CounterWidget());

    expect(find.text('0'), findsOneWidget);
    expect(find.text('1'), findsNothing);

    await tester.tap(find.byIcon(Icons.add));
    await tester.pump();

    expect(find.text('0'), findsNothing);
    expect(find.text('1'), findsOneWidget);
  });
}
```

---

## Pattern Selection Matrix

| **Scenario** | **Primary Pattern** | **Secondary Pattern** | **When to Use** |
|---------------|-------------------|---------------------|-----------------|
| **Simple State** | StatefulWidget | Provider | Local widget state |
| **Global State** | Provider | Riverpod | App-wide state |
| **Complex State** | BLoC | Riverpod | Business logic separation |
| **Navigation** | Navigator | Named Routes | Screen navigation |
| **Architecture** | Clean Architecture | BLoC Pattern | Large applications |

---

## Best Practices Checklist

### Widget Design
- [ ] Use StatelessWidget when possible
- [ ] Compose widgets from smaller widgets
- [ ] Use const constructors for immutable widgets
- [ ] Extract reusable widgets
- [ ] Follow widget composition principles

### State Management
- [ ] Start with StatefulWidget for local state
- [ ] Use Provider for simple global state
- [ ] Use BLoC/Riverpod for complex state
- [ ] Avoid setState in complex widgets

### Performance
- [ ] Use ListView.builder for long lists
- [ ] Use const constructors
- [ ] Optimize rebuilds with keys
- [ ] Use RepaintBoundary for expensive widgets

---

## Conclusion

This comprehensive guide provides Flutter-specific patterns for building scalable mobile applications. Remember:

- **Compose widgets** - Build complex UIs from simple widgets
- **Manage state appropriately** - Choose the right state management solution
- **Optimize performance** - Use const constructors and ListView.builder
- **Follow Flutter best practices** - Use StatelessWidget when possible
- **Test thoroughly** - Write widget and integration tests

Good luck with your Flutter development! 🚀

