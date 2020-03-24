<script>
  import { onMount } from 'svelte'

  const BASE_URL = 'https://api.exchangeratesapi.io/latest'

  let currencies = []
  let amount = 1
  let from = 'EUR'
  let to = 'USD'
  let exchangeRates = {}
  let currentRate = 0

  function handleFocus() {
    this.select()
  }

  function handleInput(e) {
    if (e.target.name === 'fromCurrency') {
      from = e.target.value.toUpperCase()
    } else {
      to = e.target.value.toUpperCase()
    }
  }

  onMount(async () => {
    const res = await fetch(`${BASE_URL}?base=${from}`)
    const data = await res.json()

    currencies = [from, ...Object.keys(data.rates)].sort()
    exchangeRates = { ...data.rates, [from]: 1 }
    currentRate = exchangeRates[to]
  })
</script>

<main class="flex items-center justify-center">
  <div
    class="flex flex-col w-full rounded-lg shadow bg-white sm:w-1/2 lg:max-w-xl"
  >
    <h1
      class="m-0 py-2 px-4 font-thin text-3xl text-white bg-green-700 rounded-t"
    >
      Currency Converter
    </h1>
    <div class="flex flex-col items-center justify-center w-full pt-4">
      <label for="amountToConvert">Amount to convert:</label>
      <input
        id="amountToConvert"
        class="block py-2 px-3 leading-6 border border-gray-300 rounded"
        name="amountToConvert"
        type="number"
        min="0"
        placeholder="Amount"
        bind:value={amount}
      />
    </div>

    <div class="flex items-center w-full h-full py-4 text-center">
      <div class="flex flex-col items-center w-3/5 px-4 sm:w-full">
        <label for="fromCurrency">Convert From:</label>
        <input
          id="fromCurrency"
          name="fromCurrency"
          maxlength="3"
          list="fromCurrencyList"
          on:input={handleInput}
          on:focus={handleFocus}
        />
        <datalist id="fromCurrencyList">
          {#each currencies as option}
            <option>{option}</option>
          {/each}
        </datalist>

      </div>

      <button
        class="flex items-center justify-center m-0 bg-transparent border-2
        border-transparent pointer focus:border-gray-300"
      >
        <svg
          class="w-6 h-6"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 512 512"
        >
          <path
            d="M377.941
            169.941V216H134.059v-46.059c0-21.382-25.851-32.09-40.971-16.971L7.029
            239.029c-9.373 9.373-9.373 24.568 0 33.941l86.059 86.059c15.119
            15.119 40.971 4.411 40.971-16.971V296h243.882v46.059c0 21.382 25.851
            32.09 40.971 16.971l86.059-86.059c9.373-9.373 9.373-24.568
            0-33.941l-86.059-86.059c-15.119-15.12-40.971-4.412-40.971 16.97z"
          />
        </svg>
      </button>

      <div class="flex flex-col items-center w-3/5 px-4 sm:w-full">
        <label for="toCurrency">Convert To:</label>
        <input
          id="toCurrency"
          name="toCurrency"
          class="currency-input"
          maxlength="3"
          on:input={handleInput}
          on:focus={handleFocus}
          list="toCurrencyList"
        />
        <datalist id="toCurrencyList">
          {#each currencies as option}
            <option>{option}</option>
          {/each}
        </datalist>
      </div>
    </div>

    <div class="flex flex-wrap justify-center w-full pb-2">
      <p>Conversion Result Goes Here.</p>
    </div>

  </div>
</main>

<style>
  :global(body) {
    padding-top: 2rem;
    background-color: #fafafa;
  }
</style>
