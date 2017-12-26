# Состояние загружаемых данных

*Дисклеймер: код максимально упрощён, чтобы проиллюстрировать основную мысль, не отвлекаясь на детали.*

Часто требуется сохранить состояние загружаемых данных — например, списка комментариев к статье. Обычно для этого заводят булевы флаги:

```javascript
const comments = {
  isLoading: false,
  isLoaded: false,
  isError: false,
  errorText: '',
  data: [],
};
```

Флаги легко использовать во внешнем коде, но ими сложно управлять. Так может выглядеть код, управляющий их состоянием:

```javascript
const comments = {
  isLoading: false,
  isLoaded: false,
  isError: false,
  errorText: '',
  data: [],
};

function loadComments() {
  comments.isLoaded = false;
  comments.isError = false;
  comments.isLoading = true;

  fetch('/comments')
    .then((response) => response.json())
    .then((data) => {
      comments.isLoading = false;
      comments.isLoaded = true;
      comments.data = data;
    })
    .catch((error) => {
      commments.isLoading = false;
      comments.isError = true;
      comments.errorText = error.message;
    });
}
```

Получается запутанно и многословно, нужно помнить, значения каких флагов следует сбросить. Более практичный подход — описывать состояние данных одним полем `dataState`, а все флаги автоматически вычислять на основе значения этого поля:

```javascript
const dataStates = {
  notAsked: 'notAsked',
  loading: 'loading',
  loaded: 'loaded',
  failed: 'failed',
};

const comments = {
  dataState: dataStates.notAsked,
  errorText: '',
  data: [],

  get isLoading() {
    return this.dataState === dataStates.loading;
  },

  get isLoaded() {
    return this.dataState === dataStates.loaded;
  },

  get isError() {
    return this.dataState === dataStates.failed;
  }
};
```

Кода стало чуть больше, зато теперь значения всех флагов меняются автоматически, и вероятность ошибки из-за неправильного обновления одного из флагов сведена к нулю. Функция загрузки комментариев становится значительно проще:

```javascript
function loadComments() {
  comments.dataState = dataStates.loading;

  fetch('/comments')
    .then((response) => response.json())
    .then((data) => {
      comments.dataState = dataStates.loaded;
      comments.data = data;
    })
    .catch((error) => {
      comments.dataState = dataStates.failed;
      comments.errorText = error.message;
    });
}
```

Дополнительное преимущество такого подхода — расширяемость. Представим, что для данных нужно добавить ещё одно состояние — например, `deprecated`. Добавление нового состояния ограничивается лишь правкой объекта `comments`:

```javascript
const comments = {
  /* ... */,

  get isDeprecated() {
    return this.dataState === dataStates.deprecated;
  },
};
```

При ручном управлении флагами пришлось бы править код во всех местах, где меняются значения других флагов, чтобы значение `isDeprecated` всегда было актуальным.
