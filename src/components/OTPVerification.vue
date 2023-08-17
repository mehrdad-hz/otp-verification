<template>
  <div class="otp-verification">
    <h2>OTP Verification</h2>
    <div class="otp-input-container">
      <input
        v-for="(digit, index) in otpDigits"
        :key="index"
        ref="otpInputs"
        v-model="otp[index]"
        @input="handleInputChange(index)"
        @keydown="handleKeyDown(index, $event)"
        maxlength="1"
      />
    </div>
    <button @click="validateOTP">Validate OTP</button>
    <p v-if="success" class="success-message">OTP Verified Successfully!</p>
    <p v-if="invalid" class="invalid-message">Invalid OTP. Please try again.</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  data() {
    return {
      otpDigits: 6,
      otp: Array(6).fill(""),
      success: false,
      invalid: false,
      otpInputRefs: [] as HTMLInputElement[],
    };
  },
  methods: {
    handleInputChange(index: number): void {
      if (this.otp[index].length > 1) {
        this.otp[index] = this.otp[index][0];
      }
    },
    handleKeyDown(index: number, event: KeyboardEvent): void {
      if (event.key >= "0" && event.key <= "9") {
        if (index < this.otpDigits) {
          // Clear the current digit before entering the new one
          this.otp[index] = "";
          this.otp[index] = event.key;

          if (index < this.otpDigits - 1) {
            // Move focus to the next input
            this.otpInputRefs[index + 1]?.focus();
          }
        }
      } else if (event.key === "Backspace" && index > 0) {
        // Move focus to the previous input
        this.otpInputRefs[index - 1]?.focus();
      }
    },
    validateOTP(): void {
      const validOTP: string = "123456";
      const enteredOTP: string = this.otp.join("");

      if (enteredOTP === validOTP) {
        this.success = true;
        this.invalid = false;
      } else {
        this.success = false;
        this.invalid = true;
        setTimeout(() => {
          this.invalid = false;
        }, 2000);
      }
    },
  },

  mounted() {
    this.otpInputRefs = this.$refs.otpInputs as HTMLInputElement[];
    this.otpInputRefs[0]?.focus();
  },
});
</script>

<style scoped>
.otp-verification {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 20px;
}

.otp-input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

input {
  width: 30px;
  height: 30px;
  text-align: center;
  border: 1px solid #ccc;
  border-radius: 5px;
}

input:focus {
  outline: none;
  border-color: #007bff;
}

button {
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.success-message {
  color: green;
  margin-top: 10px;
}

.invalid-message {
  color: red;
  margin-top: 10px;
}
</style>
