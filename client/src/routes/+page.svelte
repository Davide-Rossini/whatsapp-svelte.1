<script lang="ts">
  import { onMount } from 'svelte';
  import { idStore } from "../services/id";
  import { sendMessage } from "../socket";
  import { messagesStore } from "../state/messages";
  import Whatsapp from "../routes/WhatsApp_icon.png";
  import Background_chat from "../routes/background-chat.jpg";
  import Image from "../routes/barra-applicazioni.png";
  import X from "../routes/x.png";
  import Mic from "../routes/mic.png";
  import Emoji from "../routes/emoji.png";
  import Profilepicture from "../routes/profilepicture.png";
  import Greendot from "../routes/greendot.png";
  import Videocall from "../routes/videocall.png";
  import Call from "../routes/call.png";
  import Search from "../routes/search.png";
  import Endtoend from "../routes/endtoend.png";
  import { afterUpdate, onDestroy } from 'svelte';

  let messageInput = "";
  let imageClicked = false;
  let xClicked = false;

  function handleSubmit() {
    if ($idStore) {
      sendMessage(messageInput, $idStore);
    }

    messageInput = "";
  }

  function handleImageClick() {
    imageClicked = true;
  }

  function handleXClick() {
    xClicked = true;
    imageClicked = false;
  }

  let listRef: HTMLElement | undefined;

  $: {
    console.log("hello?");

    if (listRef) {
      listRef.scrollTo({
        behavior: "smooth",
        top: 0,
      });
    }
  }
</script>

<svelte:head>
  <meta charset="utf-8" />
  <link rel="icon" href="%sveltekit.assets%/favicon.png" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
</svelte:head>

{#if imageClicked}
    <header draggable="false">
    <img src={X} alt="Xcliccabile" class="x" draggable="false" unselectable="on" on:click={handleXClick}>
    <img src={Whatsapp} alt="WhatsApp Logo" class="logo" draggable="false" unselectable="on">
    <h1 draggable="false">Whatsapp Svelte</h1>
    </header>
  <main>
    <div class="informazioni">
      <img src={Profilepicture} id="skerdi-img" alt="fotoprofilo" draggable="false" unselectable="on">
      <h2>Utente</h2>
      <img src={Call} alt="call" class="strumenti" id="call" draggable="false" unselectable="on">
      <img src={Videocall} alt="videocall" class="strumenti" id="videocall" draggable="false" unselectable="on">
      <img src={Search} class="strumenti" alt="search" id="search" draggable="false" unselectable="on">
      <img src={Greendot} alt="greendot" id="greendot" draggable="false" unselectable="on">
      <h3>Online</h3>
    </div>
    <div class="end">
      <img src={Endtoend} alt="endtoend" id="endtoend" unselectable="on">
    </div>
    <div class="list-wrapper">
      <ul bind:this={listRef}>
        {#each $messagesStore.reverse() as message}
          <li class="message-wrapper">
            {#if message.userid === $idStore}
              <div />
            {/if}

            <p class="message" class:user-message={message.userid === $idStore}>
              {message.content}
            </p>

            {#if message.userid !== $idStore}
              <div />
            {/if}
          </li>
        {/each}
      </ul>
    </div>

    <form class="input-container" on:submit|preventDefault={handleSubmit}>
      <img src={Emoji} alt="emoji" class="icone" draggable="false" unselectable="on" id="emoji">
      <input type="text" placeholder="Scrivi un messaggio" bind:value={messageInput} />
      <button type="submit">Invia</button>
      <img src={Mic} alt="mic" class="icone" draggable="false" unselectable="on" id="mic">
    </form>
  </main>
{:else}
  <div class="container" on:click={handleImageClick}>
    <img src={Image} alt="barra" id="barra" unselectable="on">
  </div>
{/if}


<style>
  @import url('https://fonts.googleapis.com/css?family=Roboto:400,500|Helvetica+Neue');


body,h1,h2,h3 {
  font-family: 'Helvetica Neue', 'Roboto', sans-serif;
}


body {
  background-color: white;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center; /* Centra gli elementi in modo verticale */
  width: 100%;
}

header {
  width: 100%; /* Imposta la larghezza al 100% */
  max-width: 560px; /* Imposta la larghezza massima per centrare l'header */
  height: 50px; /* Riduci l'altezza se necessario */
  display: flex;
  align-items: center;
  background-color: #075E54;
  color: white;
  padding: 0 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.5); /* Aggiunge un'ombra per un effetto rialzato */
  border-radius: 20px 20px 0 0; /* Angoli arrotondati solo nella parte superiore */
  margin: 5px auto 0px; /* Centra l'header orizzontalmente */
  z-index: 1000; /* Assicurati che l'header sia sopra gli altri contenuti */
  position: relative; /* Necessario per il z-index */
  animation: fadeIn 1s;
}
@keyframes fadeIn {
    from {
      opacity: 0; /* Parti dall'opacità 0 */
    }
    to {
      opacity: 1; /* Arriva all'opacità 1 */
    }
  }

  /* Definisci l'animazione fadeOut */
  @keyframes fadeOut {
    from {
      opacity: 1;
    }
    to {
      opacity: 0;
    }
  }
main {
  display: flex;
  flex-direction: column;
  height: calc(100vh - 60px);
  max-width: 600px; /* Imposta la larghezza massima per centrare il contenuto */
  border-radius: 0 0 20px 20px;/* Angoli arrotondati */
  overflow: hidden; /* Nasconde gli overflow per mantenere i bordi arrotondati */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.5); /* Aggiunge un'ombra per un effetto rialzato */
  /* z-index: 1; */
  margin: 0 auto 10px; /* Centra l'header orizzontalmente */
  animation: fadeIn 1s;

}

.end{
  align-items: center;
  text-align: center;
  background-color: #f3f3f3;
}

#endtoend{
  margin-top: 10px;
}

.informazioni{
  display: flex;
  background-color: #ebebeb;
}

.strumenti{
  height: 25px;
  margin-left: 10px;
  margin-top: 15px;
  margin-bottom: 10px;
}

#call{
  position: absolute;
  margin-left: 450px;
  margin-top: 20px;
  margin-bottom: 10px;
}

#videocall{
  position: absolute;
  margin-left: 500px;
  margin-top: 20px;
  margin-bottom: 10px;
}

#search{
  position: absolute;
  margin-left: 550px;
  margin-top: 20px;
  margin-bottom: 10px;
}


#greendot{
  position: absolute;
  margin-top: 48px;
  margin-left: 80px;
}

#skerdi-img{
  border-radius: 100px;
  height: 50px;
  margin-left: 15px;
  margin-top: 10px;
  margin-bottom: 10px;
}

h2{
  margin-left: 15px;
  margin-top: 10px;
  font-size: 25px;
}

h3{
  position: absolute;
  margin-top: 40px;
  margin-left: 90px;
}


h1{
  pointer-events: none;
  user-select: none;
}


/*   header {
height: 60px;
display: flex;
align-items: center;
background-color: #075E54;
color: white;
padding: 0 20px;
pointer-events: none;
user-select: none;
} */


.logo {
height: 40px;
margin-right: 0px;
pointer-events: none;
user-select: none;
margin-left: 0px;
}

.x{
  height: 10px;
  margin-right: 100px;
  margin-left: 0px;
}


/* main {
display: flex;
flex-direction: column;
height: calc(100vh - 60px);
}
 */

.list-wrapper {
flex-grow: 1;
overflow-y: auto;
background-color: #f3f3f3;
padding: 10px;
}


ul {
list-style: none;
padding: 0;
margin: 0;
}


.message-wrapper {
display: flex;
margin-bottom: 10px;
}

#mic{
  margin-left: 10px;
  height: 20px;
  margin-top: 7px;
}



#emoji{
  height: 20px;
  margin-top: 7px;
}

.message {
padding: 8px 10px;
max-width: 60%;
border-radius: 7.5px;
font-size: 14px;
color: #fff;
word-wrap: break-word;
font-family: 'Helvetica Neue', 'Roboto', sans-serif;

}


.user-message {
background-color: #005D4B;
margin-left: auto;
border-radius: 7.5px 7.5px 0 7.5px;
font-family: 'Helvetica Neue', 'Roboto', sans-serif;

}


.message:not(.user-message) {
background-color: #1F2C34;
color: #ffffff;
border-radius: 7.5px 7.5px 7.5px 0;
font-family: 'Helvetica Neue', 'Roboto', sans-serif;

}


.input-container {
display: flex;
justify-content: center;
padding: 10px;
background-color: #ebebeb;
transform: scale(1.1);
}


input[type="text"] {
width: 70%;
max-width: 300px;
padding: 10px;
border: none;
border-radius: 20px;
margin: 0 10px;
}


button {
padding: 10px 20px;
background-color: #075E54;
color: white;
border: none;
border-radius: 20px;
cursor: pointer;
}


</style>
