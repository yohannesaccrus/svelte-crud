<script>
  import { Link } from "svelte-routing";
</script>

<nav>
  <div class={'container'}>
    <div class={'row'}>
      <div class={'column large:two'}>
        <div class="logo">
          <Link to={'/'}>
            <h1>Svelte</h1>
          </Link>
        </div>
      </div>
      <div class={'column large:ten'}>
        <ul class={'menus'}>
          <li>
            <Link to={'/'}>Home</Link>
          </li>
          <li>
            <Link to={'/about'}>About</Link>
          </li>
        </ul>
      </div>
    </div>
  </div>
</nav>
