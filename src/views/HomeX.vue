<template>
  <div class="home">
    <div class="row">
      <div class="container">
        <b-tabs content-class="">
          <b-tab title="Convert Case" active>
            <b-form-textarea
              id="textarea"
              v-model="text"
              placeholder="Type something.."
              rows="5"
              max-rows="6"
            ></b-form-textarea>
            <div class="row">
              <div class="col-12">
                <button
                  class="btn-default p-1 mr-1 font-weight-bold mt-2"
                  v-for="item in allFunc"
                  :key="item.type"
                  @click="doSomething(item.type)"
                >
                  {{ item.label }}
                </button>
              </div>
              <div class="col-6">
                <p>
                  Character Count: <span>{{ char_count }}</span> | Word Count:
                  <span>{{ word_count }}</span> | Line Count:
                  <span>{{ line_count }}</span>
                </p>
              </div>
              <!-- <b-button>Button</b-button> -->
            </div>
          </b-tab>
        </b-tabs>
      </div>
    </div>
  </div>
</template>

<script>
import md5 from "md5";
import toSentenceCase from "to-sentence-case";
export default {
  data() {
    return {
      text: "",
      allFunc: [
        { label: "Sentence case", type: 1 },
        { label: "lower case", type: 2 },
        { label: "UPPER CASE", type: 3 },
        { label: "Capitalized Case", type: 4 },
        { label: "aLtErNaTiNg cAsE", type: 5 },
        { label: "Title Case", type: 6 },
        { label: "InVeRsE CaSe", type: 7 },
        { label: "Download Text", type: 8 },
        { label: "Copy to Clipboard", type: 10 },
        { label: "Clear", type: 11 },
        { label: "Md5", type: 12 }
      ]
    };
  },
  name: "Home",
  computed: {
    word_count() {
      return (
        this.text.replace(/<\S[^><]*>/gi, ""),
        this.text.match(/\w+/g) ? this.text.match(/\w+/g).length : 0
      );
    },
    char_count() {
      let chr_cnt = 0;
      this.text.replace(/<\S[^><]*>/gi, "");
      chr_cnt = this.text.replace(/[\r\n]/g, "").length;
      chr_cnt = this.text.replace(/[\r\n\s]/g, "").length;
      chr_cnt = this.text.replace(/\r/g, "").replace(/\n/g, " ").length;
      return chr_cnt;
    },
    line_count() {
      var vm = this,
        filecont = vm.text;
      return (
        filecont.replace(/<\S[^><]*>/gi, ""),
        vm.text.match(/\w+/g) ? vm.text.match(/\w+/g).length : 0
      );
    }
  },
  methods: {
    download(filename, text) {
      var element = document.createElement("a");
      element.setAttribute(
        "href",
        "data:text/plain;charset=utf-8," + encodeURIComponent(text)
      );
      element.setAttribute("download", filename);

      element.style.display = "none";
      document.body.appendChild(element);

      element.click();

      document.body.removeChild(element);
    },
    doSomething(type) {
      if (type == 6 || type == 4) {
        let str = this.text.toLowerCase().split(" ");
        for (let i = 0; i < str.length; i++) {
          str[i] = str[i].charAt(0).toUpperCase() + str[i].slice(1);
        }
        this.text = str.join(" ");
      } else if (type == 2) {
        this.text = this.text.toLowerCase();
      } else if (type == 3) {
        this.text = this.text.toUpperCase();
      } else if (type == 7) {
        var s = "";
        var i = 0;
        while (i < this.text.length) {
          var n = this.text.charAt(i);
          if (n == n.toUpperCase()) {
            // *Call* toLowerCase
            n = n.toLowerCase();
          } else {
            // *Call* toUpperCase
            n = n.toUpperCase();
          }
          i += 1;
          s += n;
        }
        this.text = s;
      } else if (type == 5) {
        var chars = this.text.toLowerCase().split("");
        for (let i = 0; i < chars.length; i += 2) {
          chars[i] = chars[i].toUpperCase();
        }
        this.text = chars.join("");
      } else if (type == 8) {
        let filename = "hendra-convert.txt";
        this.download(filename, this.text);
      } else if (type == 10) {
        /* Get the text field */
        var copyText = document.getElementById("textarea");

        /* Select the text field */
        copyText.select();
        copyText.setSelectionRange(0, 99999); /*For mobile devices*/

        /* Copy the text inside the text field */
        document.execCommand("copy");
      } else if (type == 11) {
        this.text = "";
      } else if (type == 12) {
        this.text = md5(this.text);
      } else if (type == 1) {
        // var string = "text to          split";
        // string = string.split(" ");
        // var stringArray = new Array();
        // for (let i = 0; i < string.length; i++) {
        //   stringArray.push(string[i]);
        //   if (i != string.length - 1) {
        //     stringArray.push(" ");
        //   }
        // }
        let chars = this.text.split(/\r\n|\n|\r/);
        for (let i = 0; i < chars.length; i += 1) {
          chars[i] = toSentenceCase(chars[i]);
        }
        this.text = chars.join("\n");
      }
    }
  }
};
</script>
<style scoped>
input,
textarea,
button:focus {
  outline: none;
}
.btn-default {
  border: none;
  background: #c2c2c2;
  font-size: 14px;
}
.btn-default:hover {
  background: #434343;
  color: white;
}
</style>
