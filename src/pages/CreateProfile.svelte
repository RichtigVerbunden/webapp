<script>
  import Button from "@smui/button";
  import { Link, navigate } from "svelte-routing";
  import * as consts from "../constants";
  import regStore from "../stores/registration";
  import userStore from "../stores/user";
  import { createUser } from "../api/user";

  const STEP_PERSONAL = "personal";
  const STEP_NEEDS = "needs";
  const STEP_AVAILIBILITY = "availbility";
  const STEP_INTERESTS = "interests";
  const STEP_FINAL = "final";

  let user = {
    id: "",
    name: "",
    userType: "",
    birthday: new Date().toISOString().split("T")[0],
    sex: "Female"
  };
  regStore.subscribe(r => {
    user = { ...user, userType: r.userType };
  });
  $: flow =
    user.user != consts.TAG_REGTYPE_HELPER
      ? [STEP_PERSONAL, STEP_NEEDS, STEP_INTERESTS, STEP_FINAL]
      : [STEP_PERSONAL, STEP_AVAILIBILITY, STEP_INTERESTS, STEP_FINAL];

  let step = 0;

  const finalize = async () => {
    try {
      let { id, userType, ...input } = user;
      id = await createUser(input);
      if (!id) throw "got invalid response from createUser";
      user.id = id;
      userStore.set(user);
      navigate(consts.PAGE_TUTORIAL);
    } catch (err) {
      alert(err);
    }
  };
</script>

<h2>Profil</h2>

{#if flow[step] === STEP_PERSONAL}
  <h3>Persönliche Daten</h3>
  <div>
    <div class="image input">
      <input type="button" value="aufnehmen" />
      <input type="button" value="hochladen" />
    </div>

    <label for="id">Name:</label>
    <input id="name" type="text" bind:value={user.name} />
    
    <label for="sex">Geschlecht:</label>
    <select>
      <option disabled>Geschlecht</option>
      <option value="Female">Weiblich</option>
      <option value="Male">Männlich</option>
      <option value="Diverse">Diverse</option>
    </select>

    <label for="birthday">Geburtstag:</label>
    <input id="birthday" type="text" bind:value={user.birthday} />

    <label for="email">E-Mail:</label>
    <input id="email" type="text" bind:value={user.email} />

  </div>

{:else if flow[step] === STEP_NEEDS}
  <h3>Deine Bedürfnisse</h3>

  <div>
      <label for="id">Wie stellst du dir deine Gespräche vor?</label>

      <label for="wenigerAls15"> weniger als 15 Minuten</label> 
      <input type="radio" id="mc" name="zeit" value="To15">
      <label for="wenigerAls30"> 15 - 30 Minuten</label> 
      <input type="radio" id="mc" name="zeit" value="To30">
      <label for="mehrAls30"> mehr als 30 Minuten</label> 
      <input type="radio" id="mc" name="zeit" value="Over15">



    <label for="sex">Geschlecht deiner Gesprächsartner</label>
    <ul>
      <li> 
        <label>
          <input type="checkbox" name="partner" value="Female">
          weiblich
        </label>
      </li>
      <li> 
        <label>
           <input type="checkbox" name="partner" value="Male">
           männlich
        </label>
      </li>
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Divers">
          divers
        </label>
      </li>
    </ul> 

  </div>

{:else if flow[step] === STEP_INTERESTS}
  <h3>Deine Interessen</h3>

    <ul>
      <li> 
        <label>
          <input type="checkbox" name="partner" value="Sport">
          Sport
        </label>
      </li>

      <li> 
        <label>
           <input type="checkbox" name="partner" value="Fußball">
           Fußball
        </label>
      </li>

      <li>  
        <label>
          <input type="checkbox" name="partner" value="Reisen">
          Reisen
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Kochen - Lieblingsrezepte">
          Kochen - Lieblingsrezepte
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Spiele">
          Spiele
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Technische Fragen">
          Technische Fragen
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Familie">
          Familie
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Beruf">
          Beruf
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Heimwerk/Hobby">
          Heimwerk/Hobby
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Gesundheit">
          Gesundheit
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Wirtschaft/Gesellschaft">
          Wirtschaft/Gesellschaft
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Musik">
          Musik
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Theater">
          Theater
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Kunst">
          Kunst
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Film und Fernsehen">
          Film und Fernsehen
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Literatur">
          Literatur
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Wissenschaft">
          Wissenschaft
        </label>
      </li>
      
      <li>  
        <label>
          <input type="checkbox" name="partner" value="Religion + Spiritualität">
          Religion + Spiritualität
        </label>
      </li>
    </ul>

{:else if flow[step] === STEP_AVAILIBILITY}
  <h3>Verfügbarkeit</h3>

{:else if flow[step] === STEP_INTERESTS}
  <h3>Interessen</h3>

{:else}
  <h3>Übersicht</h3>
  
{/if}

  <Button on:click={() => step--}>Zurück</Button>
{#if flow[step] == STEP_FINAL}
  <Button on:click={finalize}>Weiter</Button>
{:else}
  <Button on:click={() => step++}>Weiter</Button>
{/if}
