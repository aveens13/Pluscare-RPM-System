<script>
  import swal from "sweetalert";
  import { fade, slide } from "svelte/transition";
  let isNumber = true;
  let letterCount = 0;
  let phoneNoValid = "";
  function addInputListener(event) {
    var number = /^[a-zA-Z]*$/;
    if (event.data == null) {
      letterCount -= 1;
      console.log("Backspace");
      isNumber = true;
      // event.target.disabled = false;
    } else if (event.data.match(number)) {
      event.target.value = phoneNoValid;
      letterCount = letterCount + 1;
      console.log(event);
      // event.target.value =
      // event.target.disabled = true;
      isNumber = false;
    } else {
      isNumber = true;
    }
    phoneNoValid = event.target.value;
  }
  async function onSubmit(event) {
    const form = event.currentTarget;
    const formData = new FormData(form);
    const plainFormdata = Object.fromEntries(formData.entries());
    const formDatainJSON = JSON.stringify(plainFormdata);
    const response = await fetch("/api/register-patient", {
      method: "POST",
      headers: {
        "Content-type": "application/json",
        Accept: "application/json",
      },
      credentials: "include",
      body: formDatainJSON,
    });
    if (response.ok) {
      const result = await response.json();
      swal({
        title: "Patient's Username and Password",
        text: `Username: ${result.uid} Password: ${result.userPassword}`,
        icon: "success",
      }).then(() => {
        swal({
          title: "Mail Sent",
          text: `Email sent successfully to ${result.mail}`,
          icon: "success",
        });
      });

      console.log(result);
    } else {
      response.json().then((result) => {
        swal({
          title: "Registration Failed",
          text: result.message,
          icon: "error",
        });
      });
    }
  }
</script>

<main>
  <div class="container" in:fade>
    <header>
      Fields marked with <span class="asterick">&#42;</span> should be filled.
    </header>
    <form
      method="POST"
      action="/api/register-patient"
      on:submit|preventDefault={onSubmit}
    >
      <span class="title">Personal Details</span>
      <div class="fields">
        <div class="input-field">
          <label for="first name"
            >First name<span class="asterick">&#42;</span></label
          >
          <input
            type="text"
            id="first name"
            name="firstName"
            placeholder="First Name"
            required
          />
        </div>
        <div class="input-field">
          <label for="first name">Middle name</label>
          <input
            type="text"
            id="middle name"
            name="middleName"
            placeholder="Middle Name"
          />
        </div>
        <div class="input-field">
          <label for="last name"
            >Last name<span class="asterick">&#42;</span></label
          >
          <input
            type="text"
            id="last name"
            name="lastName"
            placeholder="Last Name"
            required
          />
        </div>
        <!-- Birthdate -->
        <div class="input-field">
          <label for="birthdate"
            >Birthdate<span class="asterick">&#42;</span></label
          >
          <input type="date" id="dob" name="birthDate" required />
        </div>
        <div class="input-field">
          <label for="occupation">Occupation</label>
          <input
            type="text"
            id="occupation"
            name="occupation"
            placeholder="Business"
          />
        </div>
        <div class="input-field">
          <label for="blood"
            >Blood Group<span class="asterick">&#42;</span></label
          >
          <input
            type="text"
            id="blood"
            name="blood"
            placeholder="A+"
            required
          />
        </div>
        <div class="input-field">
          <label for="weight">Weight<span class="asterick">&#42;</span></label>
          <input
            type="text"
            id="weight"
            name="weight"
            placeholder="60Kg"
            required
          />
        </div>
      </div>

      <!-- Gender -->
      <div class="title">Gender<span class="asterick">&#42;</span></div>
      <div class="radios">
        <div class="input-radio">
          <input type="radio" id="male" value="Male" name="gender" />
          <label for="male">Male</label>
        </div>
        <div class="input-radio">
          <input type="radio" id="female" value="Female" name="gender" />
          <label for="female">Female</label>
        </div>
        <div class="input-radio">
          <input type="radio" id="others" value="others" name="gender" />
          <label for="others">Others</label>
        </div>
      </div>
      <div class="title">Contact information</div>
      <div class="contact">
        <div class="input-contact">
          <label for="email">E-mail<span class="asterick">&#42;</span></label>
          <input
            type="email"
            id="email"
            placeholder="example@gmail.com"
            name="patientEmail"
            required
          />
        </div>
        <div class="input-contact">
          <label for="phone"
            >Mobile no.<span class="asterick">&#42;</span><span
              class={isNumber ? "text-NotShown" : "text-Shown"}
              >Please Enter Number</span
            ></label
          >
          <input
            class={isNumber ? "number" : "not_number"}
            type="text"
            id="phone"
            on:input={addInputListener}
            name="patientNumber"
            placeholder="+977"
            required
          />
        </div>
        <div class="input-contact">
          <label for="tele">Telephone no.</label>
          <input
            type="text"
            id="tele"
            name="patientTelephone"
            placeholder="01- "
          />
        </div>
      </div>
      <div class="title">Address</div>
      <div class="fields">
        <div class="input-field">
          <label for="street"
            >Street name<span class="asterick">&#42;</span></label
          >
          <input type="text" id="street" name="streetName" required />
        </div>
        <div class="input-field">
          <label for="city">City<span class="asterick">&#42;</span></label>
          <input type="text" id="city" name="cityName" required />
        </div>
        <div class="input-field">
          <label for="state">State<span class="asterick">&#42;</span></label>
          <input type="text" id="state" name="stateName" required />
        </div>
      </div>
      <div class="buttonHolder">
        <input type="submit" value="Submit" />
      </div>
    </form>
  </div>
</main>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500&display=swap");
  main {
    max-height: 100vh;
    display: flex;
    align-items: center;
    padding-left: 250px;
    justify-content: center;
    font-family: "Poppins", sans-serif;
  }
  .asterick {
    color: red;
    /* padding-left: 2rem; */
  }
  .text-NotShown {
    display: none;
  }
  .text-Shown {
    display: inline-block;
    color: red;
    font-size: 0.8rem;
    font-family: "Poppins", sans-serif;
    padding: 0 0.8rem;
  }
  .not_number {
    background-color: #ffcccc;
    transform: scale(1.02);
  }
  .container {
    position: relative;
    max-width: 1600px;
    border-radius: 6px;
    padding: 30px;
    margin: 20px 15px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
    width: 100%;
  }
  .container header {
    font-size: 20px;
    margin-bottom: 30px;
    font-weight: 600;
    color: #333;
  }
  .container form {
    position: relative;
    min-height: 490px;
    margin-top: 16px;
    background-color: #fff;
  }
  .container form .title {
    display: block;
    margin-bottom: 8px;
    font-size: 16px;
    font-weight: 500;
    margin: 6px 0;
    color: #333;
  }
  .container form .fields {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
  }
  form .fields .input-field {
    display: flex;
    width: calc(100% / 3 - 30px);
    flex-direction: column;
    margin: 4px 0;
  }
  .input-field label {
    font-size: 14px;
    font-weight: 500;
    color: #2e2e2e;
  }
  .input-field input {
    outline: none;
    font-size: 14px;
    font-weight: 400;
    color: #333;
    border-radius: 5px;
    border: 1px solid #aaa;
    padding: 0 15px;
    height: 42px;
    margin: 8px 0;
  }
  .input-field input:is(:focus, :valid) {
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.13);
  }
  .container form .radios {
    display: flex;
    align-items: left;
    margin-bottom: 15px;
  }
  form .radios .input-radio {
    display: flex;
    font-size: 16px;
    font-weight: 400;
    width: calc(100% / 5 - 30px);
    /* flex-direction: column; */
    margin: 4px 0;
  }
  .container form .contact {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
  }
  form .contact .input-contact {
    display: flex;
    width: calc(100% / 3 - 30px);
    flex-direction: column;
    margin: 4px 0;
  }
  .input-contact label {
    font-size: 14px;
    font-weight: 500;
    color: #2e2e2e;
  }
  .input-contact input {
    outline: none;
    font-size: 14px;
    font-weight: 400;
    color: #333;
    border-radius: 5px;
    border: 1px solid #aaa;
    padding: 0 15px;
    height: 42px;
    margin: 8px 0;
  }
  form input[type="submit"] {
    margin: 20px 0;
    font-size: 1rem;
    width: 15%;
    color: black;
    background-color: #6d8baa;
    border: none;
    outline: none;
    border-radius: 10px;
    padding: 0.5rem 1rem;
    transition: all 200ms ease-in;
  }
  form input[type="submit"]:hover {
    background-color: #6d8bbb;
  }
  /* .buttonHolder {
    text-align: center;
  } */
  @media (max-width: 750px) {
    .container form {
      overflow-y: scroll;
    }
    .container form::-webkit-scrollbar {
      display: none;
    }
    form .fields .input-field {
      width: calc(100% / 2 - 30px);
    }
  }

  @media (max-width: 550px) {
    form .fields .input-field {
      width: 100%;
    }
  }
</style>
