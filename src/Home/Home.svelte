<script>
    import {Link} from "svelte-routing";
</script>
<main>
    <h1>Cyberethics</h1>
    <p>Cyberethics refers to the deep and complex ethical questions we have to ask ourselves that come along with the usage of advanced computer technologies. This can range from security concerns to accessibility and discrimination. This project, and in turn this website, is meant to make an attempt at helping educators others with making people aware of the different aspects of the complex topic that is cyberethics. Currently this website hosts two games meant to facilitate this.
    </p>
    <h3>Cards for humanity(?)</h3>
    <p>
        This is a card game where one player takes on the role of a persona, and the others use cards and their own creativity to come up with products to help them and weaknesses to put the other ideas down.

        The game is meant to be played with a deck of personas fitted to a certain domain, like elder care, personal-economy or social media, where the cards are meant to represent the eventual end users who will actually end up using the product. The technologies can be relatively general, to allow the players to be creative with their ideas, while the weaknesses are logical to cater to the domain itself to stay relevant to the discussion.

        To help make this modifiable to different use cases, a tool for building these decks and exporting them to PDF for easy printing is available on this website. There are also some default decks available.
    </p>
    <Link to="/deck-generator">Play here!</Link>
    <h3>Business simulator</h3>
    <p>
        This is a classroom/conference game where players are divided into groups and huddle around a computer and connect with their phones to vote on what response to choose when the companies your team is working as consultants for happen upon ethical dilemmas.

        The game has a central hub set up by the host, where they pick the problem set for the session, and then a group hub which each team joins with one computer. Then each team member joins the group session on their phone with the phone client. All clients are hosted on this webpage.
    </p>
    <nav>
        <Link>Central hub</Link>
        <Link>Group hub</Link>
        <Link>Phone client</Link>
    </nav>
</main>
