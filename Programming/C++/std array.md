---
создал заметку: 22-04-2025
tags:
  - Плюсы
---
### Описание
**`array`** — это **контейнер-обёртка над обычным C-массивом**, который:
- имеет **фиксированный размер** (на этапе компиляции)
- хранится **на стеке**
- не может изменить размер
- быстрее и проще по памяти
```cpp
std::array<int, 3> arr = {1, 2, 3};
arr[1] = 42; // доступ по индексу
```
### Резюме
==Кратко подведите итоги==.