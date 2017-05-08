# EquationMath
[![Build status](https://ci.appveyor.com/api/projects/status/phreo8v5ku8e5h1u?svg=true&retina=true)](https://ci.appveyor.com/project/Pzixel/equationmath)

Реализация следующего тестового задания:

Напишите программу на C#, которая приводит уравнение к каноническому виду: приводит подобные слагаемые и приравнивает сумму к нулю.
 
Уравнение может быть любого порядка, может содержать любое количество переменных, может быть записано со скобками (в этом случае приложение должно раскрыть скобки).
 
Уравнение на вход будет передано в виде строки в следующем формате:
P1 + P2 + ... = ... + PN
где P1..PN - слагаемые, представленные в виде:
ax^k
где a - число с плавающей точкой;
k - целое число;
x - переменная (переменных у одного слагаемого может быть несколько).
 
Например, может быть дано уравнение следующего вида:
x^2 + 3.5xy + y = y^2 - xy + y
Оно должно быть приведено к виду:
x^2 - y^2 + 4.5xy = 0
 
Программа должна быть оформлена как консольное приложение Visual Studio и поддерживать два режима работы – интерактивный и файловый. В интерактивном режиме программа предлагает ввести выражение и выводит результат по окончании ввода. После этого приглашение ввести выражение повторяется. Завершить работу можно с помощью сочетания Ctrl-C. В файловом режиме программа принимает в качестве параметра имя файла содержащего строчки с выражениями и записывает результат работы в новый файл с добавлением расширения .out.

# Ограничения текущей версии

Для простоты реализации некоторые случаи специально не были учтены:
1. Порядок следования аргументов нормализованного уравнения не регламентирован.
2. Большинство исключений жестко прокидываются до пользователя и ни во что не оборачиваются
3. Т.к. в задании был указан вид слагаемых, но при этом был пункт про "могут быть скобки", то исходил из того, что скобки могут использоваться только между аргументами, то есть исключительно менять знак операции. Для поддержки умножения скобки на произвольный коэффициент необходимо усовершенствовать `ParenthesisRemover`.
4. `PolynomeNormalizer` не принимает компоненты как аргументы (зависимости не внедряются, а прописаны жестко внутри этого класса). Это опять же сделано специально, чтобы не создавать для всех элементов пайплайна по интерфейсу, что удвоило бы количество файлов в модели. При необходимости это все легко расширяется и добавляется.
5. Изначально можно было довольно легко сделать задание при помощи регулярных выражений, единственной проблемой были бы скобки, но тогда любое изменение в стиле пункта 3 было бы невозможно, в текущей реализации - в этом нет ничего сложного. В реальной задаче для парсинга лучше было бы взять любой готовый парсер, который работает на основе задания грамматики.
6. В текущей версии `x^2y^2`, `y^2x^2` и `xxyy` являются тремя разными термами. Для того, чтобы нормализовывать и эти случаи, необходимо дописать немного логики в `Reducer`.
7. Возможно, разделение на такое количество классов - оверинжинеринг, но это позволило локализовать проблемы тестами и решать их независимо друг от друга.
