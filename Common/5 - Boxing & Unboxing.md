# Boxing/Unboxing

# Упаковка-преобразование и распаковка-преобразование 

Упаковка представляет собой процесс преобразования типа значения в тип object или в любой другой тип интерфейса, реализуемый этим типом значения. Когда тип значения упаковывается средой CLR, он инкапсулирует значение внутри экземпляра System.Object и сохраняет его в управляемой куче. Операция распаковки извлекает тип значения из объекта. Упаковка является неявной; распаковка является явной. Понятия упаковки и распаковки лежат в основе единой системы типов C#, в которой значение любого типа можно рассматривать как объект.
В следующем примере выполнена операция iупаковки **целочисленной переменной*, которая присвоена объекту o.

```csharp
int i = 123;
// The following line boxes i.
object o = i;  
```

Затем можно выполнить операцию распаковки объекта oи присвоить его целочисленной переменной _i_:

```csharp
o = 123;
i = (int)o;  // unboxing
```
Следующий пример иллюстрирует использование упаковки в C#.

```csharp
// String.Concat example.
// String.Concat has many versions. Rest the mouse pointer on 
// Concat in the following statement to verify that the version
// that is used here takes three object arguments. Both 42 and
// true must be boxed.
Console.WriteLine(String.Concat("Answer", 42, true));

// List example.
// Create a list of objects to hold a heterogeneous collection 
// of elements.
List<object> mixedList = new List<object>();

// Add a string element to the list. 
mixedList.Add("First Group:");

// Add some integers to the list. 
for (int j = 1; j < 5; j++)
{
    // Rest the mouse pointer over j to verify that you are adding
    // an int to a list of objects. Each element j is boxed when 
    // you add j to mixedList.
    mixedList.Add(j);
}

// Add another string and more integers.
mixedList.Add("Second Group:");
for (int j = 5; j < 10; j++)
{
    mixedList.Add(j);
}

// Display the elements in the list. Declare the loop variable by 
// using var, so that the compiler assigns its type.
foreach (var item in mixedList)
{
    // Rest the mouse pointer over item to verify that the elements
    // of mixedList are objects.
    Console.WriteLine(item);
}

// The following loop sums the squares of the first group of boxed
// integers in mixedList. The list elements are objects, and cannot
// be multiplied or added to the sum until they are unboxed. The
// unboxing must be done explicitly.
var sum = 0;
for (var j = 1; j < 5; j++)
{
    // The following statement causes a compiler error: Operator 
    // '*' cannot be applied to operands of type 'object' and
    // 'object'. 
    //sum += mixedList[j] * mixedList[j]);

    // After the list elements are unboxed, the computation does 
    // not cause a compiler error.
    sum += (int)mixedList[j] * (int)mixedList[j];
}

// The sum displayed is 30, the sum of 1 + 4 + 9 + 16.
Console.WriteLine("Sum: " + sum);

// Output:
// Answer42True
// First Group:
// 1
// 2
// 3
// 4
// Second Group:
// 5
// 6
// 7
// 8
// 9
// Sum: 30
```

## Производительность
По сравнению с простыми операциями присваивания операции упаковки и распаковки являются весьма затратными процессами с точки зрения вычислений. При выполнении упаковки типа значения необходимо создать и разместить новый объект. Объем вычислений при выполнении операции распаковки, хотя и в меньшей степени, но тоже весьма значителен. Дополнительные сведения см. в разделе Производительность.
## Упаковка
Упаковка используется для хранения типов значений в куче со сбором мусора. Упаковка представляет собой неявное преобразование типа значения в тип object или в любой другой тип интерфейса, реализуемый этим типом значения. При упаковке типа значения в куче выделяется экземпляр объекта и выполняется копирование значения в этот новый объект.
Рассмотрим следующее объявление переменной типа значения.
```csharp
int i = 123;
```

Следующий оператор неявно применяет операцию упаковки к переменной *i*.

```csharp
// Boxing copies the value of i into object o.
object o = i;  
```

Результат этого оператора создает ссылку на объект o в стеке, которая ссылается на значение типа int в куче. Это значение является копией значения типа значения, присвоенного переменной *i*. Разница между этими двумя переменными, *i*и o, показана на рисунке упаковки-преобразования ниже:

![](https://docs.microsoft.com/ru-ru/dotnet/csharp/programming-guide/types/media/boxing-and-unboxing/boxing-operation-i-o-variables.gif)

Можно также выполнять упаковку явным образом, как в следующем примере, однако явная упаковка не является обязательной.

```csharp
int i = 123;
object o = (object)i;  // explicit boxing
```

## ОПИСАНИЕ
В этом примере целочисленная переменная *i* преобразуется в объект o при помощи упаковки. Затем значение, хранимое переменной i, меняется с 123 на 456. В примере показано, что исходный тип значения и упакованный объект используют отдельные ячейки памяти, а значит могут хранить разные значения.
## Пример

```csharp
class TestBoxing
{
    static void Main()
    {
        int i = 123;

        // Boxing copies the value of i into object o.
        object o = i;  

        // Change the value of i.
        i = 456;  

        // The change in i doesn't affect the value stored in o.
        System.Console.WriteLine("The value-type value = {0}", i);
        System.Console.WriteLine("The object-type value = {0}", o);
    }
}
/* Output:
    The value-type value = 456
    The object-type value = 123
*/
```

## Распаковка
Распаковка является явным преобразованием из типа *object* в тип значения или из типа интерфейса в тип значения, реализующего этот интерфейс. Операция распаковки состоит из следующих действий:

- проверка экземпляра объекта на то, что он является упакованным значением заданного типа значения;

- копирование значения из экземпляра в переменную типа значения.

В следующем коде показаны операции по упаковке и распаковке.

```csharp
int i = 123;      // a value type
object o = i;     // boxing
int j = (int)o;   // unboxing
```

На рисунке ниже представлен результат выполнения этого кода.

![](https://docs.microsoft.com/ru-ru/dotnet/csharp/programming-guide/types/media/boxing-and-unboxing/unboxing-conversion-operation.gif)

Для успешной распаковки типов значений во время выполнения необходимо, чтобы экземпляр, который распаковывается, был ссылкой на объект, предварительно созданный с помощью упаковки экземпляра этого типа значения. Попытка распаковать null создает исключение NullReferenceException. Попытка распаковать ссылку на несовместимый тип значения создает исключение InvalidCastException.
Пример
В следующем примере показан случай недопустимой распаковки, в результате чего создается исключение InvalidCastException. В случае использования try и catch при возникновении ошибки выводится сообщение.

```csharp
class TestUnboxing
{
    static void Main()
    {
        int i = 123;
        object o = i;  // implicit boxing

        try
        {
            int j = (short)o;  // attempt to unbox

            System.Console.WriteLine("Unboxing OK.");
        }
        catch (System.InvalidCastException e)
        {
            System.Console.WriteLine("{0} Error: Incorrect unboxing.", e.Message);
        }
    }
}
```
При выполнении этой программы выводится следующий результат:
*Specified cast is not valid. Error: Incorrect unboxing.*

При изменении оператора:

```csharp
int j = (short) o;  
```

на:

```csharp
int j = (int) o; 
```

будет выполнено преобразование со следующим результатом:
*Unboxing OK*.
