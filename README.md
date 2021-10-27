# How to
## import in your main.js

```
import SimplePrint from 'vue-simple-print'
Vue.use(SimplePrint);
```

## in your page or compnent

```
this.$SimplePrint("domId", {
  title: "Page Title",
  style: "print style string, eg: .print{font-size: 18px; padding: 20px}",
  showTime: "show print time or not, boolen: true/false"
})
```

## screenshoot

<img width="1903" alt="Screen Shot 2021-10-27 at 13 48 49 PM" src="https://user-images.githubusercontent.com/546897/139007266-901dfb70-7d33-47c3-be92-0d7a5ce6433e.png">
