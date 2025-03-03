<template>
  <div class="container">
    <input type="number" v-model="this.passLength" title="Default length 9 symbols" name="plength" placeholder="Input password length (default → 9)" onfocus="this.placeholder=''" onblur="this.placeholder='Input password length (default → 9)'"/>
    <textarea class="output" readonly v-if="strongPassword.length>300">Allowed length of password → 300 max!!!</textarea>
    <textarea class="output" readonly v-else-if="strongPassword.length>0">  {{ strongPassword }}  </textarea>
    <textarea class="output" readonly v-else>0</textarea>
    <button @click="generateStrongPassword">Generate</button>
    <div  v-if="strongPassword.length>0 && strongPassword.length<301">
      <button @click="kopet">{{ accept_text }}</button>
      <button @click="reset">Reset</button>
    </div>
  </div>
</template>

<script>
//Simple password generator, password length from default (9 symbol) to max (300 symbol and can be increased.)
export default {
  data() {
    return {
      strongPassword: "",
      passLength: '',
      kopets: false,
      title: 'Password generator',
      accept_text: 'Copy',
    };
  },
  mounted(){
    document.title = this.title;
  },
  methods: {
    generateStrongPassword() {
      let strongPassword = "";
      this.kopets = false;

      if(this.passLength <9 || this.passLength==="") {
        this.passLength=9
      }


      const charset = "abcdefghVWXYZ12345ijklmnopq$%rstuvwxyzPQRSTU2625↓→↑§¶∟←↔ы{↨/.,67890.ABCDEFGHIJKLMNO,!@#&*()-+"; //default chars array
      const isValidPassword = (password) => {
        const hasNumbers = (password.match(/[0-9]/g) || []).length > 1;
        const hasSymbols = (password.match(/[-+!@#$%^&*().,↓→↑§¶∟←↔ы{↨/]/g) || []).length > 1;
        const hasUppercase = (password.match(/[A-Z]/g) || []).length > 1;
        const hasLowercase = (password.match(/[a-z]/g) || []).length > 1;
        const startsCorrectly =
            !/[-+!@#$%^&*().,]/.test(password[0]) &&
            !/[-+!@#$%^&*().,]/.test(password[1]) &&
            isNaN(password[0]) &&
            isNaN(password[1]);
        return hasNumbers && hasSymbols && hasUppercase && hasLowercase && startsCorrectly;
      };
      while (true) {
        // Generate a password
        strongPassword = "";
        for (let i = 0; i < this.passLength; i++) {
          const randomChar = charset[Math.floor(Math.random() * charset.length)];
          strongPassword += randomChar;
        }
        // Validate the password
        if (isValidPassword(strongPassword)) {
          break;
        }
      }
      // Capitalize the first two characters of the password
      this.strongPassword = strongPassword[0].toUpperCase() + strongPassword[1].toUpperCase() + strongPassword.slice(2);
      this.accept_text='Copy';
    },
    kopet(){
      navigator.clipboard.writeText(this.strongPassword);
      this.kopets=true;
      this.accept_text='✓';
    },
    reset(){
      this.strongPassword = "";
      this.passLength = "";
    }
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  margin: 5% auto;
  height: 100%;
  width: 90%;
  max-width: 600px; 
  text-align: center;
  align-items: center;
  gap: 1rem;
}

button {
  width: 100%;
  height: 40px;
  text-align: center;
  cursor: pointer;
  border: none;
  font-weight: bold;
  font-size: 1rem;
}

input {
  width: 100%;
  height: 40px;
  text-align: center;
  border: none;
  border-bottom: 1px solid green;
  font-size: 1rem;
}

input:focus {
  outline: none;
}

input::-webkit-inner-spin-button {
  display: none;
}

.output {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 50px;
  border: none;
  border-bottom: 1px solid blue;
  height: 150px;
  font-size: 1rem;
  text-align: center;
  resize: none;
  box-sizing: border-box;
}

.output:focus {
  outline: none;
}

.status {
  text-align: center;
}

@media (min-width: 768px) {
  .container {
    width: 80%;
    margin: 8% auto;
    gap: 1.5rem;
  }

  button {
    font-size: 1.1rem;
    height: 45px;
  }

  input {
    font-size: 1.1rem;
    height: 45px;
  }

  .output {
    height: 180px;
    font-size: 1.1rem;
  }
}

@media (min-width: 1024px) {
  .container {
    width: 70%;
    max-width: 800px;
  }

  button {
    font-size: 1.2rem;
    height: 50px;
  }

  input {
    font-size: 1.2rem;
    height: 50px;
  }

  .output {
    height: 200px;
    font-size: 1.2rem;
  }
}

@media (min-width: 1440px) {
  .container {
    width: 60%;
    max-width: 1000px;
  }
}
</style>


