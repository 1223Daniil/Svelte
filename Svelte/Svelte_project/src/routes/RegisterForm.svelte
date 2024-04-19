<script>
  import { createEventDispatcher, getContext } from "svelte";
  import { goto } from "$app/navigation";

  const dispatch = createEventDispatcher();
  const usersStore = getContext("users");

  let username = "";
  let email = "";
  let password = "";
  let confirmPassword = "";
  let errorUsername = "";
  let errorEmail = "";
  let errorPassword = "";
  let errorConfirmPassword = "";

  const handleSubmit = () => {
    // Сбросить предыдущие ошибки
    errorUsername = "";
    errorEmail = "";
    errorPassword = "";
    errorConfirmPassword = "";

    // Валидация имени пользователя
    if (!username.trim()) {
      errorUsername = "Имя пользователя обязательно";
    }

    // Валидация email
    if (!email.trim()) {
      errorEmail = "Email обязателен";
    } else if (!isValidEmail(email)) {
      errorEmail = "Неверный формат email";
    }

    // Валидация пароля
    if (!password.trim()) {
      errorPassword = "Пароль обязателен";
    }

    // Валидация подтверждения пароля
    if (password !== confirmPassword) {
      errorConfirmPassword = "Пароли не совпадают";
    }

    // Если есть ошибки, предотвращаем отправку формы
    if (errorUsername || errorEmail || errorPassword || errorConfirmPassword) {
      return;
    }

    const newUser = {
      username,
      email,
      password,
    };

    // Обновляем список пользователей в хранилище
    usersStore.update((users) => [...users, newUser]);

    // Очищаем поля формы
    username = "";
    email = "";
    password = "";
    confirmPassword = "";

    // Обновляем список пользователей
    dispatch("updateUserList");
    goto("/main");
  };

  // Функция для проверки формата email
  const isValidEmail = (email) => {
    const re = /\S+@\S+\.\S+/;
    return re.test(email);
  };
</script>

<form
  on:submit|preventDefault={handleSubmit}
  class="bg-gray-400 flex flex-col p-5 gap-3 items-center w-1/2 m-auto rounded-2xl"
>
  <h2 class="font-bold">Регистрация</h2>

  <label class="w-1/2 font-semibold">
    Имя пользователя:
    <input type="text" bind:value={username} required class="rounded-lg" />
    {#if errorUsername}
      <p class="text-red-500">{errorUsername}</p>
    {/if}
  </label>

  <label class="w-1/2 font-semibold">
    Email:
    <input type="email" bind:value={email} required class="rounded-lg" />
    {#if errorEmail}
      <p class="text-red-500">{errorEmail}</p>
    {/if}
  </label>

  <label class="w-1/2 font-semibold">
    Пароль:
    <input type="password" bind:value={password} required class="rounded-lg" />
    {#if errorPassword}
      <p class="text-red-500">{errorPassword}</p>
    {/if}
  </label>

  <label class="w-1/2 font-semibold">
    Подтверждение пароля:
    <input
      type="password"
      bind:value={confirmPassword}
      required
      class="rounded-lg"
    />
    {#if errorConfirmPassword}
      <p class="text-red-500">{errorConfirmPassword}</p>
    {/if}
  </label>

  <button type="submit" class="bg-black rounded-md text-white p-2">
    Зарегистрироваться
  </button>

  <a href="/login" class="text-blue-600">Уже Зарегистрированы?</a>
</form>
