<template>
  <div class="container">
    <h5>Transactions</h5>
    <table class="table">
      <thead>
        <tr>
          <th>Account</th>
          <th>Date</th>
          <th>Payee</th>
          <th>Category</th>
          <th>Memo</th>
          <th>Amount</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="transaction in cachedTransactions" v-bind:key="transaction.id">
          <td>{{ transaction.account_name }}</td>
          <td>{{ transaction.date }}</td>
          <td>{{ transaction.payee_name }}</td>
          <td>{{ transaction.category_name }}</td>
          <td>{{ transaction.memo }}</td>
          <td>{{ convertMilliUnitsToCurrencyAmount(transaction.amount).toFixed(2) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import { utils } from 'ynab';

  export default {
    props: ['transactions'],
    data() {
      return {
        cachedTransactions: [],
        cacheTimestamp: null
      };
    },
    created() {
      this.buildCache();
    },
    methods: {
      convertMilliUnitsToCurrencyAmount: utils.convertMilliUnitsToCurrencyAmount,
      buildCache() {
        if (this.shouldRebuildCache()) {
          this.cachedTransactions = this.transactions;
          this.cacheTimestamp = Date.now();
        }
      },
      shouldRebuildCache() {
        return (
          this.cachedTransactions.length === 0 ||
          this.cacheTimestamp === null ||
          Date.now() - this.cacheTimestamp >= 600000
        );
      }
    }
  };
</script>
