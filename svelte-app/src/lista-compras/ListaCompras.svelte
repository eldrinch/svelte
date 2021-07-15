<script>
import ListaItem from "./ListaItem.svelte"


let itens = JSON.parse(localStorage.getItem("lista-compras" )) ?? [];

//[{ comprado: false,
//     descricao: "Pão de forma",
// },{
//     comprado: false,
//     descricao: "Manteiga"
// }];

//Declarando uma variavel que vai ser reativa, recomputada, quando un dos seus valores seja modificado $ = react declarations
$: itensPendentes = itens.filter(item =>!item.comprado).length;

$: { 
    localStorage.setItem("lista-compras", JSON.stringify(itens))
    console.log("itens foi modificado: " + itens.length);
}

//let descricao=""
let inputEl = null;
//function handleInput(e){
//console.log(e.target.value);
//descricao = e.target.value;
//}

//let id = 0;
function adicionarItem(){
//  e.preventDefault();
    itens = [...itens, {
            comprado:false,
            // descricao: "Item " + ++id
            //descricao: descricao,
            descricao: inputEl.value
        }
    ]
//   descricao="";
inputEl.value = "";
}

function removerItem(item){
    itens = itens.filter(i => i !== item)
inputEl.value = "";
}



</script>

<h2>Lista de compras</h2>
<!-- <input value={descricao} on:input={handleInput} > -->
<!-- <input value={descricao}> -->
<form on:submit|preventDefault={adicionarItem}>
<input bind:this={inputEl}>
<!-- <button on:click={adicionarItem} >Adicionar item</button> -->
<button type="submit">Adicionar item</button>
</form>

{#if itens.length ===0}
  <div>A lista está vazia</div>
{:else}
    {#each itens as item, i}
<ListaItem
 bind:comprado={item.comprado}
 descricao={item.descricao}
 on:itemremovido={()=>removerItem(item)}
 />

  <!-- <div>{i+1} : {item.descricao}</div> -->
  <!-- <div>
    <label for="">
      <input type="checkbox" bind:checked={item.comprado}>
      {item.descricao}
    </label>
  </div> -->
    {/each}
{/if}

<!-- <div>Itens Pendentes: {itens.filter(item =>!item.comprado).length}</div> -->
<div>Itens Pendentes: {itensPendentes}</div>

