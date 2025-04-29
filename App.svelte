<script lang="ts">
  import { onMount } from "svelte";
  import svelteLogo from "./assets/svelte.svg";
  import viteLogo from "/vite.svg";
  import Counter from "./lib/Counter.svelte";

  let apiKey: string = "";
  let currentDate: string = "";

  onMount(async () => {
    const now = new Date();
    currentDate = now.toLocaleDateString();

    try {
      const res = await fetch("/api/key");
      const data = await res.json();
      apiKey = data.apiKey;
      const nytUrl =
        "https://api.nytimes.com/svc/search/v2/articlesearch.json?q=sacramento+davis&api-key=" +
        apiKey;
      fetch(nytUrl).then(statusChecker).then(dataHandler);
    } catch (error) {
      console.error("Failed to fetch API key:", error);
    }
  });

  function statusChecker(response: Response) {
    if (response.status === 200) {
      // console.log("Response is OK:", response);
      // console.log("Response status:", response.json());
      return response.json();
    } else {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
  }

  function dataHandler(data: any) {
    const articles = data.response.docs;
    console.log("Articles fetched:", articles);
    const firstArticle = articles[0];
    const secondArticle = articles[1];
    const thirdArticle = articles[2];

    //get the first three articles headlines
    const firstArticleHeading = document.querySelector("#firstArticle h2");
    if (firstArticleHeading) {
      firstArticleHeading.textContent = firstArticle.headline.main;
    }

    const secondArticleHeading = document.querySelector("#secondArticle h2");
    if (secondArticleHeading) {
      secondArticleHeading.textContent = secondArticle.headline.main;
    }

    const thirdArticleHeading = document.querySelector("#thirdArticle h2");
    if (thirdArticleHeading) {
      thirdArticleHeading.textContent = thirdArticle.headline.main;
    }

    //get the first three articles leading paragraphs
    const firstArticleLeadingParagraph =
      document.querySelector("#firstArticle p");
    if (firstArticleLeadingParagraph) {
      firstArticleLeadingParagraph.textContent =
        firstArticle.abstract || firstArticle.snippet;
    }

    const secondArticleLeadingParagraph =
      document.querySelector("#secondArticle p");
    if (secondArticleLeadingParagraph) {
      secondArticleLeadingParagraph.textContent =
        secondArticle.abstract || secondArticle.snippet;
    }

    const thirdArticleLeadingParagraph =
      document.querySelector("#thirdArticle p");
    if (thirdArticleLeadingParagraph) {
      thirdArticleLeadingParagraph.textContent =
        thirdArticle.abstract || thirdArticle.snippet;
    }

    //get the first three articles imges
    const firstArticleImage = document.querySelector(
      "#firstArticle img",
    ) as HTMLImageElement;
    if (firstArticleImage) {
      firstArticleImage.id = "firstArticleImage";
      firstArticleImage.src = firstArticle.multimedia.default.url;
      firstArticleImage.alt = firstArticle.headline.main;
      firstArticleImage.width = 250;
      firstArticleImage.height = 250;
    }

    const secondArticleImage = document.querySelector(
      "#secondArticle img",
    ) as HTMLImageElement;
    if (secondArticleImage) {
      secondArticleImage.id = "secondArticleImage";
      secondArticleImage.src = secondArticle.multimedia.default.url;
      secondArticleImage.alt = secondArticle.headline.main;
      secondArticleImage.width = 250;
      secondArticleImage.height = 250;
    }

    const thirdArticleImage = document.querySelector(
      "#thirdArticle img",
    ) as HTMLImageElement;
    if (thirdArticleImage) {
      thirdArticleImage.id = "thirdArticleImage";
      thirdArticleImage.src = thirdArticle.multimedia.default.url;
      thirdArticleImage.alt = thirdArticle.headline.main;
      thirdArticleImage.width = 250;
      thirdArticleImage.height = 250;
    }
    // Set the first article link
    const firstArticleLink = document.querySelector(
      "#firstArticleLink",
    ) as HTMLAnchorElement;
    if (firstArticleLink) {
      firstArticleLink.href = firstArticle.web_url; // Set the article's URL
    }

    const secondArticleLink = document.querySelector(
      "#secondArticleLink",
    ) as HTMLAnchorElement;
    if (secondArticleLink) {
      secondArticleLink.href = secondArticle.web_url; // Set the article's URL
    }

    const thirdArticleLink = document.querySelector(
      "#thirdArticleLink",
    ) as HTMLAnchorElement;
    if (thirdArticleLink) {
      thirdArticleLink.href = thirdArticle.web_url; // Set the article's URL
    }
  }

  function redirectToNYT() {
    window.location.href = "https://www.nytimes.com/";
  }
</script>

<main>
  <div class="grid_container">
    <div class="head">
      <p id="date">{currentDate}</p>
      <h1>The New York Times</h1>
      <hr />
    </div>
    <div id="firstArticle">
      <a id="firstArticleLink" target="_blank" rel="noopener noreferrer">
        <h2></h2>
        <p></p>
      </a>
      <img />
    </div>

    <div id="secondArticle">
      <a id="secondArticleLink" target="_blank" rel="noopener noreferrer">
        <h2>secondArticle</h2>
      </a>
      <img />
      <a id="secondArticleLink" target="_blank" rel="noopener noreferrer">
        <p></p>
      </a>
    </div>

    <div id="thirdArticle">
      <a id="thirdArticleLink" target="_blank" rel="noopener noreferrer">
        <h2>thirdArticle</h2>
        <p></p>
      </a>
      <img />
    </div>
    <hr id="last" />
  </div>

  <div style="text-align: center;">
    <button id="button" on:click={redirectToNYT}
      >Clik me to The New York Times</button
    >
  </div>
</main>

<style>
  main {
    margin-left: 5vw;
    margin-right: 5vw;
    padding: 0;
    font-family: "Georgia", serif;
  }

  .grid_container {
    display: grid;
    gap: 10px;
    grid-template-columns: repeat(6, 1fr);
  }

  .head {
    margin: 0px;
  }
  h1 {
    text-align: center;
    font-size: 60px;
    font-family: "Chomsky", serif;
    font-weight: bold;
    margin-top: 0px;
    margin-bottom: 0px;
  }

  #date {
    font-family: "Helvetica", sans-serif;
    font-size: 16px;
    color: #8f8a8a;
  }

  hr {
    /* horizontal line to separate the head and content*/
    width: 100%;
    height: 0.2vw;
    background-color: #666;
    border: none;
    margin-top: 1vw;
  }

  #firstArticle,
  #secondArticle,
  #thirdArticle {
    text-align: center;
  }

  @media only screen and (max-width: 768px) {
    #last {
      display: none;
    }

    #firstArticle,
    #secondArticle,
    #thirdArticle {
      border-bottom: 2px solid #939191;
      padding-bottom: 1.5vw;
    }
    .head {
      grid-area: 1 / span 6;
    } /* put each content in a single row*/
    #firstArticle {
      grid-area: 2 / span 6;
    }
    #secondArticle {
      grid-area: 3 / span 6;
    }
    #thirdArticle {
      grid-area: 4 / span 6;
    }
  }

  #last {
    width: 100%;
    height: 1px;
    background-color: #ccc;
  }

  @media only screen and (min-width: 768px) {
    #last {
      display: block;
    }
    #firstArticle {
      border-bottom: 1px solid #ccc;
      padding-bottom: 1.5vw;
    }
    #thirdArticle {
      border-left: 1px solid #ccc;
      padding-left: 1.5vw;
    }

    .head {
      grid-area: 1 / span 6;
    } /* put the head in a single row*/
    #firstArticle {
      grid-area: 2 / span 6;
    } /* put the firstArticle alone*/
    #secondArticle {
      grid-area: 3 / span 3;
    } /* put the secondArticle and thirdArticle in a single row and each takes 1/2 of the row to form 2 columns*/
    #thirdArticle {
      grid-area: 3 / span 3;
    }
    #last {
      grid-area: 4 / span 6;
    }
  }

  @media only screen and (min-width: 1024px) {
    #secondArticle,
    #thirdArticle {
      border-bottom: 1px solid #cccccc00;
      border-left: 1px solid #ccc;
      padding: 0 1.5vw;
    }
    #firstArticle,
    #secondArticle,
    #thirdArticle {
      border-bottom: 1px solid #cccccc00;
    }
    .head {
      grid-area: 1 / span 6;
    } /* put the head in a single line*/
    #firstArticle {
      grid-area: 2 / span 2;
    }
    #secondArticle {
      grid-area: 2 / span 2;
    } /* put the firstArticle, secondArticle and thirdArticle in a single row and each takes 1/3 of the row and then will form 3 columns*/
    #thirdArticle {
      grid-area: 2 / span 2;
    }
    #last {
      grid-area: 3 / span 6;
    }
  }

  #button {
    background-color: #04aa6d;
    border: 2px solid #04aa6d;
    transition-duration: 0.4s;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
  }

  #button:hover {
    background-color: #ffffff;
    color: green;
    transition-duration: 0.4s;
  }
  a {
    text-decoration: none;
    color: black;
  }

  a:hover h2 {
    text-decoration: none;
    color: gray;
  }
</style>
