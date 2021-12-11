<script>

    import {onMount} from 'svelte';

    let name = '';
    let alphabetArray;
    let isAllowed;
    let timer = 30;

    // Forget change of cursor, maybe just clear the input fields

    // Buttons as a store?
    const shuffleArray = (array) => array.map(value => ({value, sort: Math.random()})).sort((a, b) => a.sort - b.sort).map(({value}) => value);

    const handleButtonClick = (value) => {
        name = name+value.target.value;
    }
    const startingLetter = 'A';

    alphabetArray = shuffleArray(
        [   
            ...Array(26).fill(startingLetter).map((value, index) => String.fromCharCode(value.charCodeAt(0)+index)),
            ...['-']
        ]
    );

    onMount(() => {
        setInterval(() => {
            isAllowed = !isAllowed;
            if(!isAllowed) alphabetArray = shuffleArray(alphabetArray);
        }, Math.random()*10000);
    
        setInterval(() => {
            if(timer > 0) {
                timer = timer - 1;
            } else {
                window.alert("Database keys have rotated due to database deflector dish moving. Resetting.");
                name = '';
                timer = 30;
            }
        }, 1000);
    }) 
    
    const mousePositionCheck = () => {
        if(!isAllowed) {
            alert("You moved during red light! Resetting...");
            name = '';
        }
    }

    const handleSubmit = () => {
        const confirmedName = confirm("You submitted 'Loser', is that right?");
        if(confirmedName) {
            alert('You liar. Resetting.');
        } else {
            alert('HAHA... I mean, please try again. Resetting.');
        }
        name = '';
    }
</script>

<style>
#button-container button {
    width: 50%;
    place-self: center;
}

#container {
    width: 100%;
    height: 100vh;
}

#button-container {
    background-color: red;
    width: 50%;
    margin: auto;
    display: grid;
    column-gap: 20px;
    row-gap: 20px;
    grid-template-columns: 1fr 1fr 1fr;
    padding: 20px;
}

#button-container.green {
    background-color: green;
}

#description{
    color: red;
    font-size: 30px;
    font-weight: bolder;

}

#description.green{
    color: green;
}

#input {
    margin: auto;
    width: 50%;
}

#text-container{
    display: flex;
    width: 50%;
    margin: auto;
    justify-content: space-around;
} 

#timer {
    font-size: 30px;
    font-weight: bolder;
}

</style>

<div id="container" >
    <div id="input">
        <label for="name">First Name: </label>
        <input type="text" name="user" id="name" value={name} disabled/>
        &nbsp;
        <button on:click={handleSubmit}>Submit name</button>
    </div>
    <br/>
    <div id="text-container">
        <div id="description" class:green="{isAllowed === true}">{#if isAllowed}GREEN LIGHT{:else}RED LIGHT{/if}</div>
        <div id="timer">Seconds left to enter name: {timer}</div>
    </div>
    <div on:mousemove={mousePositionCheck} class:green="{isAllowed === true}" id="button-container">
        {#each alphabetArray as letter}
            <button value={letter} on:click={handleButtonClick} tabindex="-1">{letter}</button>
        {/each}
    </div>
</div>