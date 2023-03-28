<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/vue.svg" />
    <h1>Hello World!</h1>
    <p>Let's print awesomeness.</p>
    <button @click="printPaper">Print Paper!</button>
  </div>
</template>

<script lang="ts">
import escpos from "escpos";
const usb = require("escpos-usb");
escpos.USB = usb;
export default {
  name: "app",
  methods: {
    printPaper() {
      try {
        console.log("printPaper! start");
        const device = new escpos.USB();
        const options = { encoding: "GB18030" /* default */ };
        const printer = new escpos.Printer(device, options);

        device.open(function () {
          printer
            .font("A")
            .align("CT")
            .style("BU")
            .size(1, 1)
            .text("Hello world!")
            .text("Welcome to the Awesome-land!!!")
            .cut()
            .cashdraw()
            .close();
        });
      } catch (error) {
        console.log({ error });
      } finally {
        console.log("printPaper! end");
      }
    },
  },
};
</script>