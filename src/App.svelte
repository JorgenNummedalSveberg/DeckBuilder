<script>
    import {Router, Link, Route} from "svelte-routing";
    import Home from "./Home/Home.svelte";
    import DeckGenerator from "./DeckGenerator/DeckGenerator.svelte";

    export let url = "/";
</script>

<Router {url}>
    <nav>
        <Link to="/">Home</Link>
        <Link to="/deck-generator">Deck generator</Link>
<!--        <Link to="/blog">Business simulator</Link>-->
    </nav>
    <div>
        <Route path="/"><Home /></Route>
        <Route path="/deck-generator"><DeckGenerator/></Route>
<!--        <Route path="/blog" component={Blog} />-->
<!--        <Route path="/blog/:id" component={BlogPost} />-->
    </div>
</Router>
