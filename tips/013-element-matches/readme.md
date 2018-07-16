# Element.matches()

Для проверки активности элемента часто используется что-то вроде `node.classList.contains('active')`. В спецификации DOM есть более удобный и универсальный метод для проверки элемента на соответствие селектору — `node.matches('.active')`.

[Поддержка браузерами](https://caniuse.com/#feat=matchesselector) приличная, но для Edge 14- и IE нужно подключить [полифил](https://developer.mozilla.org/en-US/docs/Web/API/Element/matches#Polyfill).
