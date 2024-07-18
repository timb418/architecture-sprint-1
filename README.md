## Задание 1:

## Определение границ микрофронтендов и декомпозиция проекта

Выделим логические блоки для разделения на микрофронтенды:

**1. Микрофронтенд аутентификации (auth-microfrontend):**

* Отвечает за регистрацию, вход и выход пользователя.
* Содержит компоненты `Register`, `Login`, `InfoTooltip`.
* Использует утилиты для аутентификации из `auth.js`.
* Рауты `/signup` и `/signin`.

**2. Микрофронтенд профиля пользователя (profile-microfrontend):**

* Отвечает за отображение и редактирование информации профиля пользователя.
* Содержит компоненты `EditProfilePopup`, `EditAvatarPopup`.
* Предоставляет контекст `CurrentUserContext` для доступа к данным пользователя.

**3. Микрофронтенд карточек (cards-microfrontend):**

* Отвечает за отображение списка карточек, добавление новых карточек, просмотр изображений и взаимодействие с ними (лайки, удаление).
* Содержит компоненты `CardList`, `Card`, `AddPlacePopup`, `ImagePopup`, `api.js`.
* Использует `api.js` для работы с API.

**4. Микрофронтенд футера (footer-microfrontend):**

* Отвечает за отображение футера сайта.
* Содержит компонент `Footer`.

## Выбор паттерна декомпозиции и подхода к интеграции

Для интеграции микрофронтендов предлагается использовать **Webpack Module Federation**. 

В данном случае используется комбинация **трех стратегий**:

* **Вертикальная нарезка:**  
    * Проект разделен на 4 микрофронтенда, каждый из которых отвечает за отдельную бизнес-функцию: `auth-microfrontend` - аутентификация, `profile-microfrontend` - профиль пользователя, `cards-microfrontend` - карточки, `footer-microfrontend` - футер сайта.
    * Каждый микрофронтенд содержит все необходимые компоненты и утилиты для реализации своей функциональности. 

* **Автономность команд:** 
    *  В  коде  сейчас используется один стэк.  Однако,  структура  проекта  позволяет  каждой  команде  выбрать  свой  подход  в  будущем.  Например,  `cards-microfrontend`  может  быть  переписан  на  Vue.js  без  влияния  на  другие  микрофронтенды.

* **Изоляция:**  
    *  Файлы конфигурации Webpack  (`webpack.config.js`)  в  каждом  микрофронтенде  экспортируют  свои  компоненты  через  Module Federation Plugin.  Это  позволяет  избежать  конфликтов  версий  и  обеспечивает  независимость  микрофронтендов.  

**Обоснование:**

* **Вертикальная нарезка** выбрана,  потому  что  проект  достаточно  сложный  и  имеет  четко  выделенные  бизнес-функции.  
* **Автономность команд**  позволит  в  будущем  использовать  разные  технологии  для  разных  микрофронтендов,  если  это  будет  необходимо.  
* **Изоляция**  гарантирует,  что  изменения  в  одном  микрофронтенде  не  повлияют  на  работу  других.  


**Преимущества Webpack Module Federation:**

* **Независимая разработка и развертывание:**  Команды могут работать над своими микрофронтендами независимо, не блокируя друг друга.
* **Обмен зависимостями:** Микрофронтенды могут обмениваться общими зависимостями, такими как React, что уменьшает размер бандла и повышает производительность.
* **Динамическая загрузка:** Микрофронтенды загружаются по требованию, а это ускоряет начальную загрузку приложения.

## Структура проекта
Короткий вариант без стилей:
```
/frontend
├── auth-microfrontend
│   ├── src
│   │   ├── components
│   │   │   ├── Login.js
│   │   │   ├── Register.js
│   │   │   └── InfoTooltip.js
│   │   ├── utils
│   │   │   └── auth.js
│   │   └── index.js
│   ├── package.json
│   └── webpack.config.js
├── profile-microfrontend
│   ├── src
│   │   ├── components
│   │   │   ├── EditProfilePopup.js
│   │   │   └── EditAvatarPopup.js
│   │   ├── contexts
│   │   │   └── CurrentUserContext.js
│   │   └── index.js
│   ├── package.json
│   └── webpack.config.js
├── cards-microfrontend
│   ├── src
│   │   ├── components
│   │   │   ├── Main.js
│   │   │   ├── Card.js
│   │   │   ├── AddPlacePopup.js
│   │   │   └── ImagePopup.js
│   │   ├── utils
│   │   │   └── api.js
│   │   └── index.js
│   ├── package.json
│   └── webpack.config.js
├── footer-microfrontend
│   ├── src
│   │   ├── components
│   │   │   └── Footer.js
│   │   └── index.js
│   ├── package.json
│   └── webpack.config.js
├── src
│   ├── index.js
│   └── components
│       ├── App.js
│       └── Header.js
├── package.json
└── webpack.config.js
```

Полный вариант:
```
/frontend
├── auth-microfrontend
│   ├── public
│   │   └── index.html
│   ├── src
│   │   ├── components
│   │   │   ├── Login.js
│   │   │   ├── Register.js
│   │   │   └── InfoTooltip.js
│   │   ├── styles
│   │   │   ├── auth-form.css
│   │   │   ├── auth-form__button.css
│   │   │   ├── auth-form__form.css
│   │   │   ├── auth-form__input.css
│   │   │   ├── auth-form__link.css
│   │   │   ├── auth-form__text.css
│   │   │   └── auth-form__textfield.css
│   │   ├── utils
│   │   │   └── auth.js
│   │   ├── index.js
│   │   └── index.css
│   ├── package.json
│   └── webpack.config.js
├── profile-microfrontend
│   ├── public
│   │   └── index.html
│   ├── src
│   │   ├── components
│   │   │   ├── EditProfilePopup.js
│   │   │   └── EditAvatarPopup.js
│   │   ├── contexts
│   │   │   └── CurrentUserContext.js
│   │   ├── styles
│   │   │   ├── popup.css
│   │   │   ├── popup__button.css
│   │   │   ├── popup__button_disabled.css
│   │   │   ├── popup__caption.css
│   │   │   ├── popup__close.css
│   │   │   ├── popup__content.css
│   │   │   ├── popup__content_content_image.css
│   │   │   ├── popup__error.css
│   │   │   ├── popup__error_visible.css
│   │   │   ├── popup__form.css
│   │   │   ├── popup__image.css
│   │   │   ├── popup__input.css
│   │   │   ├── popup__input_type_error.css
│   │   │   ├── popup__label.css
│   │   │   └── popup__title.css
│   │   ├── index.js
│   │   └── index.css
│   ├── package.json
│   └── webpack.config.js
├── cards-microfrontend
│   ├── public
│   │   └── index.html
│   ├── src
│   │   ├── components
│   │   │   ├── Main.js
│   │   │   ├── Card.js
│   │   │   ├── AddPlacePopup.js
│   │   │   └── ImagePopup.js
│   │   ├── styles
│   │   │   ├── places.css
│   │   │   ├── places__item.css
│   │   │   ├── places__list.css
│   │   │   ├── card.css
│   │   │   ├── card__delete-button.css
│   │   │   ├── card__delete-button_hidden.css
│   │   │   ├── card__delete-button_visible.css
│   │   │   ├── card__description.css
│   │   │   ├── card__image.css
│   │   │   ├── card__like-button.css
│   │   │   ├── card__like-button_is-active.css
│   │   │   ├── card__like-count.css
│   │   │   ├── card__title.css
│   │   │   ├── popup.css
│   │   │   ├── popup__button.css
│   │   │   ├── popup__button_disabled.css
│   │   │   ├── popup__caption.css
│   │   │   ├── popup__close.css
│   │   │   ├── popup__content.css
│   │   │   ├── popup__content_content_image.css
│   │   │   ├── popup__error.css
│   │   │   ├── popup__error_visible.css
│   │   │   ├── popup__form.css
│   │   │   ├── popup__image.css
│   │   │   ├── popup__input.css
│   │   │   ├── popup__input_type_error.css
│   │   │   ├── popup__label.css
│   │   │   └── popup__title.css
│   │   ├── utils
│   │   │   └── api.js
│   │   ├── index.js
│   │   └── index.css
│   ├── package.json
│   └── webpack.config.js
├── footer-microfrontend
│   ├── public
│   │   └── index.html
│   ├── src
│   │   ├── components
│   │   │   └── Footer.js
│   │   ├── styles
│   │   │   ├── footer.css
│   │   │   └── footer__copyright.css
│   │   ├── index.js
│   │   └── index.css
│   ├── package.json
│   └── webpack.config.js
├── src
│   ├── index.js
│   ├── components
│   │   ├── App.js
│   │   ├── Header.js
│   │   └── ProtectedRoute.js
│   ├── images
│   │   ├── add-icon.svg
│   │   ├── avatar.jpg
│   │   ├── card_1.jpg
│   │   ├── card_2.jpg
│   │   ├── card_3.jpg
│   │   ├── close.svg
│   │   ├── delete-icon.svg
│   │   ├── edit-icon.svg
│   │   ├── error-icon.svg
│   │   ├── like-active.svg
│   │   ├── like-inactive.svg
│   │   ├── logo.svg
│   │   └── success-icon.svg
│   ├── index.css
│   ├── logo.svg
│   ├── serviceWorker.js
│   └── setupTests.js
├── package.json
└── webpack.config.js
```