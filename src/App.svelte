<script>
  import 'bulma/css/bulma.css';
  import { onMount } from 'svelte';
  import { is_empty } from 'svelte/internal';
  
    let entry,entry1;
    let entries = {'arr':[]};
  
    
    
    function create(){

      if((entry!== null) && (entry!=='undefined') && (entry!==""))){

        entries.arr.push(entry);
        localStorage.setItem('entries',JSON.stringify(entries))
        entries=entries; // pretty weird solution to add reactivity to array assing array to array after pushing ( arr = arr;)
        alert('Entry Created');
        entry='';
      }
      else{
        alert('Field is Empty');
      }
    }
  
  //read
    onMount(async () => {
      
      if(localStorage.getItem('entries')!=null){
        entries = JSON.parse(localStorage.getItem('entries'));
      }
     
      
    });
  // read
  
    function update(index){
      entry = entries.arr[index];
      entry1=prompt(entry);
     if (entry1 === null) {
         alert("canclled");
            return; //break out of the function early
        }
else{
      entries.arr[index] =  entry1 ;
      localStorage.removeItem('entries');
      localStorage.setItem('entries',JSON.stringify(entries))
      alert("updated")
}
       
      
    }
    function dele(index){
        if (window.confirm('Do you Really Want to Delete this Entry?'))
        {
            entries.arr.splice(index,1);
            entries.arr = entries.arr;
            localStorage.removeItem('entries');
            localStorage.setItem('entries',JSON.stringify(entries))
            alert('deleted');
        }
        else
        {
            // They clicked no
        }
      
      
    }
  
  
  </script>
  
  <!-- svelte-ignore non-top-level-reactive-declaration -->
  <header class="header is-size-1 has-text-black">
    Basic Crud Using <img style="height:60px;" src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Svelte_Logo.svg/1702px-Svelte_Logo.svg.png"  alt="">
  </header>
  <main>
    <div class="form">
    <input class="input" type="text" bind:value={entry}> <button on:click={create}>Create</button>
  </div>
    <hr>
    <table class="table is-hoverable is-fullwidth">
      <thead>
        <tr>
          <th>
            S.no
          </th>
          <th>
            Entries
          </th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
    {#if entries}
    {#each entries.arr as entry,index }
        
        <tr>
          <td>{index}</td>
          <td>{entry}</td>
          <td><button on:click={()=>update(index)}>update</button>
          <button class="red" on:click={()=>dele(index)}>delete</button></td>
        </tr>
  
        
    {/each}
    {/if}
  </tbody>
  </table>
  
  </main>
  
  <style>
  .red{
    background-color: firebrick;
  }
  </style>
