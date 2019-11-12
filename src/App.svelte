<script>
  import Header from "./Header.svelte";
  import Home from "./Home.svelte";
  import Item from "./Item.svelte";

  import { Router, Route } from "svelte-routing";

  export let url = "";
</script>

<Header />

<Router {url}>
  <div>
    <Route path="/" component={Home} />
    <Route path="/item/:id" component={Item} />
  </div>
</Router>
