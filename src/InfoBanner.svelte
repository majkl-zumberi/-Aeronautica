<script>
  import { _ } from "svelte-i18n";
  import { location, push } from "svelte-spa-router";

  let searchArticle = "";
  $: alreadyInArticles = $location.includes("/articles");
  function redirect() {
    /**
     * tweak del push:
     * apparentemente se l'utente si ritrova già nella pagina di visualizzazione degli articoli
     * la normale push verso la ricerca di un articolo non funziona, perciò prima lo mando es in home poi
     * nella corretta route in maniera asincrona
     *
     * se si trova invece in altre routes la normale push funziona
     */
    if (alreadyInArticles) {
      push("/");
      setTimeout(function () {
        push(
          `/articles/category/nosubcategory/nocategory?articleName=${searchArticle}`
        );
        searchArticle = "";
      }, 0);
    } else {
      push(
        `/articles/category/nosubcategory/nocategory?articleName=${searchArticle}`
      );
      searchArticle = "";
    }
  }
</script>

<section class="text-gray-600 body-font">
  <div class="container px-5 py-16 mx-auto flex items-center flex-wrap">
    <div class="flex flex-col items-center w-full sm:w-full md:w-full lg:w-auto">
      <img
        class=" h-14 w-14"
        src="/images/Logo-Kimpex.png"
        alt="Workflow"
      />
      <img
      class="block h-14 w-auto"
      src="/images/AvioKimpexsrl.png"
      alt="kimpex logo"
    />
    </div>
    <div class="lg:w-2/5 lg:pl-6 w-full">
      <p class="leading-relaxed text-base">
        {$_("infoBanner.search.insert", {
          default: "inserisci",
        })}
        <span class="font-medium"
          >{$_("infoBanner.search.name", {
            default: "il nome",
          })}</span
        >
        {$_("infoBanner.search.product", {
          default: "del prodotto che desideri ricercare",
        })}
      </p>
      <div class="flex md:mt-4 mt-6">
        <input
          bind:value={searchArticle}
          type="text"
          class="flex-shrink flex-grow flex-auto leading-normal w-px flex-1 border h-10 border-grey-light rounded rounded-r-none px-3 relative focus:border-blue focus:shadow"
          placeholder="es: microphone m87"
        />
        <button
          on:click={redirect}
          disabled={searchArticle == ""}
          class="{searchArticle == ''
            ? 'disabled:opacity-50 disabled:text-black'
            : ''} inline-flex text-white bg-green-400 border-0 py-2 px-4 focus:outline-none hover:bg-green-600 "
          >{$_("infoBanner.button.search", {
            default: "Cerca",
          })}</button
        >
      </div>
    </div>
    <div class="flex  flex-col lg:w-1/3 text-center justify-center w-full">
      <p class="leading-relaxed text-base text-2xl font-bold">
        {$_("infoBanner.assistance.need", {
          default: "bisogno di assistenza?",
        })}
      </p>
      <p class="leading-relaxed text-base ">
        {$_("infoBanner.assistance.callus", {
          default: "non hai trovato il prodotto che cercavi? Chiamaci!",
        })}
      </p>
      <div class="flex flex-col items-center ">
      <p class="leading-relaxed text-base text-2xl font-bold">
        +39 030 2421830
      </p>
      <p class="leading-relaxed text-xs font-normal">o</p>  
      <p class="leading-relaxed text-base text-2xl font-bold">
        +39 391 7470911
     </p>
      </div>
    </div>
  </div>
</section>
