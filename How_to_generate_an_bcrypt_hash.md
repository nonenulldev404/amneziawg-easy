# Генерация bcrypt-хэшированного пароля

В файле [`docker-compose.yml`](docker-compose.yml), вместо строки пароля в виде простого текста, требуется пароль, хэшированный с помощью bcrypt. В этом документе объясняется, как сгенерировать хэш на основе пароля в виде простого текста.

## Использование Docker + node

- Вы используете [`docker-compose.yml`](docker-compose.yml)

   Самый простой способ сгенерировать хэш пароля bcrypt с помощью wgpw — использовать docker и node:

    ```sh
    docker run ghcr.io/nonenulldev404/amneziawg-easy:latest node -e 'const bcrypt = require("bcryptjs"); const hash = bcrypt.hashSync("Укажите_Ваш_пароль", 10); console.log(hash.replace(/\$/g, "$$$$"));'
    ```

   Хешированный пароль будет выведен на Ваш терминал. Скопируйте его и используйте в `PASSWORD_HASH` переменной окружения Env в Вашем [`docker-compose.yml`](docker-compose.yml).

- Вы используете `docker run`

    Если Вы используете `docker run` для запуска amneziawg-easy, вы должны заключить строку хэша в одинарные кавычки ( '...'). Вы можете использовать эту команду:

    ```sh
    docker run --rm ghcr.io/nonenulldev404/amneziawg-easy:latest node -e "const bcrypt = require('bcryptjs'); const hash = bcrypt.hashSync('Укажите_Ваш_пароль', 10); console.log('\'' + hash + '\'');"
    ```

    Хешированный пароль будет выведен на Ваш терминал. Скопируйте его и используйте в `PASSWORD_HASH` переменной среды в Вашей команде `docker run`.

## Использование Docker + wgpw

`wg-password` (wgpw) — скрипт, который генерирует хэши паролей bcrypt. Вы можете использовать его с docker:

```sh
docker run ghcr.io/nonenulldev404/amneziawg-easy:latest wgpw Укажите_Ваш_пароль
```

Вы увидите вывод, подобный этому:

```sh
PASSWORD_HASH='$2b$12$coPqCsPtcFO.Ab99xylBNOW4.Iu7OOA2/ZIboHN6/oyxca3MWo7fW'
```

В этом примере `$2b$12$coPqCsPtcFO.Ab99xylBNOW4.Iu7OOA2/ZIboHN6/oyxca3MWo7fW` строка — это Ваш хешированный пароль. Для использования с [`docker-compose.yml`](docker-compose.yml) Вам нужно экранировать каждый символ`$`, добавляя другой `$` перед ним, иначе они будут интерпретированы как переменные. Окончательный пароль, который Вы можете использовать в [`docker-compose.yml`](docker-compose.yml), будет выглядеть так:

```sh
$$2b$$12$$coPqCsPtcFO.Ab99xylBNOW4.Iu7OOA2/ZIboHN6/oyxca3MWo7fW
```
