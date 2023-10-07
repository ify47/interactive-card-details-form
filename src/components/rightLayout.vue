<script setup>
import { computed, inject, ref } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { helpers, required } from "@vuelidate/validators";

const ccNameValue = ref("");
const ccNumberValue = ref("");
const ccMonth = ref('')
const ccYear = ref('')
const ccCvv = ref('')
// const cardNumbersProps = inject('c_numbers')
const { cardName, cardNumbers, cardMonth, cardYear, cardCvv, isVisible } = inject("inputDetails");

function handleName(event) {
    // so value can be updated on mobile aswell
    ccNameValue.value = event.target.value;
    cardName.value = ccNameValue.value;
}

function handleCcNumber(event) {
    ccNumberValue.value = event.target.value;
    // Add non-numeric characters from the input
    const formattedNumbers = ccNumberValue.value.replace(/\W/g, "");
    // Update the cardNumbers with new values formatted with 0 passed onto leftLayout
    cardNumbers.value = formattedNumbers.padEnd(16, "0");
    // Update the ccNumberValue field with spaces after every 4 characters on
    ccNumberValue.value = formattedNumbers.replace(/(\w{4})(?=\w)/g, "$1 ");
}

const handleCcDetails = (type) => {
    switch (type) {
        case 'mm':
            cardMonth.value = ccMonth.value
            break;
        case 'yy':
            cardYear.value = ccYear.value
            break;
        case 'cvv':
            cardCvv.value = ccCvv.value

        default:
            break;
    }
}

const containOnlyNumbers = (value) => {
    var numbers = /^[0-9 ]*$/;
    return value.match(numbers);
};

const rules = computed(() => ({
    ccNameValue: { required: helpers.withMessage(`can't be blank`, required) },
    ccNumberValue: {
        required: helpers.withMessage(`can't be blank`, required),
        containOnlyNumbers: helpers.withMessage(
            "Wrong format, numbers only",
            containOnlyNumbers
        ),
    },
    ccMonth: { required },
    ccYear: { required },
    ccCvv: { required }
}));

const v$ = useVuelidate(rules, { ccNameValue, ccNumberValue, ccMonth, ccYear, ccCvv });

const submitForm = async () => {
    const result = await v$.value.$validate();
    result ? isVisible.value = false : null
};
</script>

<template>
    <form @submit.prevent="submitForm">
        <div class="mane">
            <label class="name" for="fullName">Cardholder Name</label>

            <input class="type" :class="{ danger: v$.ccNameValue.$dirty && v$.ccNameValue.$invalid }" type="text"
                name="fullName" id="fullName" placeholder="e.g Jane Appleseed" @input="handleName" v-model="ccNameValue"
                :maxlength="29" @blur="v$.ccNameValue.$touch" />
            <p v-for="error in v$.ccNameValue.$errors" :key="error.$uid" class="error">
                {{ error.$message }}
            </p>
        </div>
        <div class="mane">
            <label class="name" for="card">Card Number</label>
            <input class="type" :class="{
                danger:
                    (v$.ccNumberValue.$dirty && v$.ccNumberValue.required.$invalid) ||
                    v$.ccNumberValue.containOnlyNumbers.$invalid,
            }" type="text" id="card" placeholder="e.g. 1234 5678 9123 0000" :maxlength="19" v-model="ccNumberValue"
                @input="handleCcNumber" @blur="v$.ccNumberValue.$touch" />
            <!-- <p v-if="v$.ccNameValue.$dirty && v$.ccNameValue.$invalid" class="error">Wrong format, numbers only</p> -->
            <p v-for="error in v$.ccNumberValue.$errors" :key="error.$uid" class="error">
                {{ error.$message }}
            </p>
        </div>
        <div class="flexnum">
            <div class="mane">
                <label class="name" for="details">Exp. Date (MM/YY)</label>
                <div>
                    <input class="typenum" :class="{ danger: v$.ccMonth.$dirty && v$.ccMonth.$invalid }" type="tel"
                        name="mm" id="details" :maxlength="2" placeholder="MM" v-model="ccMonth"
                        @input="handleCcDetails('mm')" @blur="v$.ccMonth.$touch" />

                    <input class="typenum minspace" :class="{ danger: v$.ccYear.$dirty && v$.ccYear.$invalid }" type="tel"
                        name="yy" id="cardDetails" :maxlength="2" placeholder="YY" v-model="ccYear"
                        @input="handleCcDetails('yy')" @blur="v$.ccYear.$touch" />
                    <p v-if="v$.ccMonth.$dirty && v$.ccMonth.$invalid || v$.ccYear.$dirty && v$.ccYear.$invalid"
                        class="error">Can't be blank</p>
                </div>
            </div>
            <div class="mane">
                <label class="name" for="card">Cvc</label>
                <input class="typenumcvv" :class="{ danger: v$.ccCvv.$dirty && v$.ccCvv.$invalid }" type="tel" id="cvv"
                    :maxlength="4" placeholder="e.g. 123" v-model="ccCvv" @input="handleCcDetails('cvv')"
                    @blur="v$.ccCvv.$touch" />
                <p v-if="v$.ccCvv.$dirty && v$.ccCvv.$invalid" class="error">
                    Can't be blank</p>
            </div>
        </div>
        <button type="submit">Confirm</button>
    </form>
</template>

<style lang="scss" scoped>
form {
    margin: auto;

    input {
        font-family: "Space Grotesk", sans-serif;
        width: 18rem;
        margin: 5px 0 25px;
        border: 1px solid hsl(270, 3%, 87%);
        border-radius: 5px;
        font-size: 15px;

        &::placeholder {
            font-size: 16px;
            color: hsl(270, 3%, 87%);
            font-family: "Space Grotesk", sans-serif;
        }

        &:focus {
            outline: none;
            background: linear-gradient(white, white) padding-box,
                linear-gradient(hsl(249, 99%, 64%), hsl(278, 94%, 30%)) border-box;
            border: 1px solid transparent;
            border-radius: 5px;
        }
    }

    .danger {
        border: 1px solid red;
    }

    .mane {
        display: flex;
        flex-direction: column;

        .type {
            padding: 10px 12px;
        }

        .typenum {
            border-radius: 5px;
            padding: 8px 12px;
            width: 45px;
        }

        .minspace {
            margin-left: 7px;
        }
    }

    .name {
        text-transform: uppercase;
        font-size: 14px;
        letter-spacing: 1px;
    }

    .flexnum {
        display: flex;
        justify-content: space-between;

        .typenumcvv {
            border-radius: 5px;
            padding: 8px 12px;
            width: 124px;
        }
    }

    button {
        width: 100%;
        font-size: 15px;
        border-radius: 5px;
        padding: 11px;
        background-color: hsl(278, 68%, 11%);
        color: white;
        cursor: pointer;
    }

    .error {
        font-size: 11px;
        color: red;
        margin: -23px 0 16px;
    }
}

@media (max-width: 568px) {
    form .name {
        font-size: 11px;

    }

    form input {
        width: 17rem;
    }

    form .mane .typenum {
        width: 38px;
    }
}
</style>
