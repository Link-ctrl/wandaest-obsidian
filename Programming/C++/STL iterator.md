---
создал заметку: 22-04-2025
tags:
  - Плюсы
---
### Описание
🔹 **Итератор** — это **объект**, который **указывает на элемент контейнера STL** и позволяет **перемещаться по элементам**, как указатель по массиву.
> 💬 Итератор = умный указатель + доступ к элементу + поведение по стандарту

Пример:
```cpp
std::vector<int> v = {1, 2, 3};
auto it = v.begin();

std::cout << *it << "\n"; // разыменование: 1
++it;                     // переход к следующему элементу
```

### Итераторы и указатели
Вектор:
```cpp
int* ptr = &v[0];
```
Итератор:
```cpp
std::vector<int>::iterator it = v.begin();
```

### STL использует **5 категорий итераторов**:
| Тип итератора             | Возможности                         | Где встречается                               |
| ------------------------- | ----------------------------------- | --------------------------------------------- |
| **InputIterator**         | 🔸 Только чтение, только вперёд     | `std::istream_iterator`, `for (auto x : ...)` |
| **OutputIterator**        | 🔸 Только запись, только вперёд     | `std::back_inserter`, `ostream_iterator`      |
| **ForwardIterator**       | 🔁 Чтение + многократный проход     | `std::forward_list`, `unordered_set`          |
| **BidirectionalIterator** | 🔁 + движение назад                 | `std::list`, `std::map`, `std::set`           |
| **RandomAccessIterator**  | 🔁 + `it + n`, `it[n]`, `it1 < it2` | `std::vector`, `std::deque`, `std::array`     |
### Примеры поведения:
```cpp
*it           // доступ к элементу
++it          // перейти к следующему
--it          // (если можно) — назад
it + 3        // (если RA) перейти на 3 вперёд
std::sort(v.begin(), v.end()); // требует RandomAccess
```

### Итератор **зависит от контейнера**
|Контейнер|Итератор|
|---|---|
|`std::vector`|RandomAccess|
|`std::deque`|RandomAccess|
|`std::list`|Bidirectional|
|`std::set/map`|Bidirectional|
|`std::unordered_set`|Forward|
|`std::forward_list`|Forward|
### Резюме
> **Итератор** — это универсальный "указатель" в STL,  
> с помощью которого можно проходить по элементам,  
> читать, писать, сортировать и применять алгоритмы.

📌 Бывает 5 видов, и **алгоритмы STL работают только с нужными категориями**  
(`sort` — только с random access, `find` — с любыми)