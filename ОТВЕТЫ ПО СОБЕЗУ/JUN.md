1. JDK - набор разработчика содержит JRE и инструменты разраба, JRE - среда исполнения программы - достаточно для запуска проги - содержит jvm и необходимые компоненты/библиотеки, JVM ^jdk
2. Java - язык программирования 
3. Что такое bytecode - это набор инструкций для выполнения jvm, не зависит от ОС - промежуточный код ^bytecode
4. JIT - just in time компиляция байткода при запуске программы - его горячих мест (hot spots) - которые часто используются 
5. byte, short, long, float, int, char, double, boolean ^types
6. JVM - виртуальная машина , которая интерпретирует байт-код, с помощью JIT компилирует его в машинный код. Jit анализирует код и находит в нем горячие участки кода - которые чаще всего исполняются и компилирует их в машинный код ускоряя выполнение программы. 
   Существует 2 вида JIT -  C1 (3 уровня компиляции - флаг -client) и  C2 (4-ый уровень компиляции - флаг -server). Клиентский быстрее запускает но работает медленнее. 
   На ОС x64  флаг -server отсутствует ^jvm
