# AutotestBack
Внимание! Код создан для текущего проекта, который я тестирую и является первым опытом в автоматизации процессов. Скрипт написан на Python, для его работы необходимо установить dotnet, dotnet ef, newman, pyautogui. Также в коде есть дополнительные закомментированные шаги на случай, если у пользователя нет сгенерированного ssh-ключа для доступа на удаленный сервер AstraLinux (то есть доступ будет при вводе пароля: читать в коде строки 53, 139, 163)

На локальной машине по пути c:\Project\ProjectBack\ имеется склонированный репозиторий бэка реализуемого проекта, пусть он будет иметь название Project. Для тестирования методов его серверной части написаны тесты в postman. Для автоматизации запуска этих тестов используется код из AutotestBack/RunPostmanTest, который выполняет следующие действия:
Строки 78-82:
1.	Создает папку на локальной машине, где будет размещаться publish Project’a, а также скрипт для создания базы данных на удаленной машине с AstraLinux.

Строки 84-92:
2.	Создает publish Project’a (той ветки, которая необходима) со всеми необходимыми атрибутами.

Строки 94-99:
3.	Создает идемпотентный скрипт базы данных Project’a со всеми необходимыми атрибутами с выводом скрипта в файл step2.sql.

Строки 101-108:
4.	Копирует на локальной машине созданный publish Project’a и скрипт базы данных в папку из п.1 с удалением appsettings.

Строки 111-138:
5.	Подключает локальную машину по ssh к удаленной машине AstraLinux, копирует на нее подготовленную папку с publish Project’a и скрипт для базы данных.
6.	Останавливает службы (demon) на AstraLinux.
7.	Обновляет проект на удаленной машине с AstraLinux и создает базу данных с необходимой мандатной меткой (в данном случае 3.0) и необходимым признаком параметра CCR (в данном случае CCR OFF).
8.	Запускает службы (demon) на AstraLinux и удаляет уже ненужную папку с publish Project’a.

Строки 150-153:
9.	Запускает подготовленные на локальной машине postman-тесты методов серверной части с помощью cli приложения newman и выводит результаты тестов в лог с датой и временем.

Строки 156-161:
10.	После прогонки тестов удаляет созданную базу данных.

После всех операций нужно открыть лог завершенных тестов и проанализировать их результаты. 