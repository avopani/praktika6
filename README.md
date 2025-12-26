# Практическая работа №6
## Ход работы
### Шаг 1 Подготовка окружения
установила Age

<img width="1276" height="533" alt="image" src="https://github.com/user-attachments/assets/4f2a6152-55b4-44fa-9693-786cb27bb463" />

Установила SOPS и проверила версию: 

<img width="1264" height="161" alt="image" src="https://github.com/user-attachments/assets/2223ff50-f866-4534-a7c3-37f19f8b2ef8" />

### Шаг 2 Создала рабочую директорию и инициализировала пустой репозиторий Git, далее шифровала ключи: 
Для шифрования данных был сгенерирован ключ Age с помощью команды:

```
age-keygen -o keys.txt
```

<img width="1134" height="525" alt="image" src="https://github.com/user-attachments/assets/e0593dcf-8151-4254-a106-1827f8539cc8" />

В результате был создан файл keys.txt, содержащий приватный ключ, а также выведен публичный ключ, который используется для шифрования данных.


### Шаг 3 Настройка конфигурации SOPS 
В корне проекта был создан файл конфигурации .sops.yaml, содержащий правила шифрования:

```
creation_rules:
  - path_regex: 'secrets\\.*\.yaml$'
    age: 'age1...'
```

Правило указывает, что все YAML-файлы в каталоге secrets должны автоматически шифроваться с использованием указанного публичного ключа Age.

<img width="1011" height="484" alt="image" src="https://github.com/user-attachments/assets/e2e9e0b1-f408-441b-a499-060f6b783189" />

### Шаг 4 Создала .gitignore

<img width="1105" height="339" alt="image" src="https://github.com/user-attachments/assets/be1aeecf-446b-41b4-bf37-f8aaa9ccbdb4" />


### Шаг 5 Шифрование конфиденциальных данных

Был создан файл secrets/database.yaml, содержащий чувствительные данные (логин и пароль).

```
database:
  user: postgres
  password: SuperSecretPassword123!
```

Шифрование выполнялось командой и просмотр файла командой ниже:

```
sops --encrypt secrets\database.yaml > secrets\database.enc.yaml
head -30 secrets/database/postgres.enc.yaml
```

<img width="1895" height="599" alt="image" src="https://github.com/user-attachments/assets/59d2a2be-a35e-4d97-8db7-b056bd91e52b" />

В результате был получен зашифрованный файл database.enc.yaml, содержащий зашифрованные значения и служебный блок sops.

<img width="1558" height="160" alt="image" src="https://github.com/user-attachments/assets/64147e47-a32b-4592-b100-146b8aaeae31" />

###  Шаг 6 Расшифровка данных
подготовка к расшифровке 

<img width="1104" height="187" alt="image" src="https://github.com/user-attachments/assets/bbb76cda-e18e-439c-b584-fb4afe36a9ec" />

расшифровка:

<img width="1188" height="300" alt="image" src="https://github.com/user-attachments/assets/e96c77b0-6bcb-43a7-b40b-01f055d45d94" />

### Шаг 7 Редактирование зашифрованных файлов

Для демонстрации возможности редактирования зашифрованного файла была использована команда sops с указанием редактора через переменную окружения:

```
export EDITOR=nano
sops secrets/db.enc.yaml
```

Файл открылся в расшифрованном виде.

Внесено изменение пароля базы данных для тестирования:

<img width="748" height="381" alt="image" src="https://github.com/user-attachments/assets/7380d3c3-03f4-4b85-9661-448ee467e6aa" />

После выхода из редактора SOPS автоматически перешифровал файл.

### Вывод
В ходе выполнения практической работы были изучены и успешно применены инструменты SOPS и Age для безопасного хранения конфиденциальных данных. Использование Ubuntu в WSL позволило развернуть полноценное Linux-окружение на Windows. Было продемонстрировано, что SOPS обеспечивает надёжное шифрование, удобное редактирование и безопасную работу с секретами, что делает данный инструмент полезным в DevOps-практиках. 
