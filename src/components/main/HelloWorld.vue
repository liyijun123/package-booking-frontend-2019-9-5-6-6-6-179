<template>
  <nav class="hello">
    <a-form class="helloForm" layout="block" :form="form" @submit="handleSubmit">
      <a-form-item style="text-align:center">
        <article>欢迎登陆！</article>
      </a-form-item>
      <a-form-item :validate-status="userNameError() ? 'error' : ''" :help="userNameError() || ''">
        <a-input
          v-decorator="[
          'userName',
          {rules: [{ required: true, message: 'Please input your username!' }]}
        ]"
          placeholder="Username"
        >
          <a-icon slot="prefix" type="user" style="color:rgba(0,0,0,.25)" />
        </a-input>
      </a-form-item>
      <a-form-item :validate-status="passwordError() ? 'error' : ''" :help="passwordError() || ''">
        <a-input
          v-decorator="[
          'password',
          {rules: [{ required: true, message: 'Please input your Password!' }]}
        ]"
          type="password"
          placeholder="Password"
        >
          <a-icon slot="prefix" type="lock" style="color:rgba(0,0,0,.25)" />
        </a-input>
      </a-form-item>
      <a-form-item>
        <a-button
          style="width:100%"
          type="primary"
          html-type="submit"
          :disabled="hasErrors(form.getFieldsError())"
          @click="login"
        >Log in</a-button>
      </a-form-item>
    </a-form>
  </nav>
</template>

<script>
function hasErrors(fieldsError) {
  return Object.keys(fieldsError).some(field => fieldsError[field]);
}
export default {
  name: "HelloWorld",
  data() {
    return {
      hasErrors,
      form: this.$form.createForm(this)
    };
  },
  props: {
    msg: String
  },
  methods: {
    login() {
      this.$router.push("/warehouse")
    },
    // Only show error after a field is touched.
    userNameError() {
      const { getFieldError, isFieldTouched } = this.form;
      return isFieldTouched("userName") && getFieldError("userName");
    },
    // Only show error after a field is touched.
    passwordError() {
      const { getFieldError, isFieldTouched } = this.form;
      return isFieldTouched("password") && getFieldError("password");
    },
    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (!err) {
          console.log("Received values of form: ", values);
        }
      });
    }
  },
  mounted() {
    this.$nextTick(() => {
      // To disabled submit button at the beginning.
      this.form.validateFields();
    });
  }
};
</script>

<style scoped >
@import "../../assets/css/main.css";
.hello {
  background: bisque;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;}
.helloForm {
    margin-top: 200px;
  }


</style>
