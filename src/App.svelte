<script>
  export let name;
  let changed = false;
  console.log('here');

  async function handleClick() {
    changed = !changed;
    const [tab] = await chrome.tabs.query({
      active: true,
      currentWindow: true,
    });
    await chrome.storage.local.set({ tab });

    chrome.scripting.executeScript({
      target: { tabId: tab.id },
      func: async () => {
        // document.body.style.backgroundColor = 'red';
        const tab = await chrome.storage.local.get('tab');
        console.log(tab);
      },
    });
  }
</script>

<main class:bg={changed}>
  <h1>Hello {name}!</h1>
  <p>Visit the svelte</p>
  <button on:click={handleClick}>Click me</button>
</main>

<style>
  main {
    text-align: center;
    padding: 0em;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  .bg {
    background-color: #daff0a;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
