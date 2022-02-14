[Вопросы для собеседования](README.md)

# Исключения
+ [Дайте определение понятию “исключение”](#дайте-определение-понятию-исключение)
+ [Какова иерархия исключений?](#какова-иерархия-исключений)
+ [Можно/нужно ли обрабатывать ошибки jvm?](#можнонужно-ли-обрабатывать-ошибки-jvm)
+ [Какие существуют способы обработки исключений?](#какие-существуют-способы-обработки-исключений)
+ [В чем особенность блока finally? Всегда ли он исполняется?](#в-чем-особенность-блока-finally-всегда-ли-он-исполняется)
+ [Может ли не быть ни одного блока catch при отлавливании исключений?](#может-ли-не-быть-ни-одного-блока-catch-при-отлавливании-исключений)
+ [Что вы знаете об обрабатываемых и не обрабатываемых (checked/unchecked) исключениях?](#что-вы-знаете-об-обрабатываемых-и-не-обрабатываемых-checkedunchecked-исключениях)
+ [Может ли метод main выбросить исключение вовне и если да, то где будет происходить обработка данного исключения?](#может-ли-метод-main-выбросить-исключение-вовне-и-если-да-то-где-будет-происходить-обработка-данного-исключения)
+ [Если оператор return содержится и в блоке catch и в finally, какой из них “главнее”?](#если-оператор-return-содержится-и-в-блоке-catch-и-в-finally-какой-из-них-главнее)
+ [Что вы знаете о SQLException? К какому типу checked или unchecked оно относится, почему?](#что-вы-знаете-о-sqlexception-к-какому-типу-checked-или-unchecked-оно-относится-почему)
+ [Что такое Error? В каком случае используется Error. Приведите пример Error’а.](#что-такое-error-в-каком-случае-используется-error-приведите-пример-errorа)
+ [Предположим, есть блок try-finally. В блоке try возникло исключение и выполнение переместилось в блок finally. В блоке finally тоже возникло исключение. Какое из двух исключений “выпадет” из блока try-finally? Что случится со вторым исключением?](#предположим-есть-блок-try-finally-в-блоке-try-возникло-исключение-и-выполнение-переместилось-в-блок-finally-в-блоке-finally-тоже-возникло-исключение-какое-из-двух-исключений-выпадет-из-блока-try-finally-что-случится-со-вторым-исключением)

## Дайте определение понятию “исключение”
Исключение — это проблема(ошибка) возникающая во время выполнения программы. Исключения могут возникать во многих случаях, например:
Пользователь ввел некорректные данные.
Файл, к которому обращается программа, не найден.
Сетевое соединение с сервером было утеряно во время передачи данных. И т.д.
Все исключения в Java являются объектами. Поэтому они могут порождаться не только автоматически при возникновении исключительной ситуации, но и создаваться самим разработчиком.

[к оглавлению](#исключения)

## Какова иерархия исключений?
Исключения делятся на несколько классов, но все они имеют общего предка — класс Throwable. Его потомками являются подклассы Exception и Error.
Исключения (Exceptions) являются результатом проблем в программе, которые в принципе решаемые и предсказуемые. Например, произошло деление на ноль в целых числах.
Ошибки (Errors) представляют собой более серьёзные проблемы, которые, согласно спецификации Java, не следует пытаться обрабатывать в собственной программе, поскольку они связаны с проблемами уровня JVM. Например, исключения такого рода возникают, если закончилась память, доступная виртуальной машине. Программа дополнительную память всё равно не сможет обеспечить для JVM.
В Java все исключения делятся на два типа: контролируемые исключения (checked) и неконтролируемые исключения (unchecked), к которым относятся ошибки (Errors) и исключения времени выполнения (RuntimeExceptions, потомок класса Exception).
Контролируемые исключения представляют собой ошибки, которые можно и нужно обрабатывать в программе, к этому типу относятся все потомки класса Exception (но не RuntimeException).

[к оглавлению](#исключения)

## Можно/нужно ли обрабатывать ошибки jvm?
Обрабатывать можно, но делать этого не стоит. Разработчику не предоставлены инструменты для обработки ошибок системы и виртуальной машины.

[к оглавлению](#исключения)

## Какие существуют способы обработки исключений?
В Java есть пять ключевых слов для работы с исключениями:
try — данное ключевое слово используется для отметки начала блока кода, который потенциально может привести к ошибке.
catch — ключевое слово для отметки начала блока кода, предназначенного для перехвата и обработки исключений.
finally — ключевое слово для отметки начала блока кода, которой является дополнительным. Этот блок помещается после последнего блока ‘catch’. Управление обычно передаётся в блок ‘finally’ в любом случае.
throw — служит для генерации исключений.
throws — ключевое слово, которое прописывается в сигнатуре метода, и обозначающее что метод потенциально может выбросить исключение с указанным типом.
Общий вид конструкции для «поимки» исключительной ситуации выглядит следующим образом:

try{ 
//здесь код, который потенциально может привести к ошибке 
} 
catch(SomeException e ){ //в скобках указывается класс конкретной ожидаемой ошибки  
//здесь описываются действия, направленные на обработку исключений 
} 
finally{ 
//выполняется в любом случае ( блок finally  не обязателен) 
}

[к оглавлению](#исключения)

## В чем особенность блока finally? Всегда ли он исполняется?
Когда исключение передано, выполнение метода направляется по нелинейному пути. Это может стать источником проблем. Например, при входе метод открывает файл и закрывает при выходе. Чтобы закрытие файла не было пропущено из-за обработки исключения, был предложен механизм finally.
Ключевое слово finally создаёт блок кода, который будет выполнен после завершения блока try/catch, но перед кодом, следующим за ним. Блок будет выполнен, независимо от того, передано исключение или нет. Оператор finally не обязателен, однако каждый оператор try требует наличия либо catch, либо finally. Код в блоке finally будет выполнен всегда, исключение - вызов в блоке catch System.exit() или другое прекращение работы JVM

[к оглавлению](#исключения)

## Может ли не быть ни одного блока catch при отлавливании исключений?
Такая запись допустима, если имеется связка try{} finally {}. Но смысла в такой записи не так много, всё же лучше иметь блок catch в котором будет обрабатываться необходимое исключение.

String x = "z";
try {
   x="234";
} finally {
    x = "Finally";
}

[к оглавлению](#исключения)

## Что вы знаете об обрабатываемых и не обрабатываемых (checked/unchecked) исключениях?
 - Throwable и Exception и все их наследники (за исключением наследников Error-а и RuntimeException-а) — checked
 - Error и RuntimeException и все их наследники — unchecked

1. Checked исключения, это те, которые должны обрабатываться блоком catch или описываться в сигнатуре метода. Unchecked могут не обрабатываться и не быть описанными.
Наличие\обработка Checked исключения проверяются на этапе компиляции.
2. Unchecked исключения в Java — наследованные от RuntimeException, checked — от Exception (не включая unchecked).
Наличие\обработка Unchecked исключения происходит на этапе выполнения.

[к оглавлению](#исключения)

## Может ли метод main выбросить исключение вовне и если да, то где будет происходить обработка данного исключения?
Может и оно будет передано в виртуальную машину Java (JVM).

[к оглавлению](#исключения)

## Если оператор return содержится и в блоке catch и в finally, какой из них “главнее”?
Вернется из блока finally.

    public static void main(String[] args) {
       String what =  method();
        System.out.println(what);
    }

    public static String method() {
        try {
            return "SomeString";
        } finally {
            return "Finally message";
        }
    }
//Вывод
Finally message

[к оглавлению](#исключения)

## Что вы знаете о SQLException? К какому типу checked или unchecked оно относится, почему?
SQLException предоставляет информацию об ошибках доступа к базе данных или других ошибках связанных с работой с базами данных.
SQLException относится к checked исключениям, а значит проверяется на этапе компиляции.
Споры об этом типе исключения идут о том, что разработчику приходится постоянно обрабатывать это исключение в коде, хотя большая часть ошибок возникает во время выполнения программы, т.е., по мнению многих, лучше бы отнести его к unchecked runtime исключениям.

try {
    // make some SQL call(s)
} catch {SQLException e) { 
    // log the exception
    return; // and give up
}
Аргумент Joshua Bloch из Effective Java Second Edition такой: сделав SQLException проверяемым — это попытка заставить разработчиков обработать исключение и обернуть его в новом уровне абстракции.

[к оглавлению](#исключения)

## Что такое Error? В каком случае используется Error. Приведите пример Error’а.
Ошибки (Errors) представляют собой более серьёзные проблемы, которые, согласно спецификации Java, не следует пытаться обрабатывать в собственной программе, поскольку они связаны с проблемами уровня JVM. Например, исключения такого рода возникают, если закончилась память, доступная виртуальной машине.

[к оглавлению](#исключения)


## Предположим, есть блок try-finally. В блоке try возникло исключение и выполнение переместилось в блок finally. В блоке finally тоже возникло исключение. Какое из двух исключений “выпадет” из блока try-finally? Что случится со вторым исключением?
Ответ аналогичный случаю с двумя return — будет обработано в finally блоке. Если было выброшено два исключения — одно в try, второе в finally, то исключение в finally «проглотит» исключение выше (см. пример). Если до блока finally исключение было обработано, то мы можем получить информацию об исключении в блоке try и тем самым не потерять исключение, которое впоследствии может быть перезаписано в finally другим исключением.

public class TestExc {

    public static void main(String[] args) {
        Exception ex = twoExceptionsMethod();
        System.out.println(ex.getClass());

        String s = twoExceptionsMethod2();
        System.out.println(s);
    }

    public static Exception twoExceptionsMethod() {
        try {
            return new IndexOutOfBoundsException();
        } finally {
            return new NullPointerException();
        }
    }

    public static String twoExceptionsMethod2() {
        try {
            throw new NullPointerException();
        }catch (NullPointerException ex) {
            System.out.println(ex.getMessage()+ " catchBlock");;
        }
        finally {
            Exception ex2 = new Exception();
            return ex2.getMessage() + "finallyBlock";
        }
    }
}
//Вывод
class java.lang.NullPointerException
null catchBlock
null finallyBlock

[к оглавлению](#исключения)

[Вопросы для собеседования](README.md)