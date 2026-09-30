<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Dm2727 – dm2727.org.uk</title>
  <style>
    body {
      margin: 0;
      font-family: "Helvetica World", Helvetica, Arial, sans-serif;
      background-color: #f5f5f5;
      color: #222;
    }

    header {
      background-color: #111;
      color: #fff;
      padding: 20px;
      text-align: center;
    }

    header h1 {
      margin: 0;
      font-size: 2rem;
      letter-spacing: 2px;
    }

    header p {
      margin: 5px 0 0;
      font-size: 0.95rem;
      opacity: 0.8;
    }

    /* Red ticker */
    .ticker-wrapper {
      background-color: #b00000;
      color: #fff;
      overflow: hidden;
      white-space: nowrap;
      box-sizing: border-box;
    }

    .ticker {
      display: inline-block;
      padding: 10px 0;
      animation: ticker-scroll 20s linear infinite;
    }

    .ticker span {
      margin-right: 50px;
      font-size: 0.95rem;
    }

    @keyframes ticker-scroll {
      0% { transform: translateX(100%); }
      100% { transform: translateX(-100%); }
    }

    main {
      max-width: 900px;
      margin: 40px auto;
      padding: 0 20px;
    }

    main p {
      line-height: 1.6;
      margin-bottom: 15px;
    }

    footer {
      text-align: center;
      padding: 20px;
      font-size: 0.85rem;
      color: #666;
    }

    a {
      color: #b00000;
      text-decoration: none;
    }

    a:hover {
      text-decoration: underline;
    }
  </style>
</head>
<body>
  <header>
    <h1>Dm2727</h1>
    <p>if youre in dm2727.github.io do not go. its dm2727.org.uk </p>
  </header>

  <div class="ticker-wrapper">
    <div class="ticker">
      <span>BRO WHAT IS HAPPENING A THUNDERBOLT IN PH?? SIREN MAP 2.0? EVEN A NEW DISCORD SERVER????</span>
    </div>
  </div>

  <main>
    <h2>about me</h2>

    <p>
      um hello there. its Dm2727, who moved in 28 sep 2023 (3 years ago) here you can find some stuff some siren stuff and yes…
    </p>

    <p>
      mario kart stuff
    </p>

    <p>
      anyways check my other stuff at
      <a href="http://dm2727.org.uk/linkz" target="_blank" rel="noopener noreferrer">
        http://dm2727.org.uk/linkz
      </a>
    </p>
  </main>

  <footer>
    &copy; <span id="year"></span> Dm2727 – dm2727.org.uk
  </footer>

  <script>
    document.getElementById('year').textContent = new Date().getFullYear();
  </script>
</body>
</html>
