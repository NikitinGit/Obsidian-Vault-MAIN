1.  В основе лежат промежуточные операции и терминальные (после которых промежуточные не могут быть вызваны, так же как терминальные не могут быть вызваны)
2. Промежуточных операций может быть несколько - терминальная операция одна
3. Использует отложение выполнение лямбда выражений - то есть при вызове терминальной операции 
4. Во всех коллекциях начиная с jdk 8 есть метод stream через который можно получать объект Stream<T> , та кже его можно получить через Arrays.stream(T[] array), Stream.of("Nikitin", "Bin") , InStream.of() , LongStream.of(), DoubleStream.of()




https://metanit.com/java/tutorial/10.1.php 