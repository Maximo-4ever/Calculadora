<template>
  <h3 class="btnSelected">{{ acumulator + btnOperation }}</h3>
  <h2 class="btnSelected">{{ result }}</h2>
  <main class="d-grid" @click="btnListener">
    <div class="btn btn-gray">C</div>
    <div class="btn btn-gray">{{ del }}</div>
    <div class="btn btn-gray">%</div>
    <div class="btn btn-yellow">/</div>
    <div class="btn">7</div>
    <div class="btn">8</div>
    <div class="btn">9</div>
    <div class="btn btn-yellow">*</div>
    <div class="btn">4</div>
    <div class="btn">5</div>
    <div class="btn">6</div>
    <div class="btn btn-yellow">-</div>
    <div class="btn">1</div>
    <div class="btn">2</div>
    <div class="btn">3</div>
    <div class="btn btn-yellow">+</div>
    <div class="btn">~</div>
    <div class="btn">0</div>
    <div class="btn">.</div>
    <div class="btn btn-yellow">=</div>
  </main>
</template>

<script>
import { ref } from "@vue/reactivity";
import { evaluate } from "mathjs";

export default {
  name: "App",
  setup() {
    const del = "<";
    const btnOperation = ref("");
    const acumulator = ref("");
    const result = ref(0);
    const operatorClick = ref(false);

    const btnListener = (e) => {
      const btnSelected = e.target;
      if (btnSelected.classList.contains("btn")) {
        result.value = "";

        if (!isNaN(btnSelected.textContent)) {
          if (operatorClick.value) {
            btnOperation.value = "";
            operatorClick.value = false;
          }

          console.log("Es un numero :)");
          btnOperation.value = `${btnOperation.value}${btnSelected.textContent}`;
        } else {
          if (btnSelected.textContent === "C") {
            return restartOperation(true);
          }
          if (btnSelected.textContent === "=") {
            return resultOperation();
          }
          processOperation(btnSelected.textContent);
        }
      }
    };
    const processOperation = (valor) => {
      if (valor === ".") {
        if (btnOperation.value.indexOf(".") === -1) {
          btnOperation.value = `${btnOperation.value}${valor}`;
        }
        return;
      }
      if (valor === "%") {
        if (btnOperation.value !== "") {
          btnOperation.value = `${parseFloat(btnOperation.value) / 100}`;
          operatorClick.value = true;
        }
        return;
      }
      addOperator(valor);
    };

    const addOperator = (operador) => {
      if (!operatorClick.value) {
        acumulator.value += `${btnOperation.value} ${operador} `;
        btnOperation.value = "";
        operatorClick.value = true;
      }
    };
    const resultOperation = () => {
      !operatorClick.value
        ? (result.value = evaluate(acumulator.value + btnOperation.value))
        : (result.value = "Error");
      restartOperation(false);
    };
    const restartOperation = (val) => {
      btnOperation.value = "";
      acumulator.value = "";

      val ? (result.value = 0) : "";
    };

    return { btnListener, btnOperation, acumulator, result, del };
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Poppins", sans-serif;
  background-color: #272931;
}
#app {
  height: 100vh;
  max-width: 415px;
  background-color: #2f323c;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  flex-direction: column;
  color: #fff;
  font-size: 30px;
  margin: auto;
  padding: 0 19px;
}
.btnSelected {
  width: 100%;
  font-size: 48px;
  line-height: 56px;
  text-align: end;
}
.d-grid {
  display: grid;
  align-items: center;
  justify-content: center;
  gap: 9px 7px;
  grid-template-columns: repeat(4, 1fr);
  margin-top: 50px;
  margin-bottom: 15px;
}
.btn {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 90px;
  height: 93px;
  border-radius: 30px;
  box-shadow: 5px 5px 34px #1b1d23, -5px -5px 34px #434755;
  border: 4px solid #2e313c;
  user-select: none;
  cursor: pointer;
}
.btn-gray {
  background: linear-gradient(
    136.36deg,
    rgba(92, 104, 126, 0.28) 0%,
    rgba(157, 178, 216, 0.33) 111.19%
  );
  border: 3px solid rgb(73 80 96);
}
.btn-yellow {
  background: linear-gradient(227deg, #feb603 0%, #ffa500f2 100%);
  border: 3px solid rgba(51, 54, 64, 0.18);
  box-shadow: 0 0 30px #fbad0352;
}

@media (max-width: 397px) {
  #app {
    font-size: 26px;
  }
  .d-grid {
    margin-top: 30px;
  }
  .btnSelected {
    font-size: 45px;
  }
  .btn {
    width: 78px;
    height: 81px;
  }
}
</style>
