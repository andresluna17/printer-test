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
const USB = require("escpos-usb");
escpos.USB = USB;
export default {
  name: "app",
  methods: {
    printPaper() {
      console.log("printPaper! start");
      let devices = escpos.USB.findPrinter();
      if (devices)
        devices.forEach((device) => {
          const options = { encoding: "GB18030" /* default */ };
          const printer = new escpos.Printer(device, options);
          console.log(":D");
          console.log({ device });
          device.open((error, d) => {
            if (error) console.log({ error, device: d });
            else
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
        });
      // const device = escpos.USB();
    },
  },
};
</script>
