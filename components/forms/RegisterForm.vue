<template>
  <div class="mx-auto max-w-md">
    <form class="mb-4 grid gap-4" @submit.prevent="registerNewUser">
      <input placeholder="Email" type="email" v-model="userInfo.email" id="email" required />
      <input placeholder="Username" type="text" v-model="userInfo.username" id="username" required />
      <input placeholder="Password" type="password" id="password" v-model="userInfo.password" required />
      <button class="mt-4 text-lg">{{ buttonText }}</button>
    </form>
  </div>
</template>

<script>
import REGISTER_CUSTOEMER from "~/gql/mutations/registerCustomer";
import GET_CART from "~/gql/queries/getCart";
export default {
  data() {
    return {
      userInfo: { email: "", password: "", username: "" },
      buttonText: "Register",
    };
  },
  methods: {
    async registerNewUser() {
      try {
        this.buttonText = "Registering...";
        const { registerCustomer } = await this.$graphql.default.request(REGISTER_CUSTOEMER, { input: this.userInfo });
        console.log(registerCustomer);

        if (registerCustomer) {
          const { cart, viewer, customer } = await this.$graphql.default.request(GET_CART);
          this.$store.commit("updateCart", cart);
          this.$store.commit("updateUser", customer);
          this.$router.push("/");
          this.$cookiz.remove("woo");
        }
      } catch (error) {
        console.error(JSON.stringify(error, undefined, 2));
      }
    },
  },
};
</script>

<style lang="postcss" scoped>
input,
button {
  @apply border rounded-2xl w-full p-3 px-4;
}
form button {
  @apply bg-primary rounded-2xl text-white p-3;
}
</style>
