<script>
    import Header from './Components/Header.svelte';
    import Hero from './Components/Hero.svelte';
    import FindCity from './Components/FindCity.svelte';
    import SearchBox from './Components/SearchBox.svelte';
</script>

<svelte:head>
    <title>Decathlon Rent : noleggia il tuo spor</title>
    <link rel="icon" type="image/png" href="https://rent.decathlon.it/assets/icon/favicon.png" />
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" />
    <link href="https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@300;400;700&display=swap" rel="stylesheet">
</svelte:head>

<Header />
<main>
    <Hero />
    <SearchBox />
</main>

<style lang="scss" global>
    @import "./style/global.scss";
    main {
        position: relative;
    }
</style>