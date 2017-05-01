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
