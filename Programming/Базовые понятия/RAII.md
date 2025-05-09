---
создал заметку: 21-04-2025
tags:
  - База
  - Плюсы
---
### Описание
**RAII** расшифровывается как:

> **Resource Acquisition Is Initialization**  
> (_Приобретение ресурса есть инициализация_)

Это **ключевая идиома C++**, означающая:

> Ресурс (память, файл, mutex и т.п.) должен быть привязан к объекту, и освобождаться в его **деструкторе**.
### Принцип:
- **Ресурсы получают в конструкторе**
- **Ресурсы освобождают в деструкторе**
- **Выход из области видимости (в том числе при исключении)** → автоматическое освобождение

### Простой пример RAII: память
```cpp
#include <memory>

void foo() {
    std::unique_ptr<int> p = std::make_unique<int>(42);
    // память автоматически освободится при выходе из foo()
}
```
Здесь:
- `std::unique_ptr` — RAII-объект
- Он сам освобождает `int` в деструкторе

### Зачем нужна идеология RAII?
|Проблема без RAII|Решение с RAII|
|---|---|
|Лёгко забыть вызвать `delete`|Деструктор всё делает сам|
|Утечки при `throw`|RAII гарантирует освобождение|
|Нужно вручную вызывать `close()`|RAII вызывает в `~деструкторе()`|
|Код становится хрупким|Код становится безопасным и компактным|
### Типичные ресурсы под RAII:

- **Память** (`new` / `delete`) → `unique_ptr`
- **Файлы** (`fopen` / `fclose`) → `fstream`
- **Мьютексы** → `lock_guard`, `scoped_lock`
- **Сокеты, дескрипторы** → обёртки с деструкторами
- **Операции в БД** (транзакции) → обёртка-RAII-коммит

### Ключевые свойства RAII:

- Работает даже при **исключениях**
- Не требует `try-finally` (как в Java/Python)
- Снижает сложность и количество ошибок
- Позволяет использовать **стек** для контроля ресурсов

### Классический C++ стиль с RAII:
```cpp
class FileGuard {
    FILE* f;
public:
    FileGuard(const char* name) { f = fopen(name, "w"); }
    ~FileGuard() { if (f) fclose(f); }
};

void do_work() {
    FileGuard fg("out.txt");
    // RAII: файл будет закрыт при выходе из функции
}
```
### Резюме
**RAII** — это **основа современного C++**.  
Она делает управление ресурсами **безопасным, лаконичным и устойчивым к исключениям**.

Если ты используешь `unique_ptr`, `fstream`, `lock_guard` — ты уже применяешь RAII, даже не задумываясь.