<template>
    <div class="group reqres">
        <div class="half left">
            <h2>Request</h2>
            <button
                class="half left"
                @click="handleClick()"
            >
                Make multiply request
            </button>
            <div class="half">
                <input 
                  type="text"
                  v-model="requestValue"
                  class="half left"
                />
                <span 
                  class="response half item"
                >
                  {{displayResponse}}
                </span>
            </div>
        </div>
        <div class="half">
                <h2>Response</h2>
                <div class="half left item">Multiply number with:</div>
                <input 
                  type="text"
                  class="half"
                  v-model="responseValue"
                />
        </div>
    </div>
</template>
<script>
export default {
    name: 'my-rpc',
    props: ["ds"],
    data() {
      return {
        responseValue: "7",
        requestValue: "3",
        displayResponse: "-"
      };
    },
    created() {
      this.rpc = this.ds.rpc;
      this.rpc.provide("multiply-numbeer", (data, response) => {
        response.send(data.value * parseFloat(this.responseValue));
      });
    },
    methods: {
      handleClick() {
        const data = {
        value: parseFloat(this.requestValue)
        };

        this.rpc.make("multiply-number", data, (err, resp) => {
        this.displayResponse = resp || err.toString();
        });
      }
    }
}
</script>