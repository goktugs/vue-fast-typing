<script>
export default {
  data() {
    return {
      words: ["uçak", "elma", "mouse", "kitap", "defter", "kalem"],
      inputWord: null,
      isTrue: true,
      trueCount: 0,
      falseCount: 0,
      timer: 60,
      interval: false,
      isRunning: false,
    };
  },

  watch: {
    inputWord(val) {
      if (!this.isRunning) this.toggleTimer();

      const currWord = this.words[0].slice(0, val.length);

      let userWord = val.replace(" ", "");

      this.isTrue = currWord === userWord;

      if (val.indexOf(" ") != -1) {
        this.isTrue ? (this.trueCount += 1) : (this.falseCount += 1);
        this.words.splice(0, 1);
        this.inputWord = "";
      }
    },
  },

  computed: {
    writingWordControl() {
      return this.isTrue ? "wordInUse" : "wrongWord";
    },
  },

  methods: {
    toggleTimer() {
      this.isRunning = true;
      this.interval = setInterval(this.timeProcess, 1000);
    },

    timeProcess() {
      this.timer--;
    },
  },
};
</script>

<template>
  <div class="col-md-6 container">
    <div class="h-100 p-5 text-white bg-dark rounded-3">
      <h2>Hızlı yazma yarışması</h2>

      <div class="card">
        <div class="card-body">
          <span
            :class="key != 0 || writingWordControl"
            v-for="(word, key) in words"
            :key="key"
            class="words ml-2"
          >
            {{ word }}
          </span>
        </div>
      </div>

      <div>
        Doğru Sayısı = {{ trueCount }}
        <br />
        Yanlış Sayısı = {{ falseCount }}
      </div>

      <div class="card mt-5 bg-secondary">
        <div class="card-body">
          <div class="input-group mb-3 input-group-lg">
            <input type="text" class="form-control" v-model="inputWord" />
            <button class="btn btn-light btn-lg mx-2" disabled type="button">
              {{ timer }}</button
            ><button class="btn btn-light btn-lg" type="button">Yenile</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.words {
  color: blue;
  font-size: 25px;
  font-weight: 400;
  padding: 5px;
  border-radius: 5px;
}

.wordInUse {
  background-color: slategrey;
  color: #fff;
}

.wrongWord {
  background-color: red;
  color: #fff;
}
</style>