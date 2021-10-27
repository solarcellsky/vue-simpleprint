// 打印类属性、方法定义
/* eslint-disable */
const SimplePrint = function(dom, options) {
  if (!(this instanceof SimplePrint)) return new SimplePrint(dom, options);

  this.options = this.extend(
    {
      title: "",
      showTime: true,
      style: ""
    },
    options
  );

  if (typeof dom === "string") {
    this.dom = document.getElementById(dom);
  } else {
    this.isDOM(dom);
    this.dom = this.isDOM(dom) ? dom : dom.$el;
  }

  this.init();
};
SimplePrint.prototype = {
  init: function() {
    this.openPrintWindow(this.getHtml());
  },
  extend: function(obj, obj2) {
    for (var k in obj2) {
      obj[k] = obj2[k];
    }
    return obj;
  },

  getHtml: function() {
    return this.dom.innerHTML;
  },

  printTime: function() {
    const currentDT = new Date();
    let Y, M, DATE, HS, MS, SS;
    Y = currentDT.getFullYear(); //四位整数表示的年份
    M = currentDT.getMonth() + 1; //月
    if (M < 10) M = "0" + M; //两位整数表示的月份，在一些情况下需要补齐
    DATE = currentDT.getDate(); //日
    if (DATE < 10) DATE = "0" + DATE; //两位整数表示的天数，在一些情况下需要补齐
    HS = currentDT.getHours(); //时
    if (HS < 10) HS = "0" + HS; //两位整数表示的小时，在一些情况下需要补齐
    MS = currentDT.getMinutes(); //分
    if (MS < 10) MS = "0" + MS; //两位整数表示的分钟，在一些情况下需要补齐
    SS = currentDT.getSeconds(); //秒
    if (SS < 10) SS = "0" + SS; //两位整数表示的秒，在一些情况下需要补齐
    const $timeString =
      Y + "-" + M + "-" + DATE + " " + HS + ":" + MS + ":" + SS;
    return "<div class='print-time'>打印时间: " + $timeString + "</div>";
  },

  openPrintWindow: function(content) {
    const $printWindow = window.open("", "", "_blank");
    const $style = document.createElement("style");
    $style.innerHTML = this.options.style + ".print-time{color:#333;font-szie:10px;text-align:right;padding: 5px 0;border-top:1px solid #888}";
    const $printTime = this.options.showTime ? this.printTime() : "";
    const $printContent = $style.outerHTML + content + $printTime;
    $printWindow.document.title = this.options.title;
    $printWindow.document.body.innerHTML = $printContent;

    this.doPrint($printWindow);
  },

  doPrint: function(printWindow) {
    printWindow.print();
    printWindow.onafterprint = this.close(printWindow);
    return false;
  },

  close(printWindow) {
    printWindow.close();
  },

  isDOM:
    typeof HTMLElement === "object"
      ? function(obj) {
          return obj instanceof HTMLElement;
        }
      : function(obj) {
          return (
            obj &&
            typeof obj === "object" &&
            obj.nodeType === 1 &&
            typeof obj.nodeName === "string"
          );
        }
};
const MyPlugin = {};
MyPlugin.install = function(Vue, options) {
  Vue.prototype.$SimplePrint = SimplePrint;
};
export default MyPlugin;
