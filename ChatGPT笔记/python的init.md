在 Python 中，`__init__` 是类的构造方法，用于初始化类的实例（对象）。当你创建一个类的对象时，`__init__` 方法会自动被调用，它可以用于为对象设置初始状态或初始化属性。

### 1. **作用**
- **初始化属性**：为类的实例赋初始值。
- **执行初始化逻辑**：在对象创建时执行一些准备工作，比如验证参数、打开文件等。
- **自动调用**：无需手动调用，创建对象时由 Python 自动执行。

---

### 2. **语法**
```python
class ClassName:
    def __init__(self, 参数1, 参数2, ...):
        # 初始化代码
        self.属性名 = 参数1
        self.属性名2 = 参数2
```

- `self` 是指向实例对象的引用，用于访问该对象的属性和方法。
- `__init__` 方法可以接收额外的参数，这些参数通常用来初始化对象属性。

---

### 3. **示例**

#### 基本使用
```python
class Person:
    def __init__(self, name, age):
        self.name = name  # 初始化 name 属性
        self.age = age    # 初始化 age 属性

# 创建对象时，__init__ 会被自动调用
person = Person("Alice", 30)
print(person.name)  # 输出: Alice
print(person.age)   # 输出: 30
```

#### 带有默认值的初始化
```python
class Person:
    def __init__(self, name="Unknown", age=0):
        self.name = name
        self.age = age

# 不传参数时使用默认值
person1 = Person()
print(person1.name)  # 输出: Unknown

# 传入参数时覆盖默认值
person2 = Person("Bob", 25)
print(person2.name)  # 输出: Bob
```

#### 使用 `__init__` 执行初始化逻辑
```python
class FileHandler:
    def __init__(self, file_name):
        self.file_name = file_name
        self.file = open(file_name, 'r')  # 打开文件
        self.content = self.file.read()  # 读取文件内容

file_handler = FileHandler('example.txt')
print(file_handler.content)  # 输出文件内容
```

---

### 4. **注意事项**
- `__init__` 方法并不是强制必须定义的，如果一个类没有 `__init__` 方法，Python 会提供一个默认的空构造函数。
- `__init__` 不是返回对象实例的地方（与其他语言的构造函数不同），而是用来初始化实例。
- 如果需要在对象销毁时执行清理操作，可以定义 `__del__` 方法（但慎用）。

---

### 5. **对比类方法与静态方法**
与 `__init__` 方法不同：
- **类方法**（`@classmethod`）可以操作类本身的属性。
- **静态方法**（`@staticmethod`）不需要访问类或实例的任何属性，用作与类相关的工具方法。
