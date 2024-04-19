<script>
  import { createEventDispatcher } from "svelte";
  import "../../app.css";

  let email = "";
  let password = "";

  const dispatch = createEventDispatcher();

  const handleSubmit = () => {
    // Проверка наличия пользователя с указанным email и паролем в sessionStorage
    const users = JSON.parse(sessionStorage.getItem("users")) || [];
    const user = users.find(
      (u) => u.email === email && u.password === password
    );
    console.log(users);
    if (user) {
      alert("Авторизация успешна!");
      // Очищаем поля формы
      email = "";
      password = "";
    } else {
      alert("Неправильный email или пароль");
    }
  };
</script>

<form
  on:submit|preventDefault={handleSubmit}
  class="bg-gray-400 flex flex-col p-5 gap-3 items-center w-1/2 m-auto rounded-2xl"
>
  <h2 class="font-bold">Авторизация</h2>
  <label>
    Email:
    <input
      class="w-1/2 font-semibold"
      type="email"
      bind:value={email}
      required
    />
  </label>
  <label>
    Пароль:
    <input
      class="w-1/2 font-semibold"
      type="password"
      bind:value={password}
      required
    />
  </label>
  <button type="submit" class="bg-black rounded-md text-white p-2">Войти</button
  >
  <a href="/" class="text-blue-600">Зарегистрироваться</a>
</form>
