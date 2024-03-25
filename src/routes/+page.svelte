<script>
    // import { onMount } from 'svelte';

    // Define your Svelte component
    let leaderboardData = '';
    let username = [];
    let compleations = [];
    let time = [];
    let skinId = [];
    let submited = false;
    let SortByComplete = false;
    let levelID = 'forest';

    // Function to fetch data from the API
    async function fetchLeaderboardData() {
        const data = {
            mapId: levelID,
            tokenId: null,
            isSortedByCompletion: SortByComplete,
        };

        const options = {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(data),
        };

        try {
            const response = await fetch('https://icedodo-api.onionfist.com/api/get_map_leaderboard', options)
            .then(Response => Response.json())
            .then(data => {
                leaderboardData = data.leaderboardRows;
                // leaderboardData = data.leaderboardRows;
                // leaderboardRows = leaderboardData.leaderboardRows
                console.log(leaderboardData);
                // make a loop that repeats for leaderboardData.length
                for (let i = 0; i < leaderboardData.length; i++) {
                    username.push(leaderboardData[i].username);
                    compleations.push(leaderboardData[i].completions);
                    time.push(leaderboardData[i].time);
                    skinId.push(leaderboardData[i].skinId);
                }
                // console.log(username);
                // console.log(compleations);
                // console.log(time);
                // console.log(skinId);
            });

        } catch (error) {
            console.error('Error fetching leaderboard data:', error);
        }
    }
    // onMount(fetchLeaderboardData);
    let getData;
    function submit() {
        if (levelID == '') {
            alert('Please enter a level ID');
            return;
        }
        if (levelID != '')
        {
            submited = true;
            getData = fetchLeaderboardData();
        }
    }
</script>
{#if !submited}
    Enter Level ID:<input bind:value={levelID}><br>
    Sort By completion: <input type="checkbox" bind:checked={SortByComplete}><br>
    <button on:click={submit}>submit</button>
{/if}
{#if submited}
{#await getData}
<p>...waiting</p>
{:then}
<div id="navbar">
    <div></div>
</div>
<center><div id="body">
    

    <main style="border: 2px red solid;">
    <div id="header">
        <h2>Ranking</h2>
        <h2 style="position:relative;left:-5%;">Completion</h2>
        <h2>Time</h2>
    </div>
    <div id="leaderboard">
    <div class="ribbon"></div>
    <table>
        {#each username as name, i}
            <tr>
            <td class="number">{i+1}</td>
            <td class="name">{name}</td>
            <td class="name">{compleations[i]}</td>
            <td class="points">{time[i]}
                {#if i == 0}
                    <img class="gold-medal" src="https://github.com/malunaridev/Challenges-iCodeThis/blob/master/4-leaderboard/assets/gold-medal.png?raw=true" alt="gold medal"/>
                {/if}
            </td>
            </tr>
        {/each}
    </table>
    </div>
</main></div></center>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
{/if}

<div style="margine:auto;position:absolute;top:90%;">
    <p>Ice Dodo by Onionfist (<a href="onionfist.com">onionfist.com</a>)</p>
    <p>Website by Guy</p>
    <a href="https://github.com/GuySandler/Ice-Dodo-Leaderboard">Github</a>
</div>

<style>
    * {
        font-size: 62, 5%;
        box-sizing: border-box;
        margin: 0;
    }

    #body {
        height: 100%;
        width: 100%;
        background-color: #fbfaff;
        display: flex;
        align-items: center;
        justify-content: center;
        border: 1px blue solid;
        position: absolute;
        top:0;
        left:0;

    }

    main {
        margin-top: 5%;
        width: 40rem;
        height: 75%;
        background-color: #ffffff;
        -webkit-box-shadow: 0px 5px 15px 8px #e4e7fb;
        box-shadow: 0px 5px 15px 8px #e4e7fb;
        display: flex;
        flex-direction: column;
        align-items: center;
        position: absolute;
        top:2.50%;
        border-radius: 0.5rem;
    }

    #header {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 2.5rem 2rem;
        background-color: rgb(204, 204, 204);
        z-index: 1;
        /* filter: drop-shadow(10px 10px 10px black); */
        /* make a stright down drop shadow that goes for 50 px */
        box-shadow: 0px 5px 15px 8px #a7a7a7;
        border-radius: .5rem 0.5rem 0 0;
    }

    h1 {
        font-family: "Rubik", sans-serif;
        font-size: 1.7rem;
        color: #141a39;
        text-transform: uppercase;
        cursor: default;
    }
    h2 {
        font-family: "Rubik", sans-serif;
        font-size: 1.7rem;
        color: #141a39;
        text-transform: uppercase;
        cursor: default;
    }
    p {
        font-family: "Rubik", sans-serif;
        /* font-size: 1.7rem; */
        color: #141a39;
        /* text-transform: uppercase; */
        cursor: default;
    }

    #leaderboard {
        width: 100%;
        position: relative;
        overflow-y: auto;
        overflow-x: hidden;
    }

    table {
        width: 100%;
        border-collapse: collapse;
        table-layout: fixed;
        color: #141a39;
        cursor: default;
    }

    tr {
        transition: all 0.2s ease-in-out;
        border-radius: 0.2rem;
    }

    tr:not(:first-child):hover {
        background-color: #fff;
        transform: scale(1.1);
        -webkit-box-shadow: 0px 5px 15px 8px #e4e7fb;
        box-shadow: 0px 5px 15px 8px #e4e7fb;
    }

    tr:nth-child(odd) {
        background-color: #f9f9f9;
    }

    tr:nth-child(1) {
        color: #fff;
    }

    td {
        height: 5rem;
        font-family: "Rubik", sans-serif;
        font-size: 1.4rem;
        padding: 1rem 2rem;
        position: relative;
    }

    .number {
        width: 1rem;
        font-size: 2.2rem;
        font-weight: bold;
        text-align: left;
    }

    .name {
        text-align: left;
        font-size: 1.2rem;
    }

    .points {
        font-weight: bold;
        font-size: 1.3rem;
        display: flex;
        justify-content: flex-end;
        align-items: center;
    }

    .points:first-child {
        width: 10rem;
    }

    .gold-medal {
        height: 3rem;
        margin-left: 1rem;
    }

    .ribbon {
        width: 42rem;
        height: 5.5rem;
        top: -0.5rem;
        background-color: #5c5be5;
        position: absolute;
        left: -1rem;
        -webkit-box-shadow: 0px 15px 11px -6px #7a7a7d;
        box-shadow: 0px 15px 11px -6px #7a7a7d;
    }

    .ribbon::before {
        content: "";
        height: 1.5rem;
        width: 1.5rem;
        bottom: -0.8rem;
        left: 0.35rem;
        transform: rotate(45deg);
        background-color: #5c5be5;
        position: absolute;
        z-index: -1;
    }

    .ribbon::after {
        content: "";
        height: 1.5rem;
        width: 1.5rem;
        bottom: -0.8rem;
        right: 0.35rem;
        transform: rotate(45deg);
        background-color: #5c5be5;
        position: absolute;
        z-index: -1;
    }

    @media (max-width: 740px) {
        * {
        font-size: 70%;
        }
    }

    @media (max-width: 500px) {
        * {
        font-size: 55%;
        }
    }

    @media (max-width: 390px) {
        * {
        font-size: 45%;
        }
    }
    #navbar {
        width: 100%;
        height: 5rem;
        background-color: #5c5be5;
        position: absolute;
        top: 0;
        left: 0;
        z-index: 1;
    }
</style>