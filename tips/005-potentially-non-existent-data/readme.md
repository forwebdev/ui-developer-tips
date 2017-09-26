# Работа с потенциально несуществующими данными

Задача: записать в переменную значение вложенного на несколько уровней свойства какого-либо объекта. Загвоздка в том, что этот объект может быть пустым. В таком
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

Такой способ работает, но в нём много дублирования кода и в целом он многословный.

Более лаконичная альтернатива без дублирования кода:

```javascript
let loadedComments = null;

fetch('...')
	.then((response) => response.json())
	.then((response) => {
		loadedComments = ((response.result || {}).user || {}).comments || [];
	});
```
