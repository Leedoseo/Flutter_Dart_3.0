# Dart 3.0 신규 문법

## ✅ 1. 레코드
- 레코드는 포지셔널 파라미터 or 네임드 파라미터 중 한 가지 방식을 적용하여 사용할 수 있음
- 두 방식 모두 괄호안에 `,`로 구분하여 작성

---

### 1-1. 포지셔널 파라미터를 이용한 레코드
- 포지셔널 파라미터를 이용한 레코드는 포지셔널 파라미터로 표시한 타입 순서를 반드시! 지켜야함

```dart
// 레코드

// 포지셔널 파라미터를 이용한 레코드
void main() {
  // 정확히 위치에 어떤 타입의 값이 입력될지 지정할 수 있음
  // (String, int)는 첫 번째 값은 String타입, 두 번쨰 값은 int타입임
  (String, int) minji = ("민지", 20);
  
  print(minji); // (민지, 20)
}
```

---

만약, 레코드의 모든 값을 사용하지 않고 특정 순서의 레코드 값을 가져오고 싶다면 `$`를 사용하면 됨

```dart
void main() {
  (String, int) minji = ("민지", 20);
  
  print(minji.$1); // 민지
  print(minji.$2); // 20
  
  print(minji); // 민지, 20
}
```

---

### 1-2. 네임드 파라미터를 이용한 레코드
- 네임드 파라미터는 입력 순서를 지킬 필요❌
- 다만 네임드 파라미터는 소괄호에 중괄호를 중첩하여 타입과 변수 이름을 `,`로 구분하고 명시해줘야함

```dart
// 네임드 파라미터를 이용한 레코드
void main() {
  // 다른 네임드 파라미터와 마찬가지로 순서 상관X
  ({String name, int age}) minji = (name: "민지", age: 20);
  
  print(minji); // (age: 20, name: 민지)
}
```

---

## ✅ 2. 구조 분해
- 구조분해란 리스트, 맵, 클래스(레코드 등) 같은 복합 데이터를 구성하는 요소를 한 번에 변수로 추출하는 문법

---

### 1. `List`에서의 구조 분해
- Dart 3.0부터 `[a, b] = list` 문법 지원
- 패턴 매칭과 함께 사용 가능함

```dart
// 리스트에서의 구조 분해
void main() {
  // 아래 코드와 동일하지만 구조 분해를 사용하면 간결한 코드 작성 가능
//   final newJeans = ["민지", "해린"];
//   final minji = newJeans[0];
//   final haerin = newJeans[1];
  final [minji, haerin] = ["민지", "해린"];
  
  print(minji);
  print(haerin);
}
```

---

### 1-2. `List`에서의 스프레드 연산자를 이용한 구조 분해
- `...`사용은 Dart 3.2 버전 이상에서만 지원 → 호환성 주의

```dart
void main() {
  final numbers = [1, 2, 3, 4, 5, 6, 7, 8];
  
  // 스프레드 연산자를 사용하게되면 중간의 값들을 버릴 수 있음
  final [x, y, ..., z] = numbers;
  
  print(x);
  print(y);
  print(z);
}
```

---

### 2. `Map`에서의 구조 분해
- `map["key"]`로 꺼내는 것보다 구조 분해가 더 깔끔하고 빠름

```dart
// 맵에서의 구조 분해
void main() {
  final minjiMap = {"name": "민지", "age": 19};
 
  // 위 Map의 구조와 똑같은 구조로 구조 분해하면 됨
  final {"name": name, "age": age} = minjiMap;
  
  print("name: $name"); // name: 민지
  print("age: $age"); // age: 19
}
```

---

### 3. `Class`에서의 구조 분해
- 해당 클래스는 `const`생성자일 필요는 없지만, `final`필드를 갖는 경우에 유용하게 사용됨

```dart
// 클래스에서의 구조 분해
void main() {
  final minji = Idol(name: "민지", age: 19);
  
  // 클래스의 생성자 구조와 똑같이 구조 분해하면 됨
  final Idol(name: name, age: age) = minji;
  
  print(name); // 민지
  print(age); // 19
}

class Idol {
  final String name;
  final int age;
  
  Idol ({
    required this.name,
    required this.age,
  });
```

---

## ✅ 3. `Switch`문 + 추가 기능
- 스위치 표현식(switch expression)
- 패턴 매칭(pattern matching)
- 완전 확인(exhaustiveness checking)
- 가드 절(guard clause)
위 기능이 추가됨

---

### 1. 표현식 기능
- `switch`문이 값을 반환할 수 있는 표현식으로 진화함 -> `=>` 구문 사용해 더 간결하게!

```dart
// 표현식 기능
void main() {
  String dayKor = "월요일";
  
  // switch문이 함수처럼 값을 반환함
  String dayEnglish = switch (dayKor) {
      // =>를 사용하면 switch문 조건에 맞을 때 값을 반환할 수 있음
      "월요일" => "Monday",
      "화요일" => "Tuesday",
      "수요일" => "Wednesday",
      "목요일" => "Thursday",
      "금요일" => "Firday",
      "토요일" => "Saturday",
      "일요일" => "Sunday",
      // _는 default와 같은 의미로 사용
      _ => "Not Found",
  };
  
  print(dayEnglish); // Monday
}
```

---

### 2. 패턴 매칭
- `switch`문을 사용할 때 패턴 매칭을 사용하면 더욱 복잡한 조건을 형성할 수 있음
- 리스트 패턴 매칭은 길이까지 정확히 맞아야함

```dart
// 패턴 매칭 - switch문에 사용하면 더욱 복잡한 조건 사용 가능
void switcher (dynamic anything) {
  switch (anything) {
    // 정확히 'aaa'문자열만 매치함
    case "aaa":
      print("match: aaa");
      break;
    // 정확히 [1, 2] 리스트만 매치함
    case [1, 2]:
      print("match: [1, 2]");
      break;
    // 3개의 값이 들어 있는 리스트를 모두 매치
    case [_, _, _]:
      print("match[_,_,_]");
      break;
    
    // 첫 번째와 두 번째 값에 int가 입력된 Record타입을 매치
    case (String a, int b):
      print("match: (String: $a, int: $b)");
      break;
    default:
        print("no match");
  }
}

void main() {
  switcher("aaa"); // match: aaa
  switcher([1, 2]); // match: [1, 2]
  switcher([3, 4, 5]); // match[_,_,_] 여기서 _는 어떤 값이든 상관 없다는 의미
  switcher([6, 7]); // no match
  switcher(("민지", 19)); // match: (String: 민지, int: 19)
  switcher(8); // no match
}
```

---

### 3. 엄격한 검사
- `switch`문에 엄격한 검사가 추가되어 모든 조건을 확인하고 있는지 빌드할 떄 확인할 수 있음
- Dart 3에서 **nullable 값은 반드시 null도 처리해야 함**
- `case null:` 또는 `default:` 중 **하나라도 없으면 컴파일 에러**

```dart
// 엄격한 검사
void main() {
  // val에 입력 될 수 있는 값은 true, false, null
  bool? val;
  
  // null 조건을 입력하지 않았기 때문에 에러 발생.
  // null case를 추가하거나 default case를 추가해야 에러가 사라짐
  switch (val) {
    case true:
      print("true");
    case false:
      print("false");
//     default:
//       print("null");
  }
}
```
위 코드에서는 지금 `default: print("null");`코드가 주석처리 되어있어서 오류가 발생함 -> 이게 엄격한 검사

---

### 4. 보호 구문
- `switch`문에는 `when`키워드로 보호 구문을 추가할 수 있음
- `when`키워드는 `boolean`으로 반환할 조건을 각 `case`문에 추가할 수 있으며 `when`키워드 뒤에 오는조건이 `true`를 반환하지 않으면 `case`매치가 되지 않음

```dart
// 보호 구문
void main() {
  (int a, int b) val = (1, -1);
  
  // default가 출력됨. 만약에 b값을 0이상으로 변경하면 1, _를 출력할 수 있음
 switch (val) {
   case (1, _) when val.$2 > 0:
     print("1, _");
     break;
   default:
     print("default");
 }
}
```

---

## ✅ 4. 클래스 제한자
- 클래스 제한자 종류
    - `base`
    - `final`
    - `interface`
    - `sealed`
    - `mixin`
- 모든 클래스 제한자는 `class`키워드 앞에 명시함
- 클래스 제한자를 명시한 클래스를 해당 클래스를 사용하는 파일이 아닌 다른 파일에 있어야 정상적으로 기능이 작동함.

---

### 1. `base`제한자

- 특징 : 상속만 가능 / 구현은 불가
- 제한 : 외부 파일에서 상속하려면 자식 클래스도 `base`, `final`, `sealed` 중 하나여야 함

```dart
// a.dart
base class Parent {}

// b.dart
import 'a.dart';

base class Child extends Parent {}   // ✅ 가능
class Child2 extends Parent {}       // ❌ 에러 (제한자 필요) -> 앞에 base가 붙으면 가능

class Child3 implements Parent {}    // ❌ 에러 (base는 구현 불가)
```

---

### 2. `final`제한자
- 특징 : 상속, 구현, 믹스인 전부 불가! 인스턴스 생성은 가능! -> 완전히 봉인된 클래스

```dart
// a.dart
final class Parent {}

// 인스턴스화 가능
Parent parent = Parent();

// 어디서든 아래는 모두 ❌
class Child extends Parent {}        // ❌ 에러
class Child2 implements Parent {}    // ❌ 에러

// b.dart
import "2_a.dart";

// 인스턴스화 가능
Parent parent = Parent();

// extends불가능
class Child extends Parent{};

// implement 불가능
class Child2 implements Parent{};
```

---

### 3. `interface`제한자

- 특징 : 상속 불가 ❌ / 구현은 가능
- 순수 인터페이스 역할만 수행 가능

```dart
// a.dart
interface class Parent{}

// b.dart
import "3_a.dart";

// 인스턴스화 가능
Parent parent = Parent();

// extend 불가능
class Child1 extends Parent{}

// implement 가능
class Child2 implements Parent{}
```

---

### 4. `sealed`제한자

- 특징
    - 같은 파일 내에서만 상속/구현 가능
    - switch/case exhaustiveness 검사에 유용함
 
```dart
// a.dart
sealed class Parent{}

// b.dart
import "4_a.dart";

// 인스턴스화 불가능
Parent parent = Parent();

// extend 불가능
class Child1 extends Parent {}

// implement 불가능
class Child2 implements Parent {}
```

---

### 5. `mixin`제한자

- `mixin`
    - 클래스에 기능을 “붙여주는” 용도 (`with`)
    - `extends`는 불가
 
```dart
mixin Logger {
  void log(String msg) => print("log: $msg");
}

class A with Logger {} // ✅
```

---

- `mixin class`
    - `mixin`처럼 쓰면서도 `extends`까지 가능
 
```dart
// mixin 제한자
mixin class MixinExample{}

// extend 가능
class Child1 extends MixinExample{}

// mixin으로 사용 가능
class Child2 with MixinExample{}
```

--- 

클래스 제한자 총 정리
## ✅ Dart 3.0 클래스 제한자 총정리

| 제한자        | `extends` (상속) | `implements` (구현) | 외부 파일에서 확장 | 인스턴스 생성 | 설명 요약 |
|---------------|------------------|----------------------|---------------------|----------------|------------|
| `base`        | ✅ 가능           | ❌ 불가              | ⛔️ 자식 클래스도 `base`, `final`, `sealed` 중 하나여야 함 | ✅ 가능       | 상속만 허용, 구현 금지 |
| `final`       | ❌ 불가           | ❌ 불가              | ❌ 불가              | ✅ 가능       | 완전히 봉인된 클래스 (확장, 구현, 믹스인 전부 불가) |
| `sealed`      | ✅ 가능           | ✅ 가능              | ❌ (같은 파일에서만 가능) | ❌ 불가       | 같은 파일 안에서만 확장 가능, 패턴 매칭에 유리 |
| `interface`   | ❌ 불가           | ✅ 가능              | ✅ 가능              | ✅ 가능       | 순수 인터페이스 역할, 구현만 허용 |
| `mixin`       | ❌ 불가           | ✅ (with만 가능)     | ✅ 가능              | ❌ 불가       | 다중 기능 조합용 코드 블록 |
| `mixin class` | ✅ 가능           | ✅ 가능              | ✅ 가능              | ✅ 가능       | mixin + 클래스 성질 모두 가짐 (extends도 됨) |
