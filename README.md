<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Moving Banner</title>
  <style>
    .banner-wrapper {
      width: 100%;
      overflow: hidden;
      background: #111;
      color: #fff;
      padding: 10px 0;
      box-sizing: border-box;
    }

    .banner {
      display: inline-block;
      white-space: nowrap;
      animation: scroll-left 12s linear infinite;
      font-family: Arial, sans-serif;
      font-size: 1.2rem;
    }

    @keyframes scroll-left {
      0% {
        transform: translateX(100%);
      }
      100% {
        transform: translateX(-100%);
      }
    }
  </style>
</head>
<body>
  <div class="banner-wrapper">
    <div class="banner">
     Dm2727+ now available on https://discord.gg/PUXdwWe4U and https://chat.whatsapp.com/D1rlBXvwJVAJnZNFcu1fjX
    </div>
  </div>
</body>
</html>
