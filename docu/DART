# Dart 기본 문법 정리

## 1. 변수 타입

### **정수 (int)**  
```dart
int number = 1;
```

### **실수 (double)**  
```dart
double number = 2.5;
```

### **불리언 (bool)**  
```dart
bool isTrue = true;
bool isFalse = false;
```

### **문자열 (String)**  
```dart
String name = 'RedVelvet';
```
- 문자열 내에서 변수 사용: 
```dart
String name = 'BlackPink';
print('My favorite group is $name'); // My favorite group is BlackPink
```

### **타입 추론 (var)**  
```dart
var name = 'BlackPink';
var number = 20;
print(name.runtimeType); // String
print(number.runtimeType); // int
```
- 복잡한 타입일 때 유용:
```dart
var data = {'key': [1.0, 2.0, 3.0]};
print(data.runtimeType); // Map<String, List<double>>
```

### **동적 타입 (dynamic)**  
```dart
dynamic value = 'Hello';
print(value.runtimeType); // String
value = 42;
print(value.runtimeType); // int
```

### **nullable과 non-nullable**  
```dart
String? name = 'Code';
name = null; // 가능

String nonNullableName = 'Code';
nonNullableName = null; // 오류
```
- null 안전 연산자:
```dart
String? name = null;
print(name?.length); // null 반환
```
- 기본값 지정:
```dart
String? name = null;
String result = name ?? 'default';
print(result); // default
```

### **final과 const**  
- **final**: 런타임에 결정되는 값
```dart
final currentTime = DateTime.now();
print(currentTime);
```
- **const**: 컴파일 타임에 결정되는 값
```dart
const fixedValue = 42;
print(fixedValue);
```

## 2. 자료구조

### **List**  
```dart
List<String> blackPink = ['Jennie', 'Jisoo', 'Rosé', 'Lisa'];
print(blackPink[0]); // Jennie
print(blackPink.length); // 4
blackPink.add('Code');
print(blackPink); // ['Jennie', 'Jisoo', 'Rosé', 'Lisa', 'Code']
blackPink.remove('Code');
print(blackPink); // ['Jennie', 'Jisoo', 'Rosé', 'Lisa']
```

### **Map**  
```dart
Map<String, String> dictionary = {
  'Harry': '해리',
  'Ron': '론',
  'Hermione': '헤르미온느'
};
dictionary['Draco'] = '드레이코';
print(dictionary);
```

### **Set**  
```dart
final Set<String> names = {'a', 'b', 'b', 'c'};
print(names); // {a, b, c}
print(names.contains('a')); // true
```

## 3. 연산자

### **기본 연산자**  
```dart
int number = 2;
number++;
print(number); // 3
number--;
print(number); // 2
```

### **비교 연산자**  
```dart
int number = 42;
print(number > 20); // true
print(number == 42); // true
```

### **논리 연산자**  
```dart
bool result = (12 > 10) && (5 < 10);
print(result); // true
```

### **null 조건 연산자**  
```dart
double? number = null;
number ??= 3.0;
print(number); // 3.0
```

## 4. 제어문

### **if문**  
```dart
int number = 10;
if (number > 5) {
  print('5보다 큽니다');
} else {
  print('5보다 작거나 같습니다');
}
```

### **switch문**  
```dart
int number = 2;
switch (number % 3) {
  case 0:
    print('0');
    break;
  case 1:
    print('1');
    break;
  default:
    print('2');
}
```

### **for loop**  
```dart
List<int> numbers = [1, 2, 3, 4, 5];
int total = 0;
for (int i = 0; i < numbers.length; i++) {
  total += numbers[i];
}
print(total);
```

### **while loop**  
```dart
int count = 0;
while (count < 5) {
  print(count);
  count++;
}
```

## 5. 함수

### **기본 함수**  
```dart
int add(int a, int b) {
  return a + b;
}
```

### **optional parameter**  
```dart
void greet(String name, [String? message]) {
  print('Hello, $name ${message ?? ''}');
}
greet('Alice'); // Hello, Alice 
greet('Alice', 'Good morning'); // Hello, Alice Good morning
```

### **named parameter**  
```dart
void greet({required String name, String message = 'Hello'}) {
  print('$message, $name');
}
greet(name: 'Alice'); // Hello, Alice
greet(name: 'Alice', message: 'Good morning'); // Good morning, Alice
```

### **arrow function**  
```dart
int add(int a, int b) => a + b;
```

### **typedef**  
```dart
typedef Operation = int Function(int x, int y);
int add(int x, int y) => x + y;
int multiply(int x, int y) => x * y;
int calculate(int x, int y, Operation op) => op(x, y);
print(calculate(2, 3, add)); // 5
print(calculate(2, 3, multiply)); // 6
```

## 6. 참고 자료

- [Dart 언어 공식 문서](https://dart.dev/guides)
- [DartPad - 온라인 Dart 실행기](https://dartpad.dev/)
