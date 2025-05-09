---
создал заметку: 21-04-2025
tags:
  - Плюсы
---
### Описание
Это **техника, позволяющая передать параметры в точности в том виде, как они были переданы** в обёртку — будь то:
- `int&`
- `const std::string&`
- `std::vector<int>&&`  
Цель: не терять **категорию значения** (lvalue/rvalue) и квалификаторы (`const`, `volatile` и др.).
### Когда это нужно?
1. **Обёртки для функций** — например, логгеры, профайлеры, трейсеры.
2. **Фабрики** — когда вы создаёте объект, передавая ему аргументы, которые пришли "снаружи", как есть.
3. **Шаблонные прокс-функции**, работающие с разными типами и не должны мешать семантике вызова.
### Как это реализуется?
С помощью **универсальных ссылок (universal / forwarding references)** + `std::forward`.
```cpp
template<typename T>
void wrapper(T&& arg) {
    callee(std::forward<T>(arg)); // идеальная передача
}
```
Здесь:
- `T&&` — **универсальная ссылка**, которая "схлопывается" в `T&` или `T&&` в зависимости от переданного аргумента.
- `std::forward<T>(arg)` — сохраняет **категорию значения**, передавая:
    - lvalue как lvalue
    - rvalue как rvalue
### Что будет без `std::forward`?
Если просто написать:
```cpp
Person(std::move(name));
```
или
```cpp
Person(name);
```
то **все значения будут передаваться как lvalue**, потому что `name` — это lvalue **внутри функции**, даже если вы передали rvalue извне!
 `std::forward<T>(name)` делает правильное "пробрасывание".
### Важные замечания:
- Работает только с **шаблонными параметрами типа**.
- Не работает с перегрузками без шаблонов (`void func(int&&)` и `void func(int&)` — это не perfect forwarding).
- Без `std::forward` теряется смысл.
### Резюме
**Perfect forwarding** — ключевая идиома современного C++ (начиная с C++11), позволяющая писать эффективный, гибкий и безопасный код, не теряя семантики переданных параметров. Применяется в **STL**, **библиотеках**, **реализациях контейнеров**, **factory-функциях**, и везде, где шаблоны передают параметры "дальше".