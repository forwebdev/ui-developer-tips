# Объявление параметров функций

Обязательные параметры функции должны перечисляться в первую очередь. Опциональные параметры следует объединить в один параметр `options`, следующий
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
