<script>
    import '../app.css'
</script>
<svelte:head>
    <meta name="og:title" content="Timetable Game" />
    <meta name="twitter:card" content="summary_large_image" />
    <meta property="og:url" content="https://timetable-game.netlify.app" />
    <meta property="og:site_name" content="Timetabe Game" />
    <meta name="robots" content="index, follow" />
    <meta property="og:type" content="Game" />
    <meta name="description" content="A game where you have a minute to answer as many timetable questions as you can!" />
    <meta name="keywords" content="education,game,timetable,1x1=1,vulcanwm" />
    <meta property="og:description" content="A game where you have a minute to answer as many timetable questions as you can!" />
</svelte:head>
<body>
    <div id="header">
        <h1>Timetable Game</h1>
    </div>
    <div id="navbar">
        <a href="/">Game</a>
        <a href="/about">About</a>
        <a href="https://github.com/VulcanWM/timetable-game" target="_blank">GitHub Repo</a>
    </div>
    <div id="main">
        <slot />
    </div>
</body>