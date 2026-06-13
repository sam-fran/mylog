<script>
    let items = $state([]);
    let inputValue = $state("");

    try {
        const saved = localStorage.getItem("listcontents");
        if (saved) {
            items = JSON.parse(saved);
        }
    } catch {}

    $effect(() => {
        localStorage.setItem("listcontents", JSON.stringify(items));
    });

    function addItem(event) {
        event.preventDefault();
        items.unshift(getLogTimestamp() + " - " + inputValue);
        inputValue = "";
    }

    function deleteAll() {
        items = [];
    }

    function getLogTimestamp() {
        const now = new Date();
        const year = now.getFullYear();
        const month = now.getMonth() + 1;
        const day = now.getDate();
        const hours = now.getHours();
        const minutes = String(now.getMinutes()).padStart(2, "0");
        const seconds = String(now.getSeconds()).padStart(2, "0");

        return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
    }
</script>

<main>
    <h1>My Log</h1>
    <!-- Use the event handler here -->
    <form onsubmit={addItem}>
        <input
            bind:value={inputValue}
            maxlength="100"
            required
            placeholder="Add item..."
        />
        <button type="submit"> Add to list </button>
    </form>

    <ul class="list">
        {#each items as item}
            <li>{item}</li>
        {/each}
    </ul>

    {#if items.length > 0}
        <button onclick={deleteAll}> Delete all </button>
    {/if}
</main>

<style>
    :root {
        background-color: black;
        color: white;
        font-family: "Courier New", Courier, monospace;
    }

    main {
        background-color: rgb(44, 72, 93);
        width: 85vw;
        border-radius: 3em;
        margin: 5vw auto;
        padding: 3%;
        text-align: center;
        box-shadow: 0px 0px 40px rgb(77, 172, 201);
    }

    button {
        color: white;
        background-color: rgb(17, 3, 60);
        padding: 1%;
        border-radius: 2em;
        border: 3px inset grey;
        cursor: grab;
    }

    input {
        color: white;
        background-color: rgb(17, 3, 60);
        padding: 1%;
        border-radius: 2em;
        border: 3px inset grey;
    }

    ul li {
        list-style-type: none;
        margin-top: 1.5em;
    }
</style>
