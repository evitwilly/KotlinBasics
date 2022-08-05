# 1. Ковариантность, контравариантность и инвариантность в Kotlin'е

Приступим к более сложным темам.

Начнем с простого примера:

    abstract class Thing

    data class Coin(val cost: Double) : Thing()

    data class Book(val name: String) : Thing()

    class Box<T>(
        private val one: T,
        private val two: T,
        private val three: T
    ) {
        fun show() {
            print("$one $two $three")
        }
    }

    fun main() {
        val box = Box(Coin(10.5), Coin(20.5), Coin(30.5))
        box.show()
    }

В данном примере я создал небольшую иерархию: класс <code>Thing</code> является суперклассом для <code>Coin</code> и <code>Book</code>

Затем я написал обобщенный класс <code>Box</code>, который может хранить три вещи.

В методе <code>main</code> создается объект класса <code>Box</code> с тремя монетами.

Теперь самое интересное попробуем сделать так:

    fun main() {
        val coinBox = Box(Coin(10.5), Coin(20.5), Coin(30.5))
        val thingBox : Box<Thing> = coinBox // ошибка приведения типа
        thingBox.show()
    }
    
По логике тут не должно быть ошибок, т.к. <code>Thing</code> суперкласс для <code>Coin</code>
 
Но увы компилятор запрещает так делать и он будет прав. Почему?
  
Давайте немного поменяем класс <code>Box</code>:
  
    class Box<T>(
        var one: T,
        private val two: T,
        private val three: T
    ) {
        fun show() {
            print("$one $two $three")
        }
    }

    fun main() {
        val coinBox = Box(Coin(10.5), Coin(20.5), Coin(30.5))
        val thingBox : Box<Thing> = coinBox // 1
        thingBox.one = Book("The Master and Margarita") // 2
        val coin : Coin = thingBox.one // 3
    }
    
Представим что Kotlin разрешил присвоение на строке 1, тогда мы можем положить туда книгу (строка 2)

Теперь мы пытаемся обратиться к первой вещи, как к монете, в результате получаем ошибку во время выполнения! (строка 3)

Это довольно серьезная ошибка, которую гораздо сложнее исправить в программе нежели на этапе компиляции и поэтому Kotlin 
запрещает такое присваивание.

Поздравляю! Вы уже рассмотрели инвариантность - точное соответствие обобщенных типов при присваивании.

Ну а что если нам все же нужно выполнить присваивание <code>val thingBox: Box<Thing> = coinBox</code>
  
Тогда идем дальше!
  
### Ковариантность
  
Поменяем немного наш класс:
  
    class Box<out T>(
        private val one: T,
        private val two: T,
        private val three: T
    ) {
        fun show() {
            print("$one $two $three")
        }
    }

    fun main() {
        val coinBox = Box(Coin(10.5), Coin(20.5), Coin(30.5))
        val thingBox : Box<Thing> = coinBox
        thingBox.show()
    }

Вуаля! С помощью ключевого слова <code>out</code> мы говорим компилятору что наш обобщенный тип будет производителем, то есть наш класс
<code>Box</code> не позволит менять переменные и свойства обобщенного типа и поэтому никогда не произойдет следующий ошибки:
  
      fun main() {
          val coinBox = Box(Coin(10.5), Coin(20.5), Coin(30.5))
          val thingBox : Box<Thing> = coinBox // все ок
          thingBox.one = Book("The Master and Margarita") // компилятор будет ругаться на вас за такой код!
          val coin : Coin = thingBox.one // даже не думайте, что доберетесь до сюда :)
      }
  
Помимо своим типов, ковариантными являются неизменяемые списки в Kotlin'е:
  
    fun main() {
        val ints = listOf(1, 2, 3)
        val numbers: List<Number> = ints
    }
 
