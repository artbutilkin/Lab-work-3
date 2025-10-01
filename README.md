## 1. Тема лабораторной работы

Объединения и перечисления в языке программирования C

## 2. Постановка каждой задачи

### Задача 1 - указатель на объединение

Написать программу, которая использует указатель на объединение. Создать и проинициализировать переменные в объединении через указатель, затем вывести их значения на экран.

### Задача 2 - побайтовая распечатка числа

Написать программу, которая использует объединение для побайтовой распечатки значения переменной типа unsigned long. Каждый байт должен быть выведен отдельно через указатель на char.

### Задача 3 - перечисление дней недели

Создать перечислимый тип данных для семи дней недели. Реализовать программу, которая выводит на экран значения каждого дня недели как целое число.

### Задача 4 - размеченное объединение

Создать размеченное объединение, заключенное в структуру. Структура должна содержать перечисление, служащее индикатором типа данных в объединении. Создать динамический массив таких структур и реализовать функцию для распечатки содержимого.

### Задача 5 - ввод и хранение данных о студентах

Создать структуру с объединением для хранения различных типов данных: имя студента и возраст (целое число или строка). Реализовать программу для динамического ввода данных о студентах и вывода на экран.

### Задача 6 - управление состояниями системы через enum

Использовать перечисление для управления состояниями системы (старт, стоп, пауза). Написать программу, которая изменяет состояние системы и выводит текущее состояние на экран.

### Задача 7 - оптимизация памяти для хранения данных о температуре и влажности

Создать структуру с битовыми полями для хранения показаний температуры (-50 до +50°C) и влажности (0-100%) с минимальным потреблением памяти.

### Задача 8 - ввод и вывод через enum и union

Создать программу, которая позволяет пользователю вводить и выводить информацию с различными типами данных через перечисления и объединения.

## 3. Математическая модель

### Задача 2

Побайтовый доступ: `unsigned long value = 0x12345678` → байты: `0x78, 0x56, 0x34, 0x12`

### Задача 7

Битовые поля:

- Температура: диапазон -50..+50 → 101 значение → 7 бит (2⁷=128)
    
- Влажность: диапазон 0..100 → 101 значение → 7 бит (2⁷=128)
    

## 4. Список идентификаторов

|Имя переменной|Тип данных|Смысловое обозначение|
|---|---|---|
|`union Data`|`union`|Объединение для разных типов данных|
|`enum Day`|`enum`|Перечисление дней недели|
|`enum DataType`|`enum`|Тип данных в объединении|
|`enum SystemState`|`enum`|Состояния системы|
|`TaggedUnion`|`struct`|Размеченное объединение|
|`Student`|`struct`|Структура студента|
|`SensorData`|`struct`|Данные датчика с битовыми полями|
|`temp`|`int`|Температура|
|`humidity`|`int`|Влажность|

## 5. Код программы

### Задача 1 - указатель на объединение

```
#include <stdio.h>
#include <string.h>

union Data {
    int i;
    float f;
    char str[20];
};

int main_task1() {
    printf("=== Задача 1 - Указатель на объединение ===\n");
    
    union Data data;
    union Data *ptr = &data;
    
    // Инициализация через указатель
    ptr->i = 10;
    printf("Целое число: %d\n", ptr->i);
    
    ptr->f = 3.14f;
    printf("Дробное число: %.2f\n", ptr->f);
    
    strcpy(ptr->str, "Hello Union");
    printf("Строка: %s\n", ptr->str);
    
    // Демонстрация перезаписи
    printf("\nДемонстрация перезаписи (последнее значение):\n");
    printf("i = %d (мусор)\n", ptr->i);
    printf("f = %.2f (мусор)\n", ptr->f);
    printf("str = %s (корректно)\n", ptr->str);
    
    return 0;
}
```

### Задача 2 - побайтовая распечатка числа

```
#include <stdio.h>

union Number {
    unsigned long value;
    unsigned char bytes[sizeof(unsigned long)];
};

int main_task2() {
    printf("\n=== Задача 2 - Побайтовая распечатка числа ===\n");
    
    union Number num;
    num.value = 0x12345678ABCDEF00;
    
    printf("Число: 0x%lX\n", num.value);
    printf("Размер unsigned long: %zu байт\n", sizeof(unsigned long));
    printf("Байты: ");
    
    for (size_t i = 0; i < sizeof(unsigned long); i++) {
        printf("0x%02X ", num.bytes[i]);
    }
    printf("\n");
    
    // Демонстрация с другим числом
    num.value = 255;
    printf("\nЧисло: %lu\n", num.value);
    printf("Байты: ");
    for (size_t i = 0; i < sizeof(unsigned long); i++) {
        printf("0x%02X ", num.bytes[i]);
    }
    printf("\n");
    
    return 0;
}
```

### Задача 3 - перечисление дней недели

```
#include <stdio.h>

enum Day {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
};

const char* day_names[] = {
    "Понедельник", "Вторник", "Среда", "Четверг", 
    "Пятница", "Суббота", "Воскресенье"
};

int main_task3() {
    printf("\n=== Задача 3 - Перечисление дней недели ===\n");
    
    printf("Дни недели и их числовые значения:\n");
    for (enum Day day = MONDAY; day <= SUNDAY; day++) {
        printf("%s = %d\n", day_names[day], day);
    }
    
    // Работа с перечислением
    enum Day today = WEDNESDAY;
    enum Day weekend1 = SATURDAY;
    
    printf("\nСегодня: %s (%d)\n", day_names[today], today);
    printf("Выходной: %s (%d)\n", day_names[weekend1], weekend1);
    
    // Арифметика с перечислениями
    enum Day tomorrow = today + 1;
    printf("Завтра: %s (%d)\n", day_names[tomorrow], tomorrow);
    
    return 0;
}
```

### Задача 4 - размеченное объединение

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

enum DataType { INT, FLOAT, STRING };

struct TaggedUnion {
    enum DataType type;
    union {
        int i;
        float f;
        char str[50];
    } data;
};

void print_tagged_union(struct TaggedUnion *tu) {
    switch (tu->type) {
        case INT:
            printf("Тип: INT, Значение: %d\n", tu->data.i);
            break;
        case FLOAT:
            printf("Тип: FLOAT, Значение: %.2f\n", tu->data.f);
            break;
        case STRING:
            printf("Тип: STRING, Значение: %s\n", tu->data.str);
            break;
        default:
            printf("Неизвестный тип\n");
    }
}

int main_task4() {
    printf("\n=== Задача 4 - Размеченное объединение ===\n");
    
    int size = 3;
    struct TaggedUnion *array = malloc(size * sizeof(struct TaggedUnion));
    
    // Инициализация массива
    array[0].type = INT;
    array[0].data.i = 42;
    
    array[1].type = FLOAT;
    array[1].data.f = 3.14159f;
    
    array[2].type = STRING;
    strcpy(array[2].data.str, "Hello World");
    
    // Вывод массива
    printf("Динамический массив размеченных объединений:\n");
    for (int i = 0; i < size; i++) {
        printf("Элемент %d: ", i);
        print_tagged_union(&array[i]);
    }
    
    free(array);
    return 0;
}
```

### Задача 5 - ввод и хранение данных о студентах

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Student {
    char name[50];
    union {
        int age_num;
        char age_str[20];
    } age;
    int use_string_age; // Флаг: 0 - число, 1 - строка
};

int main_task5() {
    printf("\n=== Задача 5 - Данные о студентах ===\n");
    
    int n;
    printf("Введите количество студентов: ");
    scanf("%d", &n);
    getchar(); // Очистка буфера
    
    struct Student *students = malloc(n * sizeof(struct Student));
    
    for (int i = 0; i < n; i++) {
        printf("\nСтудент %d:\n", i + 1);
        
        printf("Введите имя: ");
        fgets(students[i].name, 50, stdin);
        students[i].name[strcspn(students[i].name, "\n")] = 0; // Удаление \n
        
        printf("Введите возраст как число (0) или строку (1): ");
        scanf("%d", &students[i].use_string_age);
        getchar();
        
        if (students[i].use_string_age == 0) {
            printf("Введите возраст (число): ");
            scanf("%d", &students[i].age.age_num);
            getchar();
        } else {
            printf("Введите возраст (строка): ");
            fgets(students[i].age.age_str, 20, stdin);
            students[i].age.age_str[strcspn(students[i].age.age_str, "\n")] = 0;
        }
    }
    
    // Вывод студентов
    printf("\n=== Список студентов ===\n");
    for (int i = 0; i < n; i++) {
        printf("Студент %d:\n", i + 1);
        printf("  Имя: %s\n", students[i].name);
        printf("  Возраст: ");
        if (students[i].use_string_age == 0) {
            printf("%d лет\n", students[i].age.age_num);
        } else {
            printf("%s\n", students[i].age.age_str);
        }
    }
    
    free(students);
    return 0;
}
```

### Задача 6 - управление состояниями системы через enum

```
#include <stdio.h>

enum SystemState {
    STOPPED,
    RUNNING,
    PAUSED,
    ERROR
};

const char* state_names[] = {
    "ОСТАНОВЛЕНА", "ВЫПОЛНЕНИЕ", "ПАУЗА", "ОШИБКА"
};

void print_state(enum SystemState state) {
    printf("Текущее состояние системы: %s\n", state_names[state]);
}

int main_task6() {
    printf("\n=== Задача 6 - Управление состояниями системы ===\n");
    
    enum SystemState current_state = STOPPED;
    print_state(current_state);
    
    // Симуляция работы системы
    printf("\nЗапуск системы...\n");
    current_state = RUNNING;
    print_state(current_state);
    
    printf("\nПауза...\n");
    current_state = PAUSED;
    print_state(current_state);
    
    printf("\nПродолжение работы...\n");
    current_state = RUNNING;
    print_state(current_state);
    
    printf("\nОшибка!...\n");
    current_state = ERROR;
    print_state(current_state);
    
    printf("\nПерезапуск...\n");
    current_state = STOPPED;
    print_state(current_state);
    
    // Цикл по всем состояниям
    printf("\nВсе возможные состояния:\n");
    for (enum SystemState s = STOPPED; s <= ERROR; s++) {
        printf("  %d - %s\n", s, state_names[s]);
    }
    
    return 0;
}
```

### Задача 7 - оптимизация памяти для хранения данных о температуре и влажности

```
#include <stdio.h>

struct SensorData {
    unsigned int temperature : 7;  // -50..+50 (101 значений) → 7 бит
    unsigned int humidity : 7;     // 0..100 (101 значений) → 7 бит
};

int get_temperature_value(struct SensorData data) {
    return (int)data.temperature - 50;  // Преобразование из смещенного значения
}

int get_humidity_value(struct SensorData data) {
    return (int)data.humidity;
}

int main_task7() {
    printf("\n=== Задача 7 - Оптимизация памяти для датчиков ===\n");
    
    printf("Размер структуры SensorData: %zu байт\n", sizeof(struct SensorData));
    
    struct SensorData sensor;
    int temp, hum;
    
    // Ввод данных
    printf("Введите температуру (-50..+50): ");
    scanf("%d", &temp);
    printf("Введите влажность (0..100): ");
    scanf("%d", &hum);
    
    // Проверка диапазонов
    if (temp < -50 || temp > 50 || hum < 0 || hum > 100) {
        printf("Ошибка: значения вне допустимого диапазона!\n");
        return 1;
    }
    
    // Сохранение в битовые поля
    sensor.temperature = temp + 50;  // Смещение для хранения отрицательных значений
    sensor.humidity = hum;
    
    // Чтение и вывод
    printf("\nСохраненные данные:\n");
    printf("Температура: %d°C\n", get_temperature_value(sensor));
    printf("Влажность: %d%%\n", get_humidity_value(sensor));
    
    // Демонстрация экономии памяти
    struct RegularData {
        int temperature;
        int humidity;
    };
    
    printf("\nСравнение размеров:\n");
    printf("Обычная структура: %zu байт\n", sizeof(struct RegularData));
    printf("С битовыми полями: %zu байт\n", sizeof(struct SensorData));
    printf("Экономия: %zu байт\n", sizeof(struct RegularData) - sizeof(struct SensorData));
    
    return 0;
}
```

### Задача 8 - ввод и вывод через enum и union

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

enum InputType { INTEGER, FLOAT_POINT, TEXT };

union InputData {
    int int_value;
    float float_value;
    char text_value[100];
};

struct InputHandler {
    enum InputType type;
    union InputData data;
};

void print_data(struct InputHandler *handler) {
    switch (handler->type) {
        case INTEGER:
            printf("Целое число: %d\n", handler->data.int_value);
            break;
        case FLOAT_POINT:
            printf("Дробное число: %.2f\n", handler->data.float_value);
            break;
        case TEXT:
            printf("Текст: %s\n", handler->data.text_value);
            break;
    }
}

int main_task8() {
    printf("\n=== Задача 8 - Ввод и вывод через enum и union ===\n");
    
    struct InputHandler handler;
    int choice;
    
    printf("Выберите тип ввода:\n");
    printf("1 - Целое число\n");
    printf("2 - Дробное число\n");
    printf("3 - Текст\n");
    printf("Ваш выбор: ");
    scanf("%d", &choice);
    getchar(); // Очистка буфера
    
    switch (choice) {
        case 1:
            handler.type = INTEGER;
            printf("Введите целое число: ");
            scanf("%d", &handler.data.int_value);
            break;
            
        case 2:
            handler.type = FLOAT_POINT;
            printf("Введите дробное число: ");
            scanf("%f", &handler.data.float_value);
            break;
            
        case 3:
            handler.type = TEXT;
            printf("Введите текст: ");
            fgets(handler.data.text_value, 100, stdin);
            handler.data.text_value[strcspn(handler.data.text_value, "\n")] = 0;
            break;
            
        default:
            printf("Неверный выбор!\n");
            return 1;
    }
    
    printf("\nВведенные данные:\n");
    print_data(&handler);
    
    // Демонстрация размера
    printf("\nРазмер структуры InputHandler: %zu байт\n", sizeof(struct InputHandler));
    printf("Размер объединения InputData: %zu байт\n", sizeof(union InputData));
    
    return 0;
}
```

## 6. Результаты выполненной работы

![](https://github.com/artbutilkin/Lab-work-3/blob/main/Pasted%20image%2020251001114503.png)
![](https://github.com/artbutilkin/Lab-work-3/blob/main/Pasted%20image%2020251001114518.png)
![](https://github.com/artbutilkin/Lab-work-3/blob/main/Pasted%20image%2020251001114541.png)
![](https://github.com/artbutilkin/Lab-work-3/blob/main/Pasted%20image%2020251001114618.png)
![](https://github.com/artbutilkin/Lab-work-3/blob/main/Pasted%20image%2020251001114641.png)

## 7. Данные о студенте.

Сибель Максим, 1 курс, ПОО

