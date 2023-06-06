<template>
    <div class="calculator" @keydown="handleKeyDown" tabindex="0">
        <h1>Calculator</h1>
        <input type="text" id="result" v-model="inputVal" disabled />
        <div class="row-def">
            <button class="opearator clear" @click="clear">
                <i class="fa-solid fa-c"></i>
            </button>
            <button class="operator remove" @click="removeOne">
                <i class="fa-solid fa-delete-left"></i>
            </button>
            <button class="operator square" @click="square">χ2</button>
            <button class="operator sqrt" @click="sqrt">
                <i class="fa-sharp fa-solid fa-square-root-variable"></i>
            </button>
        </div>
        <div class="row">
            <button class="numbers" @click="addNum(7)">7</button>
            <button class="numbers" @click="addNum(8)">8</button>
            <button class="numbers" @click="addNum(9)">9</button>
            <button class="operator plus" @click="action('plus')">
                <i class="fa-solid fa-plus"></i>
            </button>
        </div>
        <div class="row">
            <button class="numbers" @click="addNum(4)">4</button>
            <button class="numbers" @click="addNum(5)">5</button>
            <button class="numbers" @click="addNum(6)">6</button>
            <button class="operator minus" @click="action('minus')">
                <i class="fa-solid fa-minus"></i>
            </button>
        </div>
        <div class="row">
            <button class="numbers" @click="addNum(1)">1</button>
            <button class="numbers" @click="addNum(2)">2</button>
            <button class="numbers" @click="addNum(3)">3</button>
            <button class="operator mult" @click="action('mult')">
                <i class="fa-solid fa-xmark"></i>
            </button>
        </div>
        <div class="row">
            <div>
                <button class="numbers zero" @click="addNum(0)" :disabled="disableZero">0</button>
                <button class="numbers dot" @click="addNum('.')" :disabled="buttonClicked">.</button>
            </div>
            <div>
                <button class="operator div" @click="action('div')">
                    <i class="fa-solid fa-divide"></i>
                </button>
            </div>
        </div>
        <div class="row-def">
            <button class="operator factorial" @click="factorial">
                x <i class="fa-solid fa-exclamation"></i>
            </button>
            <button class="operator" @click="pi">π</button>
            <button class="operator" @click="neg">
                <i class="fa-solid fa-plus-minus"></i>
            </button>
            <button class="operator equal" @click="equal">
                <i class="fa-solid fa-equals"></i>
            </button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            inputVal: 0,
            res: 0,
            actionType: "",
            buttonClicked: false,
            disableZero: true
        };
    },
    methods: {
        addNum(n) {
            if (n === ".") {
                this.buttonClicked = true;
            }
            this.disableZero = false
            this.inputVal = this.inputVal + n + "";
        },
        clear() {
            this.inputVal = 0;
            this.disableZero = true;
        },
        action(act) {
            this.buttonClicked = false;
            this.res += parseFloat(this.inputVal);
            this.inputVal = "";
            this.actionType = act;
        },
        square() {
            this.inputVal *= this.inputVal;
        },
        sqrt() {
            this.inputVal = Math.sqrt(this.inputVal);
        },
        factorial() {
            let result = 1;
            let current = this.inputVal;
            while (current > 1) {
                result *= current;
                current--;
            }
            this.inputVal = result;
        },
        pi() {
            this.inputVal = Math.PI
        },
        neg() {
            this.inputVal = this.inputVal >= 0 ? -this.inputVal : Math.abs(this.inputVal)
        },
        equal() {
            if (this.actionType === "plus") {
                this.inputVal = parseFloat(this.res) + parseFloat(this.inputVal);
            } else if (this.actionType === "minus") {
                this.inputVal = parseFloat(this.res) - parseFloat(this.inputVal)
            } else if (this.actionType === "mult") {
                this.inputVal = parseFloat(this.res) * parseFloat(this.inputVal);
            } else if (this.actionType === "div") {
                let val = parseFloat(this.res) / parseFloat(this.inputVal)
                this.inputVal = Number.isInteger(val) ? val : val.toFixed(2);
            }
            this.buttonClicked = false;
            this.res = 0;
        },
        removeOne() {
            this.inputVal = ("" + this.inputVal).slice(0, -1);
        },
        handleKeyDown(event) {
            const key = event.key;
            const allowedKeys = [
                "0", "1", "2", "3", "4", "5", "6", "7", "8", "9",
                "+", "-", "*", "/", "=", ".", "Enter", "Escape", "Backspace",
            ];

            if (allowedKeys.includes(key)) {
                event.preventDefault();

                if (key >= "0" && key <= "9") {
                    this.addNum(parseFloat(key));
                } else if (key === "+") {
                    this.action("plus");
                } else if (key === "-") {
                    this.action("minus");
                } else if (key === "*") {
                    this.action("mult");
                } else if (key === "/") {
                    this.action("div");
                } else if (key === "=" || key === "Enter") {
                    this.equal();
                } else if (key === "Escape") {
                    this.clear();
                } else if (key === "Backspace") {
                    this.removeOne();
                } else if (key === "." && !this.buttonClicked) {
                    this.addNum(".");
                }
            }
        },
    },
};
</script>

<style>
* {
    box-sizing: border-box;
}

html {
    background-color: #504e4e;
    outline: none;
}

body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
}

.calculator {
    width: 320px;
    margin: 50px auto;
    background-color: #f4f4f4;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.calculator h1 {
    text-align: center;
    font-size: 28px;
    font-weight: bold;
    margin-top: 0;
    color: #000;
}

.calculator input {
    width: 100%;
    margin-bottom: 20px;
    padding: 10px;
    font-size: 24px;
    text-align: right;
    border: none;
    background-color: #fff;
    border-radius: 4px;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.calculator .row-def,
.row {
    display: flex;
    margin-bottom: 10px;
}

.calculator .row-def button {
    flex: 1;
    padding: 15px;
    font-size: 24px;
    border: none;
    background-color: #ff9500;
    color: #fff;
    box-shadow: 0 1px 2px #0000001a;
    cursor: pointer;
    outline: none;
}

.calculator .row-def button.sqrt {
    border-radius: 0px 4px 4px 0px;
}

.calculator .row-def button.clear {
    border-radius: 4px 0px 0px 4px;
}

.calculator .row-def button.factorial {
    border-radius: 4px 0px 0px 4px;
}

.calculator .row-def button.equal {
    border-radius: 0px 4px 4px 0px;
}

.calculator .row-def button:hover,
.calculator .row button.operator:hover {
    background: #ffa825;
}

.calculator .row button {
    flex: 1;
    padding: 15px;
    font-size: 24px;
    border: none;
    background-color: #e0e0e0;
    border-radius: 4px;
    box-shadow: 0 1px 2px #0000001a;
    cursor: pointer;
    color: #000;
    outline: none;
}

.calculator .row button.operator {
    background-color: #ff9500;
    color: #fff;
}

.calculator .row button.numbers:hover {
    background: #c5c1c1;
}

.calculator .row button.numbers.zero {
    width: 140px;
    height: 58px;
}

.calculator .row button.numbers.dot {
    width: 70px;
    height: 58px;
}

.calculator .row button.operator.div {
    width: 70px;
    height: 58px;
}
</style>
