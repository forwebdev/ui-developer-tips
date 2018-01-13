# Доступные переходы между страницами в SPA

В SPA сломано много вещей, которые нормально работают в обычных сайтах. Одна из таких вещей — переходы между страницами. Используете динамический роутинг? Оберните содержимое страниц в элемент с `tabindex="-1"`, `role="region"` и `aria-label="Содержимое текущей страницы"`, а при переходах между страницами устанавливайте фокус на эту обёртку. Эта базовая техника сделает ваше приложение немного доступнее для тех, кто пользуется кнопкой tab или экранными читалками.

Реализация такой обёртки на Реакте:

```javascript
import React, { PureComponent } from 'react';
import PropTypes from 'prop-types';

export default class A11yRegion extends PureComponent {
  static propTypes = {
    autoFocus: PropTypes.bool,
    children: PropTypes.node
  };

  componentDidMount = () => {
    if (this.props.autoFocus) {
      this.rootNode.focus();
    }
  };

  render = () => {
    const { children } = this.props;

    return (
      <div
        ref={(node) => (this.rootNode = node)}
        className={styles.root}
        role='region'
        tabIndex='-1'
        aria-label='Page content'>
        {children}
      </div>
    );
  };
}

export const wrapWithA11yRegion = (WrappedComponent, a11yRegionProps = {}) => (props) => {
  return (
    <A11yRegion {...a11yRegionProps}>
      <WrappedComponent {...props} />
    </A11yRegion>
  );
};
```

Пример использования этой обёртки с Реакт-Роутером третьей версии:

```javascript
import { wrapWithA11yRegion } from './a11y-region.js';
/* ... */

export default () => (
  <Route component={App}>
    <Route path='/' component={wrapWithA11yRegion(MainPage, { autoFocus: true })} />
    <Route path='/news' component={wrapWithA11yRegion(NewsPage, { autoFocus: true })} />
  </Route>
);
```
