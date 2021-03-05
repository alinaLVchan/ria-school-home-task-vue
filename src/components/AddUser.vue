<template>
  <div>
    <Error 
      v-bind:errors="errors"
    />
    <form @submit.prevent="onSubmit">
      <div class="row g-3">
        <div class="col-sm-5">
          <input type="text" class="form-control" placeholder="Your Name" v-model="name" @input="checkName">
        </div>
        <div class="col-sm-5">
          <input type="text" class="form-control" placeholder="Your Email" v-model="email" @input="checkEmail">
        </div>
        <div class="col-sm">
          <input type="text" class="form-control" placeholder="Your Age" v-model="age" @input="checkAge">
        </div>
      </div>
      <br>
      <div class="col">
        <button :disabled="!isEnabled" type="submit" class="btn btn-primary" @submit="onSubmit" >Add User</button>
      </div>
    </form>
  </div>

</template>

<script>
import Error from "@/components/Error";
export default {
  name: "AddUser",
  components: {Error},
  data() {
    return {
      name: '',
      email: '',
      age: '',
      errors: [],
      rawErrors: [
        {
          field: "Name",
          message: "length mustn`t be more than 20 characters."
        },
        {
          field: "Email",
          message: "email must match the template sample@mail."
        },
        {
          field: "Age",
          message: "must contain less than 4 figures and no characters."
        },
        {
          field: "Empty fields",
          message: "all fields are required."
        }
      ],
      isEnabled: true
    }
  },
  props: ["users"],
  methods: {
    deleteExistedSameError (field) {
      for (var i = 0; i < this.errors.length; i++){
          if (this.errors[i].field == field){
            this.errors.splice(i, 1);
          }
      }
    },

    checkName () {
      this.deleteExistedSameError("Name");
      if (this.name.length > 20){
        this.errors.push(this.rawErrors[0]);
      }
      this.changeSubmitButtonAvailability();
    },

    checkEmail () {
      this.deleteExistedSameError("Email");
      if (!/^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/.test(this.email)){
        this.errors.push(this.rawErrors[1]);
      }
      this.changeSubmitButtonAvailability();
    },

    checkAge () {
      this.deleteExistedSameError("Age");
      if (this.age.toString().length > 3 || /[a-zA-Z]/g.test(this.age.toString()) || Number.parseInt(this.age) < 0){
        this.errors.push(this.rawErrors[2]);
      }
      this.changeSubmitButtonAvailability();
    },

    changeSubmitButtonAvailability () {
        for (var i = 0; i < this.errors.length; i++){
          if (this.errors[i].field == this.rawErrors[3].field){
            this.errors.splice(i, 1);
          }
        }
      this.isEnabled = !this.errors.length;
    },

    onSubmit () {
      this.deleteExistedSameError(this.rawErrors[3].field);
      if (!this.name.length || !this.email.length || !this.age.length){
        this.errors.push(this.rawErrors[3]);
      }        
      if (this.name.length && this.email.length && this.age.length){
        const newUser = {
          name: this.name,
          email: this.email,
          age: this.age
        }
        this.$emit('add-user', newUser);
        this.name = this.email = this.age = '';
      }
    }
  }
}
</script>

<style scoped>

</style>