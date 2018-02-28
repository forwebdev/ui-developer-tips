# Хранение утилитарных функций в проекте

Не храните все общие функции в одном файле `utils.js`. Сразу создайте директорию `utils` и сохраняйте каждую функцию в отдельный файл. В противном случае ваш монолитный файл `utils.js` раздуется до неприличных размеров, и рано или поздно вы всё равно захотите разбить его на отдельные файлы.

Плохо:

```
.
├── utils.js
└── utils.spec.js
```

Хорошо:

```
utils
├── chunk
│   ├── chunk.js
│   └── chunk.spec.js
├── flat-map
│   ├── flat-map.js
│   └── flat-map.spec.js
└── pluck
    ├── pluck.js
    └── pluck.spec.js
```

В случае с одним файлом `utils.js` можно импортировать несколько функций одновременно:

```
import { chunk, pluck } from '../utils.js';
```

Чтобы сохранить такую возможность после разбиения `utils.js` на отдельные файлы, добавьте в директорию `utils` файл `index.js` и экспортируйте из него все хелперы:

```
import chunk from './chunk/chunk';
import pluck from './pluck/pluck';

export {
	chunk,
	pluck
};
```

С помощью babel-плагина [export extensions](https://babeljs.io/docs/plugins/transform-export-extensions/) можно написать то же самое лаконичнее:

```
export { default as chunk } from './chunk/chunk';
export { default as pluck } from './pluck/pluck';
```

В результате можно будет писать импорты вида `import { chunk } from '../utils';`.

