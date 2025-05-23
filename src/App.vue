<script>
import BaseButton from "@/components/BaseButton.vue";

import BaseLabel from "@/components/BaseLabel.vue";
import BaseMessage from "@/components/BaseMessage.vue";
import AppPasswordStrength from "./components/AppPasswordStrength.vue";
import AppCharacterSelector from "@/components/AppCharacterSelector.vue";
import AppPasswordLengthInput from "@/components/AppPasswordLengthInput.vue";
const upperCaseLetters = [
  "A",
  "B",
  "C",
  "D",
  "E",
  "F",
  "G",
  "H",
  "I",
  "J",
  "K",
  "L",
  "M",
  "N",
  "O",
  "P",
  "Q",
  "R",
  "S",
  "T",
  "U",
  "V",
  "W",
  "X",
  "Y",
  "Z",
];
const lowerCaseLetters = [
  "a",
  "b",
  "c",
  "d",
  "e",
  "f",
  "g",
  "h",
  "i",
  "j",
  "k",
  "l",
  "m",
  "n",
  "o",
  "p",
  "q",
  "r",
  "s",
  "t",
  "u",
  "v",
  "w",
  "x",
  "y",
  "z",
];

const numbers = [
  "1",
  "2",
  "3",
  "4",
  "5",
  "6",
  "7",
  "8",
  "9",
  "0",
  "1",
  "2",
  "3",
  "4",
  "5",
  "6",
  "7",
  "8",
  "9",
  "0",
];
const symbols = [
  "!",
  "#",
  "$",
  "%",
  "&",
  "'",
  "(",
  ")",
  "*",
  "+",
  ",",
  "-",
  ".",
  "/",
  ":",
  ";",
  "<",
  "=",
  ">",
  "?",
  "@",
  "[",
  "]",
  "^",
  "_",
  "{",
  "|",
  "}",
  "~",
];
const passphrase = [];

export default {
  name: "app",
  components: {
    AppCharacterSelector,
    BaseButton,
    BaseLabel,
    AppPasswordLengthInput,
    BaseMessage,
    AppPasswordStrength,
  },
  mounted() {
    document.title = "Passwortgenerator";
  },
  computed: {
    isPasswordGenerated() {
      return this.password.length > 0;
    },
  },
  data() {
    return {
      password: [],
      labelArray: [],
      message: "",
      textColorMessage: "text-red-600",
      copyIcon: "/copy.png",
      generateButtonStatus: true,
    };
  },
  methods: {
    handleInput() {
      if (this.generateButtonStatus == true) {
        this.generatePassword();
      } else {
        this.outputErrorCharacters();
      }
    },
    generatePassword() {
      this.resetPasswordVariables();
      this.addSelectedCharactersToPassphrase();
      this.shufflePassphrase();
      this.generatePasswordfromPassphrase();
    },
    resetPasswordVariables() {
      this.deletePassphrase();
      this.password.length = 0;
      this.outputInformation("");
      this.changeCheckedToCopyIcon();
    },
    addSelectedCharactersToPassphrase() {
      const charGroup = {
        Großbuchstaben: upperCaseLetters,
        Kleinbuchstaben: lowerCaseLetters,
        Zahlen: numbers,
        Symbole: symbols,
      };

      this.labelArray.forEach((label) => {
        if (charGroup[label]) {
          this.addCharsToPassphrase(charGroup[label]);
        }
      });
    },
    shufflePassphrase() {
      var m = passphrase.length,
        t,
        i;
      while (m) {
        i = Math.floor(Math.random() * m--);
        t = passphrase[m];
        passphrase[m] = passphrase[i];
        passphrase[i] = t;
      }
    },
    generatePasswordfromPassphrase() {
      let n = 0;
      var pre_password = "";
      while (n < this.passwordLength) {
        n++;
        const randomIndex = Math.floor(Math.random() * passphrase.length);
        const item = passphrase[randomIndex];
        pre_password += item;
      }
      this.password[0] = pre_password;
    },

    // emit function for label array
    updateLabelAndEnableButton(array) {
      this.resetOutput();
      this.setLabelArray(array);
      this.flipGenerateButtonStatus(true);
    }, 
    setLabelArray(array) {
      this.labelArray = array;
    },
    flipGenerateButtonStatus(Boolean) {
      this.generateButtonStatus = Boolean;
    },
    // emit function for length
    updatePasswordLength(Number) {
      this.passwordLength = Number;
    },

    addCharsToPassphrase(value) {
      passphrase.push(...value);
    },

    deletePassphrase() {
      passphrase.length = 0;
    },

    handlePasswordCopying() {
      if (this.password.length != 0) {
        this.copyPasswordandOutputCopyMessage();
        this.changeCopyIconToChecked();
      } else {
        this.changeIconToWrong();
      }
    },
    copyPasswordandOutputCopyMessage() {
      navigator.clipboard.writeText(this.password[0]);
    },
    outputErrorCharacters() {
      this.outputInformation(
        "Es wurde keine Zeichengruppe ausgewählt!",
        "text-red-600"
      );
    },

    errorNoChars() {
      this.outputErrorCharacters();
      this.flipGenerateButtonStatus(false);
      this.deletePassphrase();
    },
    resetOutput() {
      this.outputInformation("")
    },
    outputInformation(text, kind) {
      this.message = text;
      this.textColorMessage = kind;
    },
    changeCopyIconToChecked() {
      this.copyIcon = "/check.png";
    },
    changeCheckedToCopyIcon() {
      this.copyIcon = "/copy.png";
    },
    changeIconToWrong() {
      this.copyIcon = "/wrong.png";
    },
  },
};
</script>

<template>
  <div class="flex items-center justify-center h-screen">
    <div
      class="bg-[#CAF0F8] w-80 px-4 py-2 rounded-lg animate-fade animate-once animate-duration-200 animate-ease-linear shadow-2xl"
    >
      <div>
        <h1 class="text-lg ml-16">Passwortgenerator</h1>
        <div class="grid grid-cols-2 grid-rows-1 w-72 mt-3">
          <base-label
            class="justify-self-start w-60 rounded-lg"
            :labelText="password[0]"
          ></base-label>
          <BaseButton
            class="h-10 w-10 justify-self-end hover:animate-pulse"
            @gotClicked="handlePasswordCopying"
            :icon-src="copyIcon"
          ></BaseButton>
        </div>
        <base-message
          :labelMessage="message"
          :textColor="textColorMessage"
        ></base-message>
        <AppCharacterSelector
          @update_label_array="updateLabelAndEnableButton($event)"
          @error_empty_label_array="errorNoChars()"
        ></AppCharacterSelector>
        <AppPasswordLengthInput
          @update_password_length="updatePasswordLength($event)"
        ></AppPasswordLengthInput>
        <div class="grid grid-cols-2 mt-3 mb-3">
          <BaseButton
            buttonLabel="Generiere"
            @gotClicked="handleInput"
            
            :class="
              generateButtonStatus ? 'cursor-pointer' : 'cursor-not-allowed'
            "
            icon-src="noIcon"
            class="h-10 animate-wiggle hover:animate-none"
          ></BaseButton>
          <AppPasswordStrength
            v-if="isPasswordGenerated"
            :password="password[0]"
            :labelArray="labelArray"
            class="mx-2"
          ></AppPasswordStrength>
        </div>
      </div>
    </div>
  </div>
</template>
