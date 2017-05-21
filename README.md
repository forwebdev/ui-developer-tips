# Советы для разработчика интерфейсов

- [HTML](#html)
- [JavaScript](#javascript)

## HTML

### Ссылки с `target="_blank"`

Добавляйте к ссылкам с `target="_blank"` атрибут `rel="noopener noreferrer"`,
чтобы открытая вкладка (или окно) браузера не имели доступ к родительской
вкладке (это нарушает безопасность).
[Подробное объяснение и демо](https://mathiasbynens.github.io/rel-noopener/).

Кроме того, согласно [исследованию Джейка Арчибальда](https://jakearchibald.com/2016/performance-benefits-of-rel-noopener/),
отсутствие `rel="noopener"` в некоторых случаях плохо сказывается
на производительности.

## JavaScript

### Именование переменных и функций

Явное лучше неявного. Не нужно использовать сокращения для имён,
даже если кажется, что эти сокращения общеизвестны. Другой человек,
возможно, раньше с ними не сталкивался. Также сокращения вынуждают держать
в голове контекст их использования:

```javascript
console.log(e); // что такое e?

// Плохо, сокращение требует контекста:
document.addEventListener('click', (e) => {
	console.log(e);
});

 try {} catch (e) {
	 console.log(e);
 }

 // Хорошо, явное имя
document.addEventListener('click', (event) => {
	console.log(event);
})

try {} catch (error) {
	console.log(error);
}
```

Имена предикатов должны быть вопросами, на которые можно
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

#### Переменные

Имя переменной должно описывать суть хранимого значения и начинаться
с существительного (или прилагательного, а затем существительного):

```javascript
// плохо, начинается с глагола
const getUserName = 'andrew-r';

// хорошо
const userName = 'andrew-r';
const currentPageNumber = 1;
```

#### Функции

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

Если функция возвращает или устанавливает какое-либо значение, её имя должно начиться с `get` или, соответственно, `set`:

```javascript
// плохо
function userAge() {}
function userName(name) {}

// хорошо
function getUserAge() {}
function setUserName(name) {}
```
