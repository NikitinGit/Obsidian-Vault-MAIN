1. Для каждого потока создается свой стэк вызовов ?
2. Thread создается jvm но управляется опреционнгой системой ? - Да, но жизненый цикл потока контролируется как джвм так и ос
3. Поток запускается только после start"()  и может быть выбран планировщиком потоков после этого
4. жизненный цикл состояний потока - создан, готов к исполнению (после старта) , ожмдание, блокировка, терминате - прерывание ```java
public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            System.out.println("Мы находимся в методе 'run'.");
        });

        System.out.println("Состояние потока после создания: " + thread.getState());  // NEW

        thread.start();
        System.out.println("Состояние потока после вызова start(): " + thread.getState());  // RUNNABLE

        // Дадим потоку время для завершения
        try {
            Thread.sleep(100);  // Переход основного потока в TIMED_WAITING
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Состояние потока после завершения: " + thread.getState());  // TERMINATED
    }


5. Демон поток - поток , который работает в фоновом режиме и завершается после завершения работы всех остальных потоков (как говорят при завергшение программы). Сделать поток демоном - setDeamon(true) .
6. Приоритет потока указывает на то , сколько времени процессора будет отдано потоку относительно других. Управялется ДЖВМ и ОС. При создание потока он наследует приритет создателя потока. При большом количестве максимальных приоритетов потоков может снизиться производительность - ДЖВМ БУДЕТ ТРАТИТЬ МНОГО ВРЕМЕНИ НА ВЫПОЛНЕНИЕ задач.
7. Прерывание потока делается с помощью метода interrupt - но он не останавливает сразу поток, а только утсанавливает флаг прерывания . Прерывание блокируюших опреациий вызывает исключение InterruptedException , прерывание не блокирущих не вызывает. 
8. Блокирующие операци wait, join,  sleep
9. Когда срабатывает исключение , то в методе catch флаг Thread.currentThread().isInterrupted становистя false
10. Чтобы запустить поток со своим именем можно вызвать new Thread(thread1, "Thread name; ").start(); 
11.  семафор - ограничитель доступа к общему ресурсу, устанавливает число потоков, которые могут с ним работать. Число разрешений указывается в конструкторе `new` `Semaphore(``1``);` , запрос на ресурс указывается через asquire , освобождение семафора через release 
12. Exchanger нужен для  обмена данными меж потоками - message=exchanger.exchange(message); метод exchange отправляет в буфер message и возвращает в овтете возвращает данные из другого потока - работает ток с двумя потоками и пренадлежит пакету потокобезопснах объектов - java.util.concurrent. exchange блокирует поток