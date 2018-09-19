# `URL.createObjectURL` вместо `FileReader.readAsDataURL`

Если вам нужно отобразить картинку, которая изначально представлена в виде [файла](https://developer.mozilla.org/en-US/docs/Web/API/File) или [блоба](https://developer.mozilla.org/en-US/docs/Web/API/Blob), не используйте для этого [`FileReader.readAsDataURL`](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/readAsDataURL) — он требует значительных ресурсов для чтения содержимого блоба и его конвертации в data URL, хоть это и происходит асинхронно и не блокирует основной поток.

Вместо него лучше применить синхронный [`URL.createObjectURL`](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL) — он моментально сгенерирует и свяжет с блобом временный URL, который можно использовать как угодно, например, в качестве `src` для `<img />`. Генерация URL для блоба не требует чтения его содержимого, поэтому работает быстро и не расходует ресурсы ([подробное описание алгоритма в спецификации](https://w3c.github.io/FileAPI/#url-model)).

Пока для блоба существуют временные URL, он не может быть удалён из памяти сборщиком мусора, поэтому по завершении использования URL не забудьте отвязать его от блоба с помощью вызова [`URL.revokeObjectURL`].(https://developer.mozilla.org/en-US/docs/Web/API/URL/revokeObjectURL).
