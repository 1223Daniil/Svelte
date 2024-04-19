<script>
  import { onMount } from "svelte";
  import { setContext } from "svelte";
  import { writable } from "svelte/store";
  import RegisterForm from "./RegisterForm.svelte";
  import "../app.css";
  // Создаем хранилище для списка пользователей
  const usersStore = writable([]);
  let lengthUsers = JSON.parse(sessionStorage.getItem("users")).length;
  // Подписываемся на изменения списка пользователей и сохраняем их в sessionStorage (только в браузере)

  usersStore.subscribe((value) => {
    sessionStorage.setItem("users", JSON.stringify(value));
  });

  // Функция для обновления списка пользователей
  const updateUserList = () => {
    const users = JSON.parse(sessionStorage.getItem("users")) || [];
    usersStore.set(users);
  };

  // Добавляем хранилище пользователей в контекст
  setContext("users", usersStore);

  onMount(updateUserList);
</script>

<RegisterForm {updateUserList} />

<!-- По итогу решино регистрацию оставить на клавнй странтуце через if остовлять и убират её -->
