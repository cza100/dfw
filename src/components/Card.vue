<template>
  <v-container>
    <v-container mt-10>
      <v-card elevation="2" color="red" min-height="8rem" dark @click="a_click">
        <v-card-title v-text="a_t"></v-card-title>
        <v-card-text v-text="a_d"></v-card-text>
      </v-card>
      <v-dialog v-model="dialog" persistent max-width="600px">
        <template v-slot:activator="{ on, attrs }">
          <v-btn text v-bind="attrs" v-on="on" v-show="a_flip">
            编辑
          </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="text-h5">机会编辑</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    label="名称*"
                    required
                    v-model="a_e_t"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    label="效果*"
                    required
                    v-model="a_e_d"
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog = false">
              关闭
            </v-btn>
            <v-btn color="blue darken-1" text @click="a_edit">
              提交
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-container>

    <v-container mt-10>
      <v-card
        elevation="2"
        color="blue"
        min-height="8rem"
        dark
        @click="b_click"
      >
        <v-card-title v-text="b_t"></v-card-title>
        <v-card-text v-text="b_d"></v-card-text>
      </v-card>
      <v-dialog v-model="dialog" persistent max-width="600px">
        <template v-slot:activator="{ on, attrs }">
          <v-btn text v-bind="attrs" v-on="on" v-show="b_flip">
            编辑
          </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="text-h5">命运编辑</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    label="名称*"
                    required
                    v-model="b_e_t"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    label="效果*"
                    required
                    v-model="b_e_d"
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog = false">
              关闭
            </v-btn>
            <v-btn color="blue darken-1" text @click="b_edit">
              提交
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: "Card",

  data: () => ({
    dialog: false,
    a_flip: false,
    a_id: 0,
    a_t: "机会",
    a_d: "",
    a_e_t: "",
    a_e_d: "",
    b_flip: false,
    b_id: 0,
    b_t: "命运",
    b_d: "",
    b_e_t: "",
    b_e_d: "",
  }),

  methods: {
    async a_click() {
      if (this.a_flip == false) {
        let id = "a" + Math.floor(Math.random() * 23 + 1);
        id = "a1";
        let res = await this.$axios.get("http://150.158.104.207/get/a1");

        this.a_flip = true;
        this.a_id = id;
        this.a_t = res.data[0];
        this.a_d = res.data[1];
      } else {
        this.a_flip = false;
        this.a_id = 0;
        this.a_t = "机会";
        this.a_d = "";
      }
    },
    async b_click() {
      if (this.b_flip == false) {
        let id = "b" + Math.floor(Math.random() * 23 + 1);
        let res = await this.$axios.get("http://150.158.104.207/get/id");

        this.b_flip = true;
        this.b_id = id;
        this.b_t = res.data[0];
        this.b_d = res.data[1];
      } else {
        this.b_flip = false;
        this.b_id = 0;
        this.b_t = "命运";
        this.b_d = "";
      }
    },
    async a_edit() {
      let res = await this.$axios.post("http://150.158.104.207/edit", {
        id: this.a_id,
        t: this.a_e_t,
        d: this.a_e_d,
      });

      if (res.data["error"] == 0) {
        this.a_t = this.a_e_t;
        this.a_d = this.a_e_d;
      }
    },
    async b_edit() {
      let res = await this.$axios.post("http://150.158.104.207/edit", {
        id: this.b_id,
        t: this.b_e_t,
        d: this.b_e_d,
      });
      if (res.data["error"] == 0) {
        this.b_t = this.b_e_t;
        this.b_d = this.b_e_d;
      }
    },
  },
};
</script>
