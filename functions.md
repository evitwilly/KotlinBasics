# 3. Функции в Kotlin'е

<img alt="Радуга Дэш" src="https://i.playground.ru/i/pix/817167/image.jpg" width="60%" />

Я думаю, вы уже знакомы с функциями <code>print</code> и <code>println</code>, которые позволяют вывести результат или строку.

Функции нужны, чтобы структурировать код и повторно его использовать.

Давайте создадим свою функцию, которая будем выводить символы строки через пробел:

    // новая функция
    fun printSymbols(str: String) { 
        for (symbol in str) { // мы уже рассматривали цикл for, его также можно использовать, чтобы пройтись посимвольно по строке
            print("$symbol ") // после каждого символа добавляем пробел
        }
    }

    fun main() {
        printSymbols("Hello!")
    }
    
Вуаля! Мы написали функцию!

Остановимся немного подробнее.

После ключевого слова <code>fun</code> мы пишем название функции, в круглых скобках объявляем её параметры, а в фигурных пишем саму функцию.

Обратите внимание, что после названия параметра через знак <code>:</code> мы пишем его тип.

Небольшая справка о типах (мы еще вернёмся к типам):

1. **Int** - целое число
2. **String** - строка

Идем дальше. А что если мы хотим, чтобы у нашей функции было значение по умолчанию?

В **Kotlin'е** это легко сделать:

    fun printSymbols(str: String = "Hello!") { // параметр по умолчанию
        for (symbol in str) {
            print("$symbol ")
        }
    }

    fun main() {
        printSymbols() // теперь мы можем не передать параметр, будет использован по умолчанию
    }

Довольно просто, не так ли?

Наша функция не возвращает никакого результа, давайте напишем функцию, которая будет находить факториал числа:

    fun factorial(number: Int) : Int {
        var result = 1
        for (index in 2..number) {
            result *= index
        }
        return result
    }

    fun main() {
        print(factorial(10)) // 3628800
    }

Функция <code>factorial</code> принимает на вход число и возвращает результат, 
тип которого мы прописываем после двоеточия и параметров, которая она принимает.

Вернемся к первой функции, которая ничего не возвращает, но на самом деле это не так.

Полная запись:

    fun printSymbols(str: String = "Hello!"): Unit {
        for (symbol in str) {
            print("$symbol ")
        }
    }
    
<code>Unit</code> означает ничего и не представляет никакого значения.

Если ваша функция ничего не возвращает, вам не нужно дописывать <code>Unit</code>.

Закрепим знания, напишем более сложную функцию:

    // функция, которая вычисляет квадратное уравнение
    // принимает на вход коэффициенты, возвращает строку с результатом
    fun calculateQuadraticEquation(a: Int, b: Int, c: Int) : String {
        val discriminant = b * b - 4 * a * c
        // when похож на if, только более лаконичный и применяется где
        // больше двух веток выбора
        // обратите внимание мы пишем один раз return, что тоже является прелестью Kotlin языка
        return when {
            discriminant < 0 -> {
                "Нет корня"
            }
            discriminant == 0 -> {
                val x0 = -b / (2 * a)
                "Один корень: $x0"
            }
            else -> {
                // sqrt - встроенная функция из kotlin.math 
                // toDouble() приводит целое число к дробному, чтобы вычислить корень
                val rootOfDiscriminant = sqrt(discriminant.toDouble())
                val x1 = (-b + rootOfDiscriminant) / (2 * a)
                val x2 = (-b - rootOfDiscriminant) / (2 * a)
                "Первый корень: $x1, второй корень: $x2"
            }
        }
    }

    fun main() {
        // вычисляем квадратное уравнения для a = -1, b = 7, c = 8
        print(calculateQuadraticEquation(-1, 7, 8)) // Первый корень: -1.0, второй корень: 8.0
    }

Попробуйте в интернете поискать примеры квадратных уравнений и сравнить результаты нашей программы с правильными.

Впоследствии мы узнаем больше о функциях и фишках **Kotlin'а** и с практикой у вас все получится!

**Желаю хорошего кода и счастья конечно же)))**