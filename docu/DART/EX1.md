# Dart Practice Exercises

## 1. Basic Variable Practice

### **Exercise 1: Current Date and Time**
Create a program that prints the current date and time using `DateTime.now()`. Use both `final` and `const` to observe the differences.

```dart
void main() {
  final currentTime = DateTime.now();
  print('Current Time: $currentTime');
  // Uncommenting the line below will cause an error
  // const compileTime = DateTime.now();
}
```

### **Exercise 2: Favorite Songs**
Create a list of your favorite songs and print:
- The length of the list
- The first song
- The last song

```dart
void main() {
  List<String> songs = ['Bohemian Rhapsody', 'Imagine', 'Hotel California', 'Yesterday'];
  print('Total Songs: ${songs.length}');
  print('First Song: ${songs.first}');
  print('Last Song: ${songs.last}');
}
```

### **Exercise 3: English to Korean Dictionary**
Create a `Map` to store English words as keys and their Korean translations as values. Print all the keys and values.

```dart
void main() {
  Map<String, String> dictionary = {
    'Hello': '안녕하세요',
    'Thank you': '감사합니다',
    'Goodbye': '안녕히 가세요'
  };

  print('All keys: ${dictionary.keys}');
  print('All values: ${dictionary.values}');
}
```

## 2. Control Flow Practice

### **Exercise 4: Odd or Even Checker**
Create a program that takes an integer as input and prints if it is odd or even.

```dart
void main() {
  int number = 7;
  if (number % 2 == 0) {
    print('$number is even');
  } else {
    print('$number is odd');
  }
}
```

### **Exercise 5: Sum of Numbers**
Use a `for` loop to print the sum of numbers from 1 to 100.

```dart
void main() {
  int sum = 0;
  for (int i = 1; i <= 100; i++) {
    sum += i;
  }
  print('Sum of numbers from 1 to 100: $sum');
}
```

### **Exercise 6: Age Group Messages**
Create a `switch` statement that prints different messages based on a person's age range.

```dart
void main() {
  int age = 25;
  switch (age ~/ 10) {
    case 0:
    case 1:
        print('Child');
        break;
    case 2:
    case 3:
    case 4:
    case 5:
        print('Adult');
        break;
    case 6:
    case 7:
    case 8:
    case 9:
        print('Senior');
        break;
    default:
        print('Unknown age range');
  }
}
```

## 3. Function Practice

### **Exercise 7: Sum Function**
Write a function that takes two numbers and returns their sum.

```dart
int add(int a, int b) {
  return a + b;
}

void main() {
  print('Sum: ${add(3, 5)}');
}
```

### **Exercise 8: Average Calculator**
Create a function that takes a list of numbers and returns their average.

```dart
double average(List<int> numbers) {
  int sum = 0;
  for (int number in numbers) {
    sum += number;
  }
  return sum / numbers.length;
}

void main() {
  print('Average: ${average([10, 20, 30, 40, 50])}');
}
```

### **Exercise 9: Flexible Calculator**
Use a `typedef` to create a flexible calculator function that can handle addition, subtraction, multiplication, and division.

```dart
typedef Operation = int Function(int x, int y);

int add(int x, int y) => x + y;
int subtract(int x, int y) => x - y;
int multiply(int x, int y) => x * y;
int divide(int x, int y) => (y != 0) ? x ~/ y : 0;

int calculate(int x, int y, Operation operation) => operation(x, y);

void main() {
  print('Addition: ${calculate(10, 5, add)}');
  print('Subtraction: ${calculate(10, 5, subtract)}');
  print('Multiplication: ${calculate(10, 5, multiply)}');
  print('Division: ${calculate(10, 5, divide)}');
}
```

## 4. Additional Resources

- [Dart Language Tour](https://dart.dev/guides/language/language-tour)
- [DartPad (Online Editor)](https://dartpad.dev/)
- [Dart API Documentation](https://api.dart.dev/)
