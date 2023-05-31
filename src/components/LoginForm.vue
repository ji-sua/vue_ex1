<template>
  <v-container style="transform: translateY(50%)">
    <v-row class="text-center">
      <v-col cols="12">
        <v-card class="mx-auto px-6 py-8" max-width="380">
          <v-form v-model="form">
            <v-text-field
              v-model="userId"
              class="mb-2"
              clearable
              label="이메일"
              required
            ></v-text-field>

            <v-text-field
              v-model="userPassword"
              clearable
              type="password"
              label="비밀번호"
              placeholder="비밀번호를 입력하세요!"
              required
            ></v-text-field>
            <br />
            <v-btn
              block
              color="success"
              size="large"
              type="submit"
              variant="elevated"
              @click.prevent="login"
            >
              로그인
            </v-btn>
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { Buffer } from "buffer";
export default {
  name: "LoginForm",

  data: () => ({
    userId: "",
    userPassword: "",
  }),

  methods: {
    login() {
      let loginData = {};
      loginData.userId = this.userId;
      loginData.userPassword = this.userPassword;

      const header = {
        alg: "HS256",
        typ: "JWT",
      };
      const payload = {
        id: this.userId,
        password: this.userPassword,
      };

      const encodeHeader = Buffer.from(JSON.stringify(header))
        .toString("base64")
        .replaceAll("=", "");
      const encodePayload = Buffer.from(JSON.stringify(payload))
        .toString("base64")
        .replaceAll("=", "");
      console.log("header : ", encodeHeader);
      console.log("payload : ", encodePayload);

      const crypto = require("crypto");
      const signature = crypto
        .createHmac("sha256", "secret_key")
        .update(`${encodeHeader}.${encodePayload}`)
        .digest("base64")
        .replaceAll("=", "");

      console.log("signature : ", signature);

      const jwt = `${encodeHeader}.${encodePayload}.${signature}`;
      console.log("jwt : ", jwt);
    },
  },
};
</script>
