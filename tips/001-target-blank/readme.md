# Ссылки с `target="_blank"`

Добавляйте к ссылкам с `target="_blank"` атрибут `rel="noopener noreferrer"`, чтобы открытая вкладка (или окно) браузера не имели доступ к родительской вкладке (это нарушает безопасность). [Подробное объяснение и демо](https://mathiasbynens.github.io/rel-noopener/).

Кроме того, согласно [исследованию Джейка Арчибальда](https://jakearchibald.com/2016/performance-benefits-of-rel-noopener/), отсутствие `rel="noopener"` в некоторых случаях плохо сказывается на производительности.
