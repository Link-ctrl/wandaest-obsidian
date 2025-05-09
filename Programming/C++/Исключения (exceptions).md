---
создал заметку: 22-04-2025
tags:
  - База
  - Плюсы
---
### Описание
**Исключение** — это **механизм обработки ошибок**, при котором выполнение программы **немедленно прерывается** и **передаётся в блок обработки**, минуя обычный ход исполнения.
> 💬 Говоря проще: это способ сказать "что-то пошло не так" — и передать управление в другое место.

### Как работает?
- Ты **выбрасываешь исключение** через `throw`
- Исполнение **немедленно прерывается**
- Компилятор **ищет ближайший `try {}` блок с подходящим `catch`**
- Выполняется `catch`
- После этого можно продолжать работу

Пример:
```cpp
#include <iostream>
#include <stdexcept>

void mayFail(bool fail) {
    if (fail)
        throw std::runtime_error("Something went wrong!");
}

int main() {
    try {
        mayFail(true);
        std::cout << "Все хорошо!\n";
    } catch (const std::runtime_error& e) {
        std::cerr << "Ошибка: " << e.what() << "\n";
    }
}
```
Вывод:
```cpp
Ошибка: Something went wrong!
```

### Синтаксис
#### 1. `throw`

```cpp
throw std::runtime_error("Ошибка");
```
Можно выбрасывать любой тип (обычно — `class`, желательно наследник `std::exception`)
#### 2. `try / catch`
```cpp
try {
    // потенциально опасный код
} catch (const SomeExceptionType& e) {
    // обработка
}
```
Можно цепочку `catch`-блоков:
```cpp
catch (const std::invalid_argument& e) { ... }
catch (const std::runtime_error& e) { ... }
catch (...) { std::cerr << "Неизвестная ошибка!\n"; }
```
#### 3. `noexcept`
Ключевое слово, указывающее, **может ли функция выбрасывать исключения**
```cpp
void f() noexcept;        // гарантированно не выбрасывает
void g() noexcept(false); // может выбрасывать
```

### Иерархия исключений в STL:
```cpp
std::exception
 ├── std::logic_error
 │    ├── std::invalid_argument
 │    └── std::domain_error
 └── std::runtime_error
      ├── std::overflow_error
      └── std::underflow_error
```

### Когда использовать исключения:
✅ Для **ошибок времени выполнения**, которые:
- **не являются нормальной логикой** (`деление на 0`, `файл не найден`, `ошибка выделения памяти`)
- **требуют выйти из глубокой вложенности функций**

### Когда **не стоит** использовать:
- Для **нормального управления потоком**
- В **реальном времени / embedded** (дорогая стоимость)
- Когда используешь альтернативы: `std::optional`, `std::expected`, коды ошибок
### Резюме
> **Исключения** в C++ — это мощный способ обработки ошибок:  
> вместо проверок возвращаемых значений, ты используешь `throw`, `try`, `catch`  
> и получаешь **чистый, безопасный и читаемый код**.

📌 Главное:
- `throw` — выбрасываем ошибку
- `try` — защищаем участок
- `catch` — обрабатываем