<script>
import moment from "moment";

// Define some default state for our form.
const person = {
  name: "",
  birthdate: ""
};
var personalInfoErrors = [];

function validatePersonalInfoForm() {
  // Reset validation errors on each submit.
  personalInfoErrors = [];

  if(person.name.length === 0) {
    personalInfoErrors.push("Name is required");
  }

  // We want to check if a value for birthdate is present *before* we try to also parse and validate it.
  if(person.birthdate.length === 0) {
    personalInfoErrors.push("Birthdate is required");
  } else {
    const birthdate = moment(person.birthdate, "YYYY-MM-DD");
    if(!birthdate.isValid()) {
      personalInfoErrors.push("Birthdate is invalid");
    }
  }
}

// Handle the submission for the personal information form.
function handlePersonalInfoSubmission(e) {
  validatePersonalInfoForm();

  // Do things now that our form data has been validated.
  if(!personalInfoErrors.length) {
    alert("Submitted!");
  }
}
</script>

<div class="personal-info-form-errors" class:has-errors="{personalInfoErrors.length}">
  {#each personalInfoErrors as error}
    <div class="error">{error}</div>
  {/each}
</div>

<form on:submit|preventDefault={handlePersonalInfoSubmission}>
  <label for="name">Name
    <input type="text" id="name" name="name" bind:value={person.name}>
  </label>
  <label for="birthdate">Birthdate
    <input type="text" id="birthdate" name="birthdate" bind:value={person.birthdate} placeholder="2002-02-15">
  </label>
  <button>Save</button>
</form>

<style>
.personal-info-form-errors {
  display: none;
  color: red;
  margin: 1em;
}

.personal-info-form-errors.has-errors {
  display: block;
}
</style>
