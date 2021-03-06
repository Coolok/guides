Новые функции в Ember.js добавляются в рамках условных выражений.

Код, который стоит за этими флажками, можно условно активировать (или полностью убрать) на основе конфигурации вашего проекта. Это позволяет выборочно выпускать новые разработанные функции, когда сообщество Ember.js полагает, что они готовы к практическому использованию.

## Особенность жизненного цикла

Это новая условная особенность доступна только в сборках canary (самых ранних). Ее можно подключить во время выполнения программы через конфигурационный файл проекта.

В самом начале цикла beta основная команда Ember оценивает каждую новую функцию. Функции, которые считаются стабильными, делают доступными в следующей бета-версии и активируют их по умолчанию.

Бета-функции, которые получили негативный отклик у сообщества, отключаются в следующем бета-релизе, и уже не попадают в стабильный релиз. Их еще могут включить в последующий цикл beta, если проблемы с ними будут решены.

Когда цикл beta завершается, все функции, которые были в нем подключены, попадают в следующий стабильный релиз. В этот момент флажки функций удаляются из сборки canary и последующих ответвлений beta, и функции становятся частью фреймворка.

## Детали пометок

Статус флажка в сгенерированной сборке контролируется в файле `features.json` в корне проекта Ember.js. Этот файл содержит список всех новых функций и их текущий статус.

Функция может иметь один из трех флажков:

* `true` — функция **присутствует** и **активирована**: код всегда доступен в сгенерированной сборке.
* `null` — функция **присутствует**, но **отключена** в сборке. Ее можно активировать во время выполнения.
* `false` — функция полностью **отключена**: код отсутствует в сгенерированной сборке.

Процесс удаления флажков из сборки на выходе обрабатывается скриптом [`defeatureify`](https://github.com/thomasboyt/defeatureify).

## Список функций ([`FEATURES.md`](https://github.com/emberjs/ember.js/blob/master/FEATURES.md))

Когда разработчик добавляет новую функцию в канал `canary` (то есть в ветку `master` на github), он также добавляет запись в [`FEATURES.md`](https://github.com/emberjs/ember.js/blob/master/FEATURES.md), которая поясняет, что эта функция делает, и привязывает функцию к исходному запросу на включение. Этот список обновляется и отражает, что доступно в каждом канале (`stable`, `beta` и `master`).

## Активация во время исполнения

При использовании сборок canary и beta вы можете **активировать** любую функцию, которая  «присутствует, но **отключена**», прежде чем приложение загрузится, установите значение флажка на `true`:

`config/environment.js`
```js
var ENV = {
  EmberENV: {
    FEATURES: {
      'link-to': true
    }
  }
};
```

По-настоящему амбициозные разработчики могут установить `ENV.EmberENV.ENABLE_ALL_FEATURES` на `true`, чтобы активировать все экспериментальные функции.