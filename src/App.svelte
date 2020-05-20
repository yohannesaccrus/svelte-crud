<script>
  import { Router, Route } from "svelte-routing";

  import Navbar from "./layouts/Navbar.svelte";

  import Home  from "./pages/Home.svelte";
  import About from "./pages/About.svelte";
</script>

<Router>
  <Navbar />
  <Route path={'/'} component={Home} />
  <Route path={'/about'} component={About} />
</Router>
