<template>
  <div class="flex min-h-36 justify-center items-center">
    <table v-if="orders" class="text-left w-full table-auto">
      <thead>
        <tr>
          <th>Order</th>
          <th>Date</th>
          <th>Status</th>
          <th class="text-right">Total</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="order in orders.nodes" :key="order.orderNumber">
          <td class="rounded-l-lg">
            <NuxtLink :to="`/order-summary?id=${order.orderNumber}`">{{ order.orderNumber }}</NuxtLink>
          </td>
          <td>{{ formatDate(order.date) }}</td>
          <td :class="`order-${order.status}`" class="order-status">{{ order.status }}</td>
          <td class="rounded-r-lg text-right">{{ order.total }}</td>
        </tr>
      </tbody>
    </table>
    <LoadingIcon v-else :size="24" stroke="2" />
  </div>
</template>

<script>
import GET_ORDERS from "~/gql/queries/getOrders.gql";
export default {
  data() {
    return {
      orders: null,
    };
  },
  methods: {
    async getOrders() {
      try {
        const { customer } = await this.$graphql.default.request(GET_ORDERS);
        this.orders = customer.orders;
      } catch (error) {
        console.log(error);
      }
    },
    formatDate(date) {
      // Outputs date like: January 29, 2022
      return new Date(date).toLocaleDateString("en-US", {
        month: "long",
        day: "numeric",
        year: "numeric",
      });
    },
  },
  mounted() {
    this.getOrders();
  },
};
</script>

<style lang="postcss" scoped>
tbody tr:nth-child(odd) {
  background-color: #fafafa;
}
tbody tr {
  @apply text-sm text-gray-500;
}
td,
th {
  @apply py-2 px-3;
}
.order-status {
  @apply border rounded-md font-semibold bg-gray-100 my-2 mx-3 text-xs leading-none p-1.5 inline-block;
}
.order-COMPLETED {
  @apply bg-green-50 border-green-100 text-green-600;
}
.order-CANCELLED {
  @apply bg-red-50 border-red-100 text-red-600;
}
.order-PENDING {
  @apply bg-yellow-50 border-yellow-100 text-yellow-600;
}
.order-PROCESSING {
  @apply bg-blue-50 border-blue-100 text-blue-600;
}
</style>
