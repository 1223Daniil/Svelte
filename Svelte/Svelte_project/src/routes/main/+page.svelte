<script>
  import "../../app.css";
  const users = JSON.parse(sessionStorage.getItem("users"));
</script>

<table class="table-auto">
  <thead>
    <tr>
      <th>Имя пользователя</th>
      <th>Email</th>
    </tr>
  </thead>
  <tbody>
    {#each users as user}
      <tr>
        <td class="p-4">{user.username}</td>
        <td class="p-4">{user.email}</td>
      </tr>
    {/each}
  </tbody>
</table>
