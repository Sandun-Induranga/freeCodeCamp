---
id: 58966a17f9fc0f352b528e6d
title: Реєстрація нових користувачів
challengeType: 2
forumTopicId: 301561
dashedName: registration-of-new-users
---

# --description--

Тепер потрібно дозволити новому користувачеві створити обліковий запис на нашому сайті. У `res.render` для головної сторінки додайте нову змінну до переданого об'єкта `showRegistration: true`. Після оновлення вашої сторінки, ви побачите реєстраційну форму, яка вже була створена в файлі `index.pug`! Ця форма налаштована для надсилання **POST** запиту до `/register`, тому тут ми повинні прийняти **POST** і створити об'єкт користувача в базі даних.

Логіка реєстраційного маршруту має бути наступною: Реєстрація нового користувача > Автентифікація нового користувача > Переадресація на /profile

Логіка кроку 1, реєстрація нового користувача, повинна бути такою: Зробіть запит до бази даних за допомогою команди findOne > Якщо користувач був повернений, то він існує і переадресовується на головну сторінку *АБО* якщо користувач не визначений і помилки не виникло, тоді 'insertOne' у базу даних з ім'ям користувача та паролем, і поки не з'явиться помилок, викликайте *наступного*, щоб перейти до кроку 2, автентифікації нового користувача, для якого вже написана логіка в маршруті POST */login*.

```js
app.route('/register')
  .post((req, res, next) => {
    myDataBase.findOne({ username: req.body.username }, function(err, user) {
      if (err) {
        next(err);
      } else if (user) {
        res.redirect('/');
      } else {
        myDataBase.insertOne({
          username: req.body.username,
          password: req.body.password
        },
          (err, doc) => {
            if (err) {
              res.redirect('/');
            } else {
              // The inserted document is held within
              // the ops property of the doc
              next(null, doc.ops[0]);
            }
          }
        )
      }
    })
  },
    passport.authenticate('local', { failureRedirect: '/' }),
    (req, res, next) => {
      res.redirect('/profile');
    }
  );
```

Підтвердьте сторінку, якщо все виконано вірно. If you're running into errors, you can <a href="https://gist.github.com/camperbot/b230a5b3bbc89b1fa0ce32a2aa7b083e" target="_blank" rel="noopener noreferrer nofollow">check out the project completed up to this point</a>.

**ПРИМІТКА:** З цього моменту можуть виникнути проблеми з використанням браузеру *picture-in-picture*. Якщо ви використовуєте онлайн IDE, який має попередній перегляд програми в редакторі, рекомендується відкрити цей попередній перегляд у новій вкладці.

# --hints--

Ви повинні зареєструвати маршрут та відобразити його на головній сторінці.

```js
(getUserInput) =>
  $.get(getUserInput('url') + '/_api/server.js').then(
    (data) => {
      assert.match(
        data,
        /showRegistration:( |)true/gi,
        'You should be passing the variable showRegistration as true to your render function for the homepage'
      );
      assert.match(
        data,
        /register[^]*post[^]*findOne[^]*username:( |)req.body.username/gi,
        'You should have a route accepted a post request on register that querys the db with findone and the query being username: req.body.username'
      );
    },
    (xhr) => {
      throw new Error(xhr.statusText);
    }
  );
```

Реєстрація повинна працювати.

```js
async (getUserInput) => {
  const url = getUserInput('url');
  const user = `freeCodeCampTester${Date.now()}`;
  const xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function () {
    if (this.readyState == 4 && this.status == 200) {
      test(this);
    } else {
      throw new Error(`${this.status} ${this.statusText}`);
    }
  };
  xhttp.open('POST', url + '/register', true);
  xhttp.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
  xhttp.send(`username=${user}&password=${user}`);
  function test(xhttpRes) {
    const data = xhttpRes.responseText;
    assert.match(
      data,
      /Profile/gi,
      'Register should work, and redirect successfully to the profile.'
    );
  }
};
```

Вхід в обліковий запис повинен працювати.

```js
async (getUserInput) => {
  const url = getUserInput('url');
  const user = `freeCodeCampTester${Date.now()}`;
  const xhttpReg = new XMLHttpRequest();
  xhttpReg.onreadystatechange = function () {
    if (this.readyState == 4 && this.status == 200) {
      login();
    } else {
      throw new Error(`${this.status} ${this.statusText}`);
    }
  };
  xhttpReg.open('POST', url + '/register', true);
  xhttpReg.setRequestHeader(
    'Content-type',
    'application/x-www-form-urlencoded'
  );
  xhttpReg.send(`username=${user}&password=${user}`);
  function login() {
    const xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
      if (this.readyState == 4 && this.status == 200) {
        test(this);
      } else {
        throw new Error(`${this.status} ${this.statusText}`);
      }
    };
    xhttp.open('POST', url + '/login', true);
    xhttp.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
    xhttp.send(`username=${user}&password=${user}`);
  }
  function test(xhttpRes) {
    const data = xhttpRes.responseText;
    assert.match(
      data,
      /Profile/gi,
      'Login should work if previous test was done successfully and redirect successfully to the profile.'
    );
    assert.match(
      data,
      new RegExp(user, 'g'),
      'The profile should properly display the welcome to the user logged in'
    );
  }
};
```

Вихід з облікового запису повинен працювати.

```js
(getUserInput) =>
  $.ajax({
    url: getUserInput('url') + '/logout',
    type: 'GET',
    xhrFields: { withCredentials: true }
  }).then(
    (data) => {
      assert.match(data, /Home/gi, 'Logout should redirect to home');
    },
    (xhr) => {
      throw new Error(xhr.statusText);
    }
  );
```

Профіль більше не повинен працювати після виходу з облікового запису.

```js
(getUserInput) =>
  $.ajax({
    url: getUserInput('url') + '/profile',
    type: 'GET',
    crossDomain: true,
    xhrFields: { withCredentials: true }
  }).then(
    (data) => {
      assert.match(
        data,
        /Home/gi,
        'Profile should redirect to home when we are logged out now again'
      );
    },
    (xhr) => {
      throw new Error(xhr.statusText);
    }
  );
```

# --solutions--

```js
/**
  Backend challenges don't need solutions, 
  because they would need to be tested against a full working project. 
  Please check our contributing guidelines to learn more.
*/
```
