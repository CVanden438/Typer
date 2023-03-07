<script lang="ts">
  import { onMount } from "svelte";

  let words = ["this","is","the","thing","that","you","need","to","type","this","is","the","thing","that","you","need","to","type","this","is","the","thing","that","you","need","to","type","this","is","the","thing","that","you","need","to","type"]
  let typed = []
  $: wordsTest = words.join("")
  let spaceCount = 0
  let input = ""
  $: length = input.length
  $: currentWord = words[spaceCount]
  const handleKeyDown = (e) => {
    if(e.code === "Space"){
      e.preventDefault()
    }
  }
  const onKeyDown = (e) => {
    if(e.code==="Space"){
      spaceCount++
      typed = [...typed,input]
      input = ""
    }
  }
  onMount(()=>{
    document.addEventListener("keydown",onKeyDown)
  })
  $: console.log(typed)
  const reset = () => {
    typed = []
    input = ""
    spaceCount = 0
  }
</script>

<main>
  <div class="paragraph-container">
    {#each words as word, i}
    <div class="word-container">
      {#each word as letter, j}
        <letter class={`${i<spaceCount? typed[i][j]===words[i][j]?"success":"error" : j<length && i<=spaceCount? input[j]===words[i][j]? "success":"error":""}`}>{letter}</letter>
      {/each}
    </div>
    {/each}
    <!-- <div class="caret" style="--length:{length}"></div> -->
    <!-- {#each wordsTest as letter,i}
      <letter class={`${i<length? input[i]===wordsTest[i]?"success":"error":""}`}>{letter}</letter>
    {/each} -->
  </div>
  <input type="text" placeholder="type here" on:keydown={handleKeyDown} bind:value={input}>
  <p>length: {length}</p>
  <p>word: {spaceCount+1}</p>
  <button on:click={reset}>RESET</button>
</main>

<style>
  main{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }
.paragraph-container{
  display: flex;
  gap: 1rem;
  justify-content: center;
  font-size: 2rem;
  font-weight: bolder;
  position: relative;
  border: 1px solid white;
  width: 50rem;
  flex-wrap: wrap;
  margin-bottom: 5rem;
}
input{
  width: 50vw;
  height: 2rem;
  font-size: larger;
  padding: 1rem;
  opacity: 100;
}

.success{
  color: green
}
.error{
  color: red;
}
.caret {
  width: 2px;
  height: 2rem;
  background-color: yellow;
  animation: blink-caret 1s step-start infinite;
  position: absolute;
  left: calc( var(--length)*19.2px);
}

@keyframes blink-caret {
  50% {
    opacity: 0;
  }
}
</style>