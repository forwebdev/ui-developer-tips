# Советы для разработчика интерфейсов

- [HTML](#html)
- [Доступность](#доступность)
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

## Доступность

### `aria-label`

Используйте атрибут `aria-label` для описания элементов интерфейса, которые
не получается описать с помощью обычного `<label>`.

```html
<button aria-label='Закрыть окно'>✕</button>
```

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

### Объявление параметров функций

Обязательные параметры функции должны перечисляться в первую очередь.
Опциональные параметры следует объединить в один параметр `options`, следующий
в конце списка параметров:

```javascript
// плохо
function initTabs(rootNode, defaultTabIndex = 0, trackEvents = false) {}

// непонятно, что означают последние два параметра,
// если не посмотреть на исходное объявление функции
initTabs(document.querySelector('.js-tabs'), 1, true);

// хорошо
function initTabs(rootNode, options) {
	const { defaultTabIndex = 0, trackEvents = false } = options;
}

// понятно, за что отвечает каждый параметр
initTabs(document.querySelector('.js-tabs'), {
	defaultTabIndex: 0,
	trackEvnets: false,
});
```

### Работа с потенциально несуществующими данными

Задача: записать в переменную значение вложенного на несколько уровней свойства
какого-либо объекта. Загвоздка в том, что этот объект может быть пустым. В таком
случае в переменную нужно установить значение по умолчанию.

Велик соблазн начать вручную проверять существование каждого уровня вложенности:

```javascript
let loadedComments = null;

fetch('...')
	.then((response) => response.json())
	.then((response) => {
		if (response.result && response.result.user && response.result.user.comments) {
			loadedComments = response.result.user.comments;
		} else {
			loadedComments = [];
		}
	});
```

Такой способ работает, но в нём много дублирования кода и в целом
он многословный.

Более лаконичная альтернатива без дублирования кода:

```javascript
let loadedComments = null;

fetch('...')
	.then((response) => response.json())
	.then((response) => {
		loadedComments = ((response.result || {}).user || {}).comments || [];
	});
```
