const { chromium } = require('playwright');

(async () => {
  const browser = await chromium.launch();
  const context = await browser.newContext();
  const page = await context.newPage();

  // Шаг 1: Переходим на главную страницу сайта
  await page.goto('https://www.sima-land.ru/');

  // Шаг 2: Нажимаем на кнопку "Войти" в хедере
  await page.click('.header__auth__login');

  // Шаг 3: Вводим логин и пароль
  await page.fill('#auth_login_email', 'qa_test@test.ru');
  await page.fill('#auth_login_password', 'qa_test');

  // Шаг 4: Нажимаем на кнопку "Войти"
  await Promise.all([
    page.waitForNavigation(),
    page.click('.auth__login__btn'),
  ]);

  // Шаг 5: Проверяем, что авторизация прошла успешно
  const title = await page.title();
  if (title === 'Интернет-магазин Sima-land') {
    console.log('Авторизация прошла успешно!');
  } else {
    console.log('Авторизация не прошла!');
  }

  await browser.close();
})();
