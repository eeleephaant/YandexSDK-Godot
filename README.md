[ENG](./README.en.md) [RU](./README.md)

# Yandex games SDK для Godot 4.3+

### Fork by eeleephaant

![Godot и Yandex Игры](https://user-images.githubusercontent.com/101056496/266880767-a4c872d1-180d-4424-b9f8-dfedc2731c51.png "Godot и Yandex Игры")

*Неофициальная* реализация Yandex games SDK для Godot.
Делаю для себя и своих игр, по этому тут реализованны не все методы и не всегда до конца (буду потихоньку доделывать).
Если не хватает каких либо функций или я что-то криво сделал, можете создать ошибку (может быть я до неё дойду) или исправить и залить сюда, буду очень признателен.

Протестировано на Godot 4.3dev6

## Начало работы

Аддон можно установить из assetlib или как сабмодуль если вы используете гит для хранения версий проекта

```
git add submodule https://github.com/BasilYes/godot-yandex-games-sdk.git addons/godot-yandex-games-sdk
```

Просто установите плагин и добавьте "yandex" в feature (не знаю как в переводе) к вашему экспорту (см. скрин ниже)

![Пример экспорта](https://user-images.githubusercontent.com/101056496/266880786-4838d959-b1b3-4bd3-baf3-ebdc79a511f3.png "пример экспорта")

## Методы

Все методы находятся в синглтоне YandexSDK

### Инициализация Yandex SDK

```gdscript
YandexSDK.init_game() -> void
```

Самый первый метод, который необходимо вызвать. Он инициализирует YandexSDK, без его вызова не будет работать ни один другой метод sdk. Это просто реализация [метода](https://yandex.ru/dev/games/doc/ru/sdk/sdk-gameready) начала игры из документации.

### Показ рекламы

```gdscript
YandexSDK.show_ad() -> void
```

Просто показывает пользователю полноэкранную рекламу. Закрытие рекламы или ошибка показа вызовет сигнал **ad(result)**, переменная result содержит значение 'closed', 'error', 'opened'.

### Показ рекламы за вознаграждение

```gdscript
YandexSDK.show_rewarded_ad() -> void
```

Показывает пользователю рекламу с наградой. Вызывает сигнал **rewarded_ad(result)**, переменная result содержит одно из строковых значений 'rewarded', 'closed', 'opened' или 'error'.

### Инициализация данных игрока

```gdscript
YandexSDK.init_player() -> void
```

Инициализация данных игрока, необходимых для сохранения и других действий, связанных с игроком. Приведенные ниже методы не будут работать без вызова этого метода.

### Сохранение данных игрока

```gdscript
YandexSDK.save_data(data: Dictionary, flush: bool = false) -> void
```

Сохраняет данные игрока. Максимальный размер данных не должен превышать 200 KБ. *Внимание* словарь перезаписывает *ВСЕ* данные уже находящиеся на серверах яндекса. (т.е. то, что там было заменятеся на то, что вы отправили)

* **data**: Dictionary, содержащий пары ключ-значение.
* **flush**: Boolean, определяет очередность отправки данных. При значении «true» данные будут отправлены на сервер немедленно; «false» (значение по умолчанию) — запрос на отправку данных будет поставлен в очередь.

### Сохранение численных данных игрока

```gdscript
YandexSDK.save_stats(stats: Dictionary) -> void
```

Сохраняет численные данные игрока. Максимальный размер данных не должен превышать 10 КБ. *Внимание* словарь перезаписывает *ВСЕ* данные уже находящиеся на серверах яндекса. (т.е. то, что там было заменятеся на то, что вы отправили)

* **stats**: Dictionary, содержащий пары ключ-значение, где каждое значение должно быть числом.

### Загрузка данных игрока

```gdscript
YandexSDK.load_data(keys: Array) -> void
```

Отправляет запрос на получение внутриигровых данных игрока, после получения вызывает сигнал **data_loaded(data)**, data - Dictionary с полученными данными

* **keys**: список ключей, которые необходимо вернуть.

### Загрузка численных данных игрока

```gdscript
YandexSDK.load_stats(keys: Array) -> void
```

Отправляет запрос на получение численных данных пользователя, после получения вызывает сигнал **stats_loaded(data)**, data - Dictionary с полученными данными

* **keys**: список ключей, которые необходимо вернуть.

### Инициализация лидербордов

```gdscript
YandexSDK.init_leaderboard() -> void
```

Инициализирует лидерборды игры. Этот метод должен быть вызван перед использованием других функций, связанных с лидербордами. После успешной инициализации вызывается сигнал **leaderboard_initialized**, который указывает, что лидерборды готовы к использованию.

### Сохранение значения в лидерборд

```gdscript
YandexSDK.save_leaderboard_score(leaderboard_name: String, score: int, extra_data: String = "") -> void
```

Сохраняет значение в указанный лидерборд.

- **leaderboard_name**: Строка, представляющая имя лидерборда, в который необходимо сохранить значение.
- **score**: Целое число, представляющее собой результат, который необходимо сохранить в лидерборд.
- **extra_data**: Дополнительные данные, которые могут быть связаны с этой записью в лидерборде (по умолчанию пустая строка).

### Загрузить запись игрока из лидерборда

```gdscript
YandexSDK.load_leaderboard_player_entry(leaderboard_name: String) -> void
```

Загружает запись игрока из указанного лидерборда. После загрузки вызывается сигнал **leaderboard_player_entry_loaded(data)**, где **data** - Dictionary с информацией о записи игрока в лидерборде.

- **leaderboard_name**: Строка, представляющая имя лидерборда, из которого необходимо загрузить запись игрока.

### Загрузить записи игроков

```gdscript
YandexSDK.load_leaderboard_entries(leaderboard_name: String, include_user: bool, quantity_around: int, quantity_top: int) -> void
```

Загружает записи игроков из указанного лидерборда с возможностью настройки количества загружаемых записей и включения информации об авторизованном пользователе. После загрузки вызывается сигнал **leaderboard_entries_loaded(data)**, где **data** - Dictionary с информацией о записях игроков в лидерборде.

- **leaderboard_name**: Строка, представляющая имя лидерборда, из которого необходимо загрузить записи игроков.
- **include_user**: Булево значение, указывающее, нужно ли включать информацию об авторизованном пользователе в результаты загрузки.
- **quantity_around**: Количество записей ниже и выше пользователя по лидерборду, которое необходимо загрузить.
- **quantity_top**: Количество записей из топа лидерборда, которое необходимо загрузить.

###### Проверка авторизации игрока

```gdscript
`YandexSDK.check_is_authorized() -> void`
```

Проверяет, авторизован ли текущий игрок. После проверки вызывает сигнал **check_auth(answer)**, где **answer** - булево значение, указывающее, авторизован ли игрок.

### Открыть окно авторизации

```gdscript
YandexSDK.open_auth_dialog() -> void`
```

Открывает окно авторизации игрока. Перед открытием окна выполняет проверку авторизации.

Больше информации можно найти на [официальном сайте](https://yandex.ru/dev/games/doc/en/sdk/sdk-player).
