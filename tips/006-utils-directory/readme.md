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

В дополнение к этому, вы можете использовать ещё одну хорошую практику: создайте в папке `utils` файл `index.js` и там импортируйте и экспортируйте ваши функции.
```
// index.js

import chunk from './chunk/chunk'
import flatMap from './flat-map/flat-map'
import pluck from './pluck/pluck'

export {
	chunk,
	flatMap,
	pluck
}
```

Это позволит не импортировать в проекте вот так:
```
import chunk from './utils/chunk/chunk'
import flatMap from './utils/flat-map/flat-map'
import pluck from './utils/pluck/pluck'
```
А вот так:
```
import { chunk, flatMap, pluck } from './utils'
```
