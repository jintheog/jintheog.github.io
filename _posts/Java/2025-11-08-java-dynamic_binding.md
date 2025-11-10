---
title: "Dynamic binding, runtime polymorphism"
date: 2025-11-08 00:00:00 +0900
categories: [Java]
tags: [OOP, Java]
---

```java
package d.inheritance.practice_02;

class Animal {
    String name;
    int age;

    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void makeSound() {
        System.out.println("arrrr");
    }

    public String getSpecies() {
        return "동물";
    }
}

class Lion extends Animal {
    public Lion(String name, int age) {
        super(name, age);
    }
    @Override
    public void makeSound() {
        System.out.println("어흥!");
    }

    @Override
    public String getSpecies() {
        return "사자";
    }
}

class Elephant extends Animal {
    public Elephant(String name, int age) {
        super(name, age);
    }
    @Override
    public void makeSound() {
        System.out.println("뿌우우!");
    }
    @Override
    public String getSpecies() {
        return "코끼리";
    }
}

class Monkey extends Animal {
    public Monkey(String name, int age) {
        super(name, age);
    }
    @Override
    public void makeSound() {
        System.out.println("끼끼!");
    }
    @Override
    public String getSpecies() {
        return "원숭이";
    }
}

class Zoo {
    Animal[] animals;

    public void feedingTime(){
        System.out.println("=== 먹이 시간 ===");
        for(Animal animal : animals) {
            System.out.print(animal.getSpecies()+" "+ animal.name + ": ");
            animal.makeSound();
        }
    }
}

public class AnimalMain {
    public static void main(String[] args) {
        Animal lion = new Lion("심바", 3);
        Animal elephant = new Elephant("덤보", 3);
        Animal monkey = new Monkey("조조", 3);

        Zoo zoo = new Zoo();
        zoo.animals = new Animal[]{lion, elephant, monkey};
        zoo.feedingTime();
    }
}

```

# 의문점

```java
    Animal lion = new Lion("심바", 3);
    Animal elephant = new Elephant("덤보", 3);
    Animal monkey = new Monkey("조조", 3);
```

- 새로운 각 타입의 동물 객체 생성 후 모두 상위 클래스인 Animal 타입으로 업캐스팅을 함.
- 그러면 ` zoo.animals = new Animal[]{lion, elephant, monkey};` 여기서 대입을 할때 전부 Animal 타입으로 데이터들이 대입이 됨?

```bash
zoo ──▶ [Zoo 객체]
          ├─ animals ──▶ [배열 객체]
          │                ├─ [0] = Lion 객체
          │                ├─ [1] = Elephant 객체
          │                └─ [2] = Monkey 객체
          └─ (다른 필드들...)

```

✅ 형식(타입)은 Animal이지만, 실제로 저장되는 객체는 Lion, Elephant, Monkey 객체 그대로다

- “Animal 타입의 배열” 안에 **“Animal을 상속한 객체들”**이 들어가는 것

```java
zoo.animals = new Animal[]{lion, elephant, monkey};

```

# implicit typecast 묵시적 형변환 (업캐스팅)

```java
  Animal lion = new Lion("심바", 3);
  Animal elephant = new Elephant("덤보", 3);
  Animal monkey = new Monkey("조조", 3);
```

### 1️⃣ new Lion("심바", 3)

- → 실제로 Lion 객체를 메모리에 만든다.
- → 이 객체는 Animal을 상속했기 때문에 Animal 타입으로도 간주될 수 있음.

### 2️⃣ Animal lion = ...

- → 변수의 타입은 Animal
- → 오른쪽의 Lion 객체를 자동으로 Animal 타입으로 변환(업캐스팅) 해서 대입함.
- → 즉, Lion 객체의 주소(reference) 가 Animal 타입 변수에 저장돼.

```bash
[Heap 메모리]
 └── (Lion 객체)
       ├── name = "심바"
       ├── age = 3
       └── makeSound() → "어흥!"

[Stack 메모리]
 └── lion (Animal 타입 참조 변수)
         └── → (Lion 객체의 주소)

```

- “lion, elephant, monkey는 전부 업캐스팅해서 Animal 타입 변수에 담았다.”
  - ✅ yes
- “그리고 new Animal[]로 Animal 타입 배열을 만들었다.”
  - ✅ yes
- “그럼 Zoo의 animals 배열 안에는 Animal 타입 객체들이 들어있어야 하지 않나?”
  - 🟡 겉보기(형식)로는 맞지만, 실제로는 다르다.

# 핵심: “변수의 타입” vs “객체의 실제 타입”

- 자바에서는 ‘참조 변수의 타입’과 ‘실제 객체의 타입’은 별개

```java
Animal lion = new Lion("심바", 3);
```

- `lion` 변수의 타입(형식) 은 `Animal`

- `lion` 변수 안에 들어 있는 실제 객체(내용) 는 `Lion`

- **“Animal 타입 변수에 Lion 객체의 주소를 저장했다.”**

```bash
[Stack]             [Heap]
lion ───────────▶  (Lion 객체)
                    ↑
                    └─ 실제 타입: Lion
                       부모 타입: Animal

```

# 배열의 경우도 똑같이 작동

```java
zoo.animals = new Animal[]{lion, elephant, monkey};
```

- → 배열의 타입 자체는 Animal[]
- → 배열 안에 저장된 값(참조) 들은 각각

  - Lion 객체의 주소

  - Elephant 객체의 주소

  - Monkey 객체의 주소

즉, 배열 요소들이 “Animal 변수” 역할을 하지만,
그 변수들이 가리키는 건 “자식 객체들(Lion, Elephant, Monkey)”.

# 정리 표

| 항목             | 의미                            | 예시                         |
| ---------------- | ------------------------------- | ---------------------------- |
| **변수 타입**    | 참조(라벨) 타입                 | `Animal lion`                |
| **객체 타입**    | 실제 생성된 인스턴스의 타입     | `new Lion("심바", 3)`        |
| **배열 타입**    | 어떤 타입의 참조를 담을 수 있나 | `Animal[]`                   |
| **배열 안 내용** | 각각이 참조하는 실제 객체       | `Lion`, `Elephant`, `Monkey` |

> Zoo.animals 배열의 각 요소는 Animal 타입의 참조 변수이지만,
> 그 참조변수가 가리키는 실제 객체는 각각 자식 클래스(Lion, Elephant, Monkey) 다.

> 즉, 배열의 타입은 Animal이고,
> 배열 속의 실제 객체 타입은 Animal이 아니라 자식 클래스들이다.

---

# 1️⃣ 업캐스팅 (Upcasting)

```java
Animal lion = new Lion("심바", 3);
```

- 왼쪽(참조 변수 타입): Animal
- 오른쪽(객체 실제 타입): Lion

즉, 이 변수는 **Animal처럼 보이지만**, 실제로는 **Lion 객체를 가리키는 포인터(참조)** 야.

# 2️⃣ Zoo의 배열

```java
zoo.animals = new Animal[]{lion, elephant, monkey};
```

- → 전부 Animal 타입 배열에 저장되었지만
- → 각 칸에는 Lion, Elephant, Monkey 객체가 들어 있음.

# 3️⃣ 다형성의 핵심 (Dynamic Dispatch)

```java
for (Animal animal : animals) {
    System.out.print(animal.getSpecies() + " " + animal.name + ": ");
    animal.makeSound();
}
```

- 여기서 animal 변수는 분명히 Animal 타입이다.
- 그런데 자바의 메서드 호출은 **“동적 바인딩(Dynamic Binding)”** 규칙을 따르기 때문에 **실제 객체의 타입**을 기준으로 어떤 `makeSound()`를 실행할지 결정

| 실제 객체 타입 | 호출되는 메서드            |
| -------------- | -------------------------- |
| `Lion`         | `Lion`의 `makeSound()`     |
| `Elephant`     | `Elephant`의 `makeSound()` |
| `Monkey`       | `Monkey`의 `makeSound()`   |

| 개념               | 설명                                                                     |
| ------------------ | ------------------------------------------------------------------------ |
| **컴파일 시점**    | 참조 변수의 타입(`Animal`) 기준으로 검사함 (메서드가 존재하는지 확인)    |
| **실행 시점**      | 실제 객체 타입(`Lion`, `Elephant`, `Monkey`) 기준으로 실행할 메서드 결정 |
| **결과**           | 오버라이딩된 메서드가 호출됨                                             |
| **이 개념의 이름** | 다형성 (Polymorphism) / 동적 바인딩 (Dynamic Binding)                    |

# 동적 바인딩

- 바인딩은 메서드 호출과 실제 실행될 메서드를 연결하는 과정

| 구분 | 정적 바인딩       | 동적바인딩          |
| ---- | ----------------- | ------------------- |
| 시점 | 컴파일 타임       | 런타임              |
| 대상 | 오버로딩된 메서드 | 오버라이딩된 메서드 |
| 결정 | 참조 타입 기준    | 실제 객체 타입 기준 |
| 성능 | 빠름              | 상대적으로 느림     |

# 🔍 “바인딩(binding)”이란?

- “바인딩”은 **‘어떤 메서드를 실행할지 결정하는 시점’**을 뜻함
- 자바가 `animal.makeSound();` 같은 코드를 실행할 때, 이게 **어떤 클래스의 makeSound()를 호출해야 하는지** 결정하는 순간이 바인딩이다.

# 정적 바인딩(Static Binding) vs 동적 바인딩(Dynamic Binding)

| 구분                              | 시점                    | 설명                                                        | 예시                                |
| --------------------------------- | ----------------------- | ----------------------------------------------------------- | ----------------------------------- |
| **정적 바인딩 (Static Binding)**  | **컴파일 시점**         | 어떤 메서드/변수를 쓸지 **컴파일러가 미리 결정**            | `private`, `static`, `final` 메서드 |
| **동적 바인딩 (Dynamic Binding)** | **실행 시점 (Runtime)** | 실제 객체의 타입을 기준으로 **어떤 메서드를 실행할지** 결정 | 오버라이딩된 인스턴스 메서드        |

# 예시

```java
public class DynamicBinding {
    public static void main(String[] args) {
        Animal[] animals = {
            new Dog(),
            new Cat(),
            new Dog()
        };

        for (Animal animal : animals) {
            // 컴파일 시점: Animal.makeSound() 호출 계획
            // 런타임 시점: 실제 객체의 makeSound() 실행 (동적 바인딩)
            animal.makeSound();
        }
    }
}
```

```bash
멍멍!
야옹!
멍멍!
```

# 예시 2

```java
class Animal {
    public void makeSound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("멍멍!");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();  // 업캐스팅
        a.makeSound();         // 어떤 makeSound()가 호출될까?
    }
}

```

```bash
멍멍!
```

# 왜 Animal 타입인데 Dog의 메서드가 실행될까?

이게 바로 동적 바인딩:

1. 컴파일 시점

- 컴파일러는 변수 타입(Animal)만 보고 “makeSound()라는 메서드가 존재하는지”만 확인.

- "Animal에 makeSound()가 있네? OK, 컴파일 통과."

2. 실행 시점

- JVM이 실제로 객체(new Dog())를 보고 “아, 이건 Dog 객체구나!”

- 그래서 Dog 클래스의 오버라이딩된 makeSound() 를 실행한다.

즉,

> 컴파일러는 “형식(Animal)” 기준으로 검사하고,
> 실행기는 “실체(Dog)” 기준으로 실행한다.

```scss
Animal a ───▶ (Dog 객체)
               ↑
               └─ makeSound() → "멍멍!"
```

- 변수 a는 Animal 타입이지만,

- 가리키는 실제 객체는 Dog

- 호출 시 JVM은 “Dog에 오버라이딩된 makeSound()”를 찾아 실행

| 구분      | 정적 바인딩                   | 동적 바인딩                       |
| --------- | ----------------------------- | --------------------------------- |
| 결정 시점 | 컴파일 타임                   | 런타임                            |
| 기준      | 변수의 타입                   | 실제 객체의 타입                  |
| 적용 대상 | static, final, private 메서드 | 오버라이딩된 인스턴스 메서드      |
| 대표 예시 | `Math.max(a, b)`              | `animal.makeSound()` (오버라이딩) |

> 동적 바인딩(Dynamic Binding) 이란, 부모 타입의 참조 변수가 오버라이딩된 메서드를 호출할 때, 실행 시점에 실제 객체의 타입을 기준으로 메서드를 결정하는 것.

```java
for (Animal animal : animals) {
    animal.makeSound();
}
```

- 이 부분이 바로 **동적 바인딩의 대표적인 예시**.
- 컴파일러는 “Animal에 makeSound()가 있나?”만 확인하지만,
- 실행할 땐 각각 `Lion`, `Elephant`, `Monkey`의 메서드가 실제로 호출되는 것.
