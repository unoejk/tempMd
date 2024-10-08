По теории основа это реакт: хуки все кейсы использования прям от зубов чтобы отскакивало, рендеринг, мемоизаиция все о
ней, о реконсиляции подробно; джс: все о асинхронщине подробно и с примерами, замыкание, протипы и this все подробно,
парлигмы програмирования, о каррирование, полиморфизме,;по тс основы;немного о тестирование с примерами, ci/cd; по вебу:
solid каждую букву и другие принципы тоже, как рендериться от запроса в боаузере до отрисовки, это пожалуй то что я
вспомнил

---

# webCore

---

## Процесс загрузки

https://vc.ru/selectel/76371-chto-proishodit-kogda-polzovatel-nabiraet-v-brauzere-adres-saita

- Вы вводите адрес
- Браузер ищет в своем кэше запись о DNS. Он проверяет 4 кэша: браузера, операционной системы, роутера, провайдера
- DNS запрос ищет нужный IP на разных DNS серверах, сперва в Local DNS resolver, который поставляется провайдером. Он в
  свою очередь сначала идет к Root Server, который дает ему TLD Top Level Domain(например .ru) сервер и так по цепочке.
- Браузер инициализирует TCP соединение с сервером
- Браузер посылает HTTP запрос на получение нужной страницы
- Сервер обрабатывает запрос и высылает ответ. Вместе с этим высылает куки файл, которым ПК и сервер обмениваются при
  каждом запросе.
- Рендерится HTML разметка
- Проверяются HTML теги и отсылаются GET запросы на получение дополнительных элементов. Файлы кэшируются
- Отображается страница в браузере

---

## Критические этапы рендеринга

- **Парсинг HTML и создание DOM.**
- **DOM** ответы в виде HTML превращаются в токены, которые в свою очередь превращаются в узлы и в последующем формируют
  DOM дерево.
- **CSSOM** все данные о том как стилизовать DOM.
- **JavaScript** загрузка всех скриптов.
- **Accessibility Tree** при парсинге HTML, анализируются специальные атрибуты по типу role и aria.
- **Render Tree** Соединение DOM и CSSOM.
- **Layout/Reflow** - это процесс определения размеров и позиций всех элементов на странице, а также их отношений друг к
  другу. Это происходит на основе CSS-правил и содержимого страницы.
- **Paint/Repaint** это процесс рендеринга элементов на странице. Он включает в себя применение цветов, текстур,
  градиентов и других стилей к элементам.
- **Compositing**  техника разделения частей страницы на слои, их раздельной отрисовки и компоновки в виде страницы в
  отдельном потоке, называемом потоком композитора. Когда части документа рисуются в разных слоях, накладываясь друг на
  друга, композитинг необходим для того, чтобы они выводились на экран в правильном порядке и содержимое отображалось
  корректно. При этом используются мощности GPU.

---

## SOLID

SOLID — это мнемоническая аббревиатура пяти принципов проектирования в объектно‑ориентированном программировании

- Single Responsobility - Принцип единственной ответственности, который подразумевает 1 сущность = 1 задача. Мы не
  делаем сущности, которые делают запросы, логируют что-то, сохраняют и т.д., это анти паттерн, такие сущности называют
  God object. В таком случае нужно декомпозировать сущность на несколько разных, которые выполняют одну задачу.
- Open-closed Principle - Принцип открытости/закрытости - Класс должен быть закрыт для изменения, но открыт для
  расширения. Пишем код так, чтобы другие могли легко расширить функционал, не меняя написанный (оттестированный,
  понравившийся твоему начальнику) код.
- Liskov Substitution Principle - Принцип подстановки Барбары Лисков - Если в коде программы Базовый класс заменить на
  его Наследника, то программа должна работать, так как в Наследнике есть все операции, которые были в Базовом. В
  Базовый класс нужно выносить только общую логику, которую наследники будут реализовывать. Наследников создаем только
  тогда, когда они правильно собираются реализовать логику Базового класса без проблем.
- Interface Segregation Principle - Принцип разделения интерфейса - Клиенты не должны зависеть от интерфейсов, которые
  они не используют. Большие интерфейсы следует разбивать на интерфейсы поменьше. Так клиенты смогут использовать только
  те интерфейсы, которые им нужны. Это делает менее связанный код, уменьшает зависимости между элементами системы,
  упрощает изменения в коде.
- Dependency Invertion Pronciple - Принцип инверсии зависимости - модули верхнего уровня не должны зависеть от
  модулей нижнего уровня. Оба должны зависеть от абстракций. Абстракции не должны зависеть от деталей. Детали должны
  зависеть от абстракций. Проще говоря, это означает, что вместо того, чтобы модуль верхнего уровня напрямую зависел от
  модулей нижнего уровня, оба модуля должны зависеть от общего интерфейса или абстракции. Таким образом, изменения в
  модуле нижнего уровня не должны затрагивать модуль верхнего уровня, а наоборот.

---

## Остальные принципы

- DRY/DIE - Don't Repeat Yourself / Duplication Is Evil.
- KISS Keep It Simple Stupid - призывает к тому, чтобы решения были максимально простыми. Он подразумевает, что чем
  проще и понятнее код, тем проще его поддерживать и изменять.
- YAGNI You Ain't Gonna Need It - не следует включать в программу функциональность, которая не требуется в данный
  момент. Он предостерегает от излишней сложности и избыточности кода.
- APO Avoid Premature Optimization - не следует оптимизировать код заранее, до того как станет ясно, что он
  действительно требует оптимизации. Ранняя оптимизация может привести к излишней сложности и утрате читаемости кода.

---

## Парадигмы программирования

https://doka-guide.vercel.app/tools/programming-paradigms/

----

## ООП

- Абстракция - представляет собой концепцию, которая позволяет скрыть детали реализации объекта и предоставить только
  необходимый набор данных и операций для работы с ним. Это как использование автомобиля без необходимости знать, как он
  работает внутри. Вам не нужно знать, как каждая деталь двигателя взаимодействует друг с другом; вам просто нужно
  знать, как управлять машиной с помощью руля и педалей. Абстракция в программировании позволяет скрыть сложные детали
  реализации, чтобы облегчить использование объекта или системы.
- Наследование - способность объекта/класса наследоваться от другого объекта/класса. Наследование позволяет создавать
  новые классы на основе существующих (родительских) классов. Подкласс (или дочерний класс) наследует свойства и методы
  родительского класса, а также может определять свои собственные свойства и методы. Это позволяет избегать повторного
  кодирования и обеспечивает повторное использование кода.
- Инкапсуляция - объединение данных и методов, которые работают с этими данными, в единый объект/класс. Объект скрывает
  свою внутреннюю реализацию от внешнего мира, предоставляя только определенные методы и свойства для взаимодействия,
  делается это с помощью геттеров и сеттеров, публичных и приватных полей в объекте/классе. Это позволяет обеспечить
  безопасность данных и упрощает их использование.
- Полиморфизм - позволяет объектам разных классов реагировать на вызовы методов с одинаковыми именами разным для каждого
  класса способом. Это делает код более гибким и легким для поддержки и расширения, поскольку различные объекты могут
  вести себя по-разному, но при этом использовать общий интерфейс для взаимодействия с ними.

---

## ФП

- Чистые функции (Pure Functions): Чистые функции - это функции, которые при одних и тех же входных данных всегда
  возвращают одинаковый результат и не имеют побочных эффектов, то есть они не изменяют состояние программы или внешние
  данные.
- Неизменяемость (Immutability): Данные в функциональном программировании обычно считаются неизменяемыми, что означает,
  что после создания структура данных не может быть изменена. Вместо этого операции над данными создают новые структуры
  данных.
- Функции высшего порядка (Higher-Order Functions): Функции высшего порядка - это функции, которые могут принимать
  другие функции в качестве аргументов или возвращать их в качестве результатов. Они позволяют создавать более
  абстрактные и гибкие функции.
- Рекурсия (Recursion): Рекурсия широко используется в функциональном программировании вместо циклов для выполнения
  итераций.
- Композиция функций (Function Composition): Композиция функций - это процесс комбинирования нескольких функций в одну,
  чтобы создать новую функцию.
- Преобразование данных (Data Transformation): Функциональное программирование поддерживает мощные средства для
  преобразования данных с помощью функций отображения, фильтрации и свертки.
- Строгая типизация (Strong Typing): Некоторые языки функционального программирования имеют строгую типизацию, которая
  обеспечивает большую надежность программ и упрощает рефакторинг.

---

## Тестирование CI/CD

- Continuous Integration (CI) — процесс автоматического сбора кода из разных источников (например, Git-репозиториев),
  выполнения тестов и сборки приложения. Это позволяет быстро выявлять возможные проблемы и обеспечивает стабильность
  продукта на всех этапах разработки.
- Continuous Deployment (CD) — процесс автоматического развертывания приложения на различных стадиях, включая тестовые,
  стейджинг и продакшен-среды. Цель CD — обеспечить максимально быстрое и безопасное развертывание новых версий продукта
  без необходимости ручного вмешательства.

Основная задача CI — сделать так, чтобы по мере параллельной работы над одной или разными кодовыми базами в конечном
счёте получилась версия приложения, которую можно доставить в окружение. Для этого каждый кусок кода должен быть
версионирован, протестирован и упакован в подходящий формат.

- unit-тесты — чтобы протестировать отдельные компоненты;
- интеграционные тесты — чтобы проверить интеграцию этих компонентов;
- базовый линтинг — чтобы проверить код на соответствие тому, как мы договорились писать его внутри команды;
- статистический код-анализ — чтобы отловить потенциальные уязвимости в нашем коде;
- smoke-тесты — чтобы проверить, развернулось ли приложение, и корректно ли работают его базовые компоненты.

Continuous delivery или непрерывная доставка — процесс, по окончании которого мы можем нажать на кнопку и доставить
новую версию приложения нашим клиентам.
В continuous deployment или непрерывном развёртывании кнопки и ручного шага у нас нет. Всё проходит автоматически. Но
поскольку выкатывать изменения клиентам в фоне без нашего участия сложно, непрерывное развёртывание возможно только при
наличии стратегии тестирования и системы мониторинга. Тогда если вдруг что-то пойдёт не так, мы узнаем об этом раньше
клиентов.

- E2E-тесты — чтобы проверить корректность работы UI;
- performance-тесты — чтобы проверить, нет ли просадок по производительности;
- регрессионные тесты — чтобы убедиться, что мы не вернулись к багам, исправленным в прошлом, перед выходом релиза на
  продакшн-среды;
- security-тесты, «ломающие» наше приложение — чтобы проверить, что у нас нет уязвимостей;
- penetration-тесты — чтобы проверить устойчивость приложения к взломам и отражению DDoS атак.

https://habr.com/ru/companies/slurm/articles/670422/
https://habr.com/ru/companies/ruvds/articles/676752/

----

# js

---

## Принцип асинхронности

событийный цикл: скрипт => одна макрозадача => все микрозадачи => рендеринг => заново к одной макрозадаче

Интерпритатор: то что идёт по коду js сверху вниз

- Call Stack (выполнялка):
  когда интерпритатор идёт по коду, он кидаёт все зачдачи в Call Stack где они сразу же исполняются
  если в Call Stack попадает что-то, чему надо отстояться (например, setTimeout), то оно улетает в Web Apis, вместо
  исполнения
- Web Apis (ожидалка):
  сюда попадает всё, чему надо отстояться (например: setTimeout, addEventListener, ...)
  setTimeout просто крутит свой таймер в Web Apis, а потом отпраляет вложеннй код в Callback Queue, после чего удаляется
  из Web Apis
  addEventListener таймера не имеет и остаётся в Web Apis жить, каждый раз когда будет происходить заложенный еvent,
  вложеннй код будет отправляться в Callback Queue
- Callback Queue (очередь в выполнялку):
  очередь, ожидающая когда Event Loop запустит их в Call Stack
- Event Loop (смотрящий за очередью в выполнялку):
  цикл, ожидающий когда Call Stack опустеет, после чего делает итерацию по Callback Queue
  и поочерёдно запускает нахоящиеся там команды в Call Stack

микрозадачи:

- операции с промисами (например, Promise.then(), Promise.catch(), fetch() и так далее);
- операции с очередью мутации (например, используемые в API MutationObserver для наблюдения за изменениями DOM);
- операции, связанные с queueMicrotask(), — функцией для явного добавления микрозадач.

макрозадачи:

- обработка таймеров (setTimeout, setInterval);
- обработка событий пользовательского ввода (например, клики, скроллинг);
- выполнение AJAX-запросов.

---

## Замыкание

Замыкание (Closure) - функция, которая запоминает своё окружение выполнения (лексическое окружение), даже когда функция
вызывается вне этого окружения.

---

## this, scope

Область видимости - это место откуда мы имеем доступ к переменным или функциям. Также область видимости это по сути
набор правил по которым ищется переменная. Сначала в локальной, затем во внешней и т.д. пока не дойдет до глобальной.

- Глобальная
- Функциональная/Локальная - переменные и функции объявленные внутри функции доступны только этой функции и всем
  вложенным в нее функциям.
- Блочная появилась в ES6 для переменных let и const, область видимости внутри фигурных скобок, так называемого блока.

Ключевое слово this в JavaScript используется для ссылки на объект, в котором выполняется код.

Контекст можно определить на основе трех вопросов:

- Включен строгий режим или нет?
- Какая функция используется?
- Как функция вызывается?

В функциях, созданных при помощи ключевого слова function:

- this определяется в момент вызова
- this можно задать явным образом через методы call, apply и bind

В стрелочных функциях:

- Нет своего this, они берут его из внешнего окружения в момент создания
- Нельзя присвоить this методами call, apply и bind
- НЕ теряют this
- В методе объекта, созданном через стрелочную функцию, this === window, ВСЕГДА! НЕ ВАЖНО КАКОЙ РЕЖИМ!

this у глобальной области видимости:

- Без 'use strict': this === window
- С 'use strict': this === undefined

this у методов объектов:

- равен объекту, но только в том случае, если мы вызываем метод через объект
- Если присвоить метод объекта в переменную и вызвать переменную как функцию, или передать метод в таймер, то он будет
  вызываться как обычная функция и this потеряется

this в конструкторах и классах:

- this === экземпляру объекта

----

## Прототипы объектов

let objectName=Object.create(Object.prototype,{
prop1:'text',
})

let objectName=Object.create(null)

Прототип - это объект, который используется в качестве шаблона для создания других объектов. Каждый объект в JavaScript
имеет ссылку на свой прототип, который определяет его свойства и методы. Если свойство или метод не найден в самом
объекте, JavaScript будет искать его в прототипе этого объекта, затем в прототипе прототипа и так далее, пока не будет
найдено или не достигнут конец цепочки прототипов (Object.prototype). Прототипы позволяют создавать иерархию объектов и
упрощать наследование свойств и методов.

Прототипное наследование - это особенность JavaScript, которая позволяет объектам наследовать свойства и методы других
объектов через использование их прототипов. В JavaScript каждый объект имеет ссылку на прототип, который является другим
объектом.

- .--proto-- - есть у всех объектов! Это ссылка на .prototype того класса, с помощью которого создан обьект.
- .prototype - есть только у function и class.
- .--proto-- объекта === .prototype его конструктора, так как он создан этим prototype

----

## Прототипы классов

class className2 extends className1{
propName:'text',
}

----

## Разница между классовым и прототипным наследованием

Классовое наследование:

- В классовом наследовании используется концепция классов и объектов, подобная той, что применяется в языках
  программирования, таких как Java или C++.
- Наследование происходит путем создания подкласса от родительского класса с использованием ключевого слова extends.
- Все методы и свойства родительского класса копируются в подкласс, который может быть изменен или дополнен.

Прототипное наследование:

- В прототипном наследовании используется концепция прототипов, которая уникальна для JavaScript.
- Все объекты в JavaScript имеют ссылку на прототип, который может быть использован для наследования свойств и методов.
- Наследование происходит путем создания нового объекта с использованием существующего объекта в качестве прототипа с
  помощью метода Object.create() или изменения прототипа объекта напрямую с помощью свойства --proto--.
- Подклассы могут наследовать свойства и методы от своих прототипов и расширять их, добавляя свои собственные методы и
  свойства.

Хорошо упомянуть:

- Классы: тесные связи, иерархия.
- Прототипы: конкатенативное наследование, делегирование, функциональное наследование, композиция.

----

## Каррирование

Каррирование - это процесс преобразования функции с несколькими аргументами в последовательность функций, каждая из
которых имеет только 1 аргумент. Работает за счет замыканий.

https://learn.javascript.ru/currying-partials

----

# react

----

## Реконсиляция

React Reconciliation - это рекурсивный алгоритм React, используемый для того, чтобы отличить текущее дерево элементов от
нового для определения частей, которые нужно будет заменить.

1. **Изменение состояния или пропсов:**
   Когда происходит изменение состояния/пропсов в React-компоненте, React создает новое виртуальное дерево элементов (*
   *Virtual DOM**) для представления обновленного состояния.
2. **Сравнение предыдущего и нового деревьев:**
   React берет предыдущее виртуальное дерево и новое виртуальное дерево, затем начинает сравнивать их, начиная с
   корневых элементов.
3. **Сравнение типов элементов:**
   Сначала React сравнивает типы элементов в предыдущем и новом деревьях. Если типы разные, React полностью удаляет
   старый элемент и все его дочерние элементы из реального DOM.
4. **Сравнение дочерних элементов:**
   Если типы элементов совпадают, React рекурсивно сравнивает дочерние элементы обоих деревьев.
5. **Определение изменений:**
   В процессе сравнения React определяет, какие части дерева изменились. Это может включать в себя добавление, удаление
   или обновление компонентов.
6. **Пакет обновлений (Batch Updates)**:
   React группирует изменения, чтобы минимизировать количество манипуляций DOM и повысить производительность. Это
   означает, что React не обновляет DOM после каждого изменения, а собирает все изменения и применяет их одновременно.

7. **Применение изменений:**
   React применяет эти изменения к реальному DOM. Если компонент был изменен, React обновит его атрибуты и содержимое.
   Если компонент был добавлен, React добавит его в DOM. Если компонент был удален, React удаляет его из DOM.

----

## Мемоизация

В программировании мемоизация - это метод оптимизации, который делает приложения более эффективными и, следовательно,
более быстрыми. Он делает это, сохраняя результаты вычислений в кэше и извлекая ту же информацию из кэша в следующий
раз, когда она потребуется, вместо того, чтобы вычислять ее снова.

----

## Мемоизация

В программировании мемоизация - это метод оптимизации, который делает приложения более эффективными и, следовательно,
более быстрыми. Он делает это, сохраняя результаты вычислений в кэше и извлекая ту же информацию из кэша в следующий
раз, когда она потребуется, вместо того, чтобы вычислять ее снова.

----

## хуки

https://reactdev.ru/reference/react/useMemo/#returns

----
