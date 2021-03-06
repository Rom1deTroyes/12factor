## XII. Задачі адміністрування
### Виконуйте задачі адміністрування/керування за допомогою разових процесів

[Формація процесів](./concurrency) є певним набором процесів, які необхідні для виконання регулярних задач застосунку (наприклад, обробка веб-запитів). Разом з тим, розробникам часто необхідно виконувати разові адміністративні задачі для обслуговування застосунку, такі як:

* Запуск міграції бази даних (наприклад, `manage.py migrate` в Django, `rake db:migrate` в Rails).
* Запуск консолі ([REPL](http://en.wikipedia.org/wiki/Read-eval-print_loop)) для виконання довільного коду або перевірки моделі застосунку на діючій базі даних. Більшість мов надають REPL шляхом запуску інтерпретатора без будь-яких аргументів (наприклад, `python` або `perl`) або в деяких випадках мають окрему команду (наприклад, `irb` для Ruby, `rails console` для Rails).
* Запуск разових скриптів, збережених в репозиторії застосунку (наприклад, `php scripts/fix_bad_records.php`).

Разові процеси адміністрування слід запускати в такому ж середовищі, в якому запущені регулярні [тривалі процеси](./processes) застосунку. Вони запускаються на базі [релізу](./build-release-run), використовуючи ту ж [кодову базу](./codebase) і [конфігурацію](./config), як і будь-який інший процес на базі цього релізу. Для уникнення проблем з синхронізацією код адміністрування має поставлятися з кодом застосунку.

Для всіх типів процесів мають використовуватися однакові методи [ізоляції залежностей](./dependencies). Наприклад, якщо веб-процес Ruby використовує команду `bundle exec thin start`, то для міграції бази даних слід використовувати `bundle exec rake db:migrate`. Аналогічно, для програми на Python з Virtualenv слід використовувати `bin/python` як для запуску веб-сервера Tornado, так і для запуску будь-яких `manage.py` процесів адміністрування.

Методологія дванадцяти факторів надає перевагу мовам, які мають REPL "з коробки", і які дозволяють легко запускати разові скрипти. У локальному development середовищі розробник може запустити процес адміністрування за допомогою консольної команди всередині директорії застосунку. У production середовищі для запуску такого процесу розробники можуть використовувати ssh або інший механізм віддаленого виконання команд, що надається середовищем виконання.