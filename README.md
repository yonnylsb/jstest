# jstest

function renderCard({id, ename, base}) {
    return `
        <div class="pokecard">
            <h1>${id}</h1>
            <p class="name">${ename}</p>
            <p>${getAttribute(base)}</p>
        </div>
    `
};

function getAttribute(base) {
    let Attributes = [];

    for(let attribute in base){
        Attributes.push(
            `
            <p><span class="attr">${attribute}</span>:<span>${base[attribute]}</span></p>
            `
        );
    }

    return Attributes.join('');
}

function main(data){
    let app = document.querySelector('.app'),
        sortDataByName = sortByName(data),
        groupPokeByLetter  = groupByLetter(sortDataByName),
        cardsByGroup = renderByGroups(groupPokeByLetter);
        
    // app.innerHTML = sortDataByName.map(renderCard).join('');
    app.innerHTML = renderAllByGroups(cardsByGroup);
}

function renderAllByGroups(groups){
    let allGroups = groups.map((group)=>{
        return `
            <section class="group">
                ${group}
            </section>
        `;
    });

    return allGroups.join('');
}

function renderByGroups(groups) {
    let renderedGroups = [];

    for(let group in groups){
        let  groupOfCards = groups[group].map((pokemon)=>renderCard(pokemon)).join('');
        renderedGroups.push(groupOfCards);
    }
    return renderedGroups;
}

function sortByName(data){
    let sortedData = data.slice().sort((pokeA, pokeB)=>{
        return ('' + pokeA.ename).localeCompare(pokeB.ename);
    });
    return sortedData;
}

function groupByLetter(data) {
    let groupByLetter = data.reduce((groupedByName, poke)=>{
        let letter = poke.ename[0]; 
        (groupedByName[letter] = groupedByName[letter] || []).push(poke)
        return  groupedByName;
    }, {});

    return groupByLetter;
}

fetch('https://raw.githubusercontent.com/fanzeyi/Pokemon-DB/master/pokedex.json')
    .then(response => {
        return response.json();
    })
    .then(data => {
        main(data);
        return true;
    })
    .catch((e)=>console.warn(e));
    
    
