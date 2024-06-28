# Задание №1
Условие: на языке Python или C++ написать алгоритм (функцию) определения четности целого числа, который будет аналогичен нижеприведенному по функциональности, но отличен по своей сути. Объяснить плюсы и минусы обеих реализаций. 
Пример: 
``` python
def isEven(value):

      return value % 2 == 0
```

## Решение
Можно приобразовать число в строку и определить имеется ли в конце этого числа одна из данных цифр: 0, 2, 4, 6, 8. Если последняя цифра числа является одной из вышеперечисленных цифр, то число - четное.
```python
def isEven(num):
  return str(num)[-1] in "02468"]
```
Но этот алгоритм работает ооочень долго. Так как он выполняет много действий. Намного быстрее работает следующий способ:
```python
def isEven(num):
  return not (num & 1) 
```
Этот способ лучше, так как побитовые операции работают быстрее. Также оператор `%` выполняет два действия: деление и вычитание, это тоже снижает скорость работы.

# Задание №2
На языке Python или С++ написать минимум по 2 класса реализовывающих циклический буфер FIFO. Объяснить плюсы и минусы каждой реализации.

## Первый способ
 
```python
class CycleFIFO:

    def __init__(self, capacity=10):
        self.capacity = capacity
        self.front = 0
        self.end = 0
        self.count = 0
        self.FIFO = list(range(0, capacity))

    def __move_left(self):
        if self.front < self.capacity - 1:
            self.front += 1
        else:
            self.front = 0

    def __change_count(self):
        if self.count < self.capacity:
            self.count += 1
        else:
            pass

    def add_value(self, value):
        if self.end <= self.capacity - 1:
            if (self.end == self.front) and (self.count != 0):
                self.__move_left()
            self.FIFO[self.end] = value
            self.end += 1
            self.__change_count()
        else:
            self.end = 0
            if (self.end == self.front) and (self.count != 0):
                self.__move_left()
            self.FIFO[self.end] = value
            self.end += 1
            self.__change_count()

    def pop_front(self):
        if self.count > 0:
            self.count -= 1
            self.__move_left()
        else:
            pass

    def get_front_value(self):
        if self.count > 0:
            temp = self.FIFO[self.front]
            return temp
        else:
            return 'FIFO is empty!'

    def show_count(self):
        return 'Count of variables in FIFO: ' + str(self.count)
```

Первый способ реализован с помощью указателей на элементы по индексу. Плюсом данного метода является удаление и добавление элемента за О(1), так как используются указатели на первый и последний элементы.

## Второй способ

```python
class CycleFIFO:

    def __init__(self, capacity=10):
        self.capacity = capacity
        self.count = 0
        self.FIFO = list(range(0, capacity))

    def __change_count(self):
        if self.count < self.capacity:
            self.count += 1
        else:
            pass

    def add_value(self, value):
        self.FIFO[-1] = value
        self.__change_count()
        self.FIFO.insert(0, self.FIFO.pop())

    def get_front_value(self):
        if self.count > 0:
            temp = self.FIFO[self.count - 1]
            return temp
        else:
            return 'FIFO is empty!'

    def pop_front(self):
        for i in range(self.count - 1):
            self.FIFO.append(self.FIFO.pop())
        self.count -= 1

    def show_count(self):
        return 'Count of variables in FIFO: ' + str(self.count)
```
Второй способ реализован с помощью циклического сдвига списка. Плюсом второй реализации является более понятный код, так как нет такого количества логических операций. Но минусом является использование встроенных методов.

# Задание №3 
На языке Python или С++ предложить алгоритм, который быстрее всего (по процессорным тикам) отсортирует данный ей массив чисел. Массив может быть любого размера со случайным порядком чисел (в том числе и отсортированным). Объяснить, почему вы считаете, что функция соответствует заданным критериям.

## Решение
Для данной задачи я выбрал сортировку слиянием. Среднее время выполнения данного алгоритма O(n log n). Данная сортировка хорошо подходит для больших списков и не особо зависит от неудачных наборов данных. Недостатком данного подхода является большие затраты по памяти для хранения временных копий списков.
```python
def merge(left_list, right_list):  
    sorted_list = []
    left_list_index = right_list_index = 0

    # Длина левого и правого списка
    left_list_length, right_list_length = len(left_list), len(right_list)

    for _ in range(left_list_length + right_list_length):
        if left_list_index < left_list_length and right_list_index < right_list_length:
            # Сравниваем первые элементы в начале каждого списка
            # Если первый элемент левого подсписка меньше, добавляем его
            # в отсортированный массив
            if left_list[left_list_index] <= right_list[right_list_index]:
                sorted_list.append(left_list[left_list_index])
                left_list_index += 1
            # Если первый элемент правого подсписка меньше, добавляем его
            # в отсортированный массив
            else:
                sorted_list.append(right_list[right_list_index])
                right_list_index += 1

        # Если достигнут конец левого списка, элементы правого списка
        # добавляем в конец результирующего списка
        elif left_list_index == left_list_length:
            sorted_list.append(right_list[right_list_index])
            right_list_index += 1
        # Если достигнут конец правого списка, элементы левого списка добавляем в отсортированный массив
        elif right_list_index == right_list_length:
            sorted_list.append(left_list[left_list_index])
            left_list_index += 1

    return sorted_list

def merge_sort(arr: list) -> list:  
    # если в списке один элемент, то возвращаем его
    if len(arr) <= 1:
        return arr

    # Находим серидину списка
    mid = len(arr) // 2

    # Сортируем и объединяем подсписки
    left_list = merge_sort(arr[:mid])
    right_list = merge_sort(arr[mid:])

    # Объединяем отсортированные списки в конечный список
    return merge(left_list, right_list)
```
