# Советы для разработчика интерфейсов

- [JavaScript](#javascript)

## JavaScript

### Именование переменных и функций

Имя переменной должно описывать суть хранимого значения и начинаться
с существительного (или прилагательного, а затем существительного):

```javascript
// плохо, начинается с глагола
const getUserName = 'andrew-r';

// хорошо
const userName = 'andrew-r';
```

Имя функции должно начинаться с глагола:

```javascript
// плохо
function nextPageNumber() {}

// хорошо
function getNextPageNumber() {}
```

Имя функции должно описывать не момент вызова, а совершаемое действие:

```javascript
// плохо
function onClick() {}

// хорошо
function goToNextPage() {}
```

Имена предикатов (переменных и функций) должны быть вопросами, на которые можно
ответить «да» или «нет». Если вопрос сам по себе не является глаголом (например,
includes, equals), имя предиката должно начинаться с is, are, has, can и т. п.:

```javascript
// плохо
const authorisation = Boolean(state.currentUser);
const allowPublishing = state.currentUser.permissions.includes('publishing');
const strictEquality = (a) => (b) => a === b;

// хорошо
const isAuthorised = Boolean(state.currentUser);
const canPublish = state.currentUser.permissions.includes('publishing');
const hasFriends = state.currentUser.friends.length > 0;
const strictEquals = (a) => (b) => a === b;
```
