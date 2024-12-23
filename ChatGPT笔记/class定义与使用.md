### Python 中的 `class`：定义和使用

在 Python 中，`class` 是创建自定义对象的基础。它是一种蓝图或模板，用于定义对象的属性和行为。通过 `class` 创建的对象叫做**实例**，它们可以包含数据（属性）和功能（方法）。

---

### 1. **定义一个类**

#### 语法：
```python
class ClassName:
    # 类的属性和方法
    def method(self):
        pass
```

- **`class`**：关键字，用于声明一个类。
- **`ClassName`**：类的名称，建议使用 PascalCase（首字母大写）。
- **`self`**：指向当前对象的引用，用于访问实例属性和方法。

---

### 2. **类的组成部分**

#### （1）**属性**  
属性是存储在类或实例中的数据。可以分为：
- **实例属性**：每个对象独有的属性，通过 `self` 访问。
- **类属性**：所有对象共享的属性，通过类名访问。

示例：
```python
class Car:
    # 类属性
    wheels = 4

    def __init__(self, brand, color):
        # 实例属性
        self.brand = brand
        self.color = color

# 创建对象
car1 = Car("Toyota", "Red")
car2 = Car("Honda", "Blue")

print(car1.wheels)  # 输出: 4 (通过实例访问类属性)
print(car1.brand)   # 输出: Toyota (实例属性)
```

#### （2）**方法**  
方法是定义在类中的函数，用于定义对象的行为。

- **实例方法**：操作实例的属性或执行特定行为，必须传入 `self`。
- **类方法**：通过 `@classmethod` 装饰器定义，操作类本身，使用 `cls` 参数。
- **静态方法**：通过 `@staticmethod` 装饰器定义，与类关联但不依赖实例或类本身。

示例：
```python
class MathOperations:
    # 静态方法
    @staticmethod
    def add(a, b):
        return a + b

    # 类方法
    @classmethod
    def info(cls):
        return f"This is the {cls.__name__} class"

# 使用静态方法
print(MathOperations.add(5, 3))  # 输出: 8

# 使用类方法
print(MathOperations.info())  # 输出: This is the MathOperations class
```

---

### 3. **创建和使用类**

#### （1）创建对象
通过类创建的对象称为**实例**。实例会调用类的构造方法（`__init__`）初始化对象的状态。

```python
class Dog:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed

# 创建对象
dog1 = Dog("Buddy", "Golden Retriever")
dog2 = Dog("Charlie", "Poodle")

# 访问属性
print(dog1.name)  # 输出: Buddy
print(dog2.breed) # 输出: Poodle
```

#### （2）修改属性
可以直接通过实例对象修改属性值。
```python
dog1.name = "Max"
print(dog1.name)  # 输出: Max
```

---

### 4. **继承（Inheritance）**

通过继承，可以创建一个基于现有类的新类，从而重用和扩展现有代码。子类继承父类的所有属性和方法，可以覆盖或添加新功能。

```python
class Animal:
    def speak(self):
        print("Animal makes a sound")

class Dog(Animal):
    def speak(self):
        print("Dog barks")

# 创建实例
dog = Dog()
dog.speak()  # 输出: Dog barks
```

- 子类可以使用 `super()` 调用父类的方法：
```python
class Dog(Animal):
    def speak(self):
        super().speak()  # 调用父类的 speak 方法
        print("Dog barks")

dog = Dog()
dog.speak()
# 输出:
# Animal makes a sound
# Dog barks
```

---

### 5. **封装与访问控制**

#### （1）访问控制
- **公共属性/方法**：默认是公开的，任何地方都可以访问。
- **私有属性/方法**：通过双下划线（`__`）定义，外部不能直接访问，只能通过类内部方法访问。

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.__balance = balance  # 私有属性

    def get_balance(self):
        return self.__balance

# 创建对象
account = BankAccount("Alice", 1000)
print(account.get_balance())  # 输出: 1000
```

---

### 6. **多态（Polymorphism）**

多态允许子类在不同的上下文中以不同的方式使用父类的方法。

```python
class Shape:
    def area(self):
        pass

class Circle(Shape):
    def area(self):
        print("Calculating area of a circle")

class Square(Shape):
    def area(self):
        print("Calculating area of a square")

# 多态示例
shapes = [Circle(), Square()]
for shape in shapes:
    shape.area()
```

---

### 7. **特殊方法（Magic Methods）**

特殊方法以双下划线包围，允许在类中定义自定义行为。例如：
- `__init__`：构造方法
- `__str__`：定义对象的字符串表示
- `__add__`：定义 `+` 运算符行为

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)

p1 = Point(2, 3)
p2 = Point(1, 1)
p3 = p1 + p2  # 调用 __add__
print(p3.x, p3.y)  # 输出: 3 4
```

---

### 8. **小结**

- **类是代码复用和组织的强大工具**。
- 支持面向对象的基本概念：**封装**、**继承** 和 **多态**。
- 灵活支持特殊方法、自定义行为和动态属性。
