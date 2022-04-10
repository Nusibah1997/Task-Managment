<template>
  <div>
    <v-app-bar color="#fff" flat outlined dense elevation="1">
      <v-btn outlined class="mx-2 rounded-lg" icon color="info">
        <v-icon dark> mdi-home-outline </v-icon>
      </v-btn>

      <v-btn class="rounded-lg" outlined icon color="info">
        <v-icon dark> mdi-view-quilt-outline </v-icon>
      </v-btn>
      <v-col cols="12" sm="3">
        <v-text-field
          class="mx-4 rounded-lg"
          flat
          hide-details
          prepend-inner-icon="mdi-magnify"
          solo-inverted
        >
        </v-text-field>
      </v-col>
      <template v-slot:extension>
        <v-tabs centered> </v-tabs>
      </template>

      <div class="d-flex ml-auto">
        <v-dialog
          v-model="dialog"
          persistent
          max-width="600px"
          max-high="1000px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              fab
              dark
              color="info"
              class="mx-3 rounded-lg"
              v-bind="attrs"
              v-on="on"
            >
              <v-icon dark> mdi-plus </v-icon>
              <!-- <CardCreate/> -->
            </v-btn>
          </template>

          <form
            v-bind:class="classList"
            v-on:submit.prevent="createTaskInSectionI"
          >
            <v-card>
              <v-card-title>
                <v-icon large class="mx-3 rounded-lg" color="info">
                  mdi-view-quilt-outline
                </v-icon>
                <span class="text-h5 font-weight-bold">Design Team Board</span>
                <div class="ml-auto">
                  <v-btn
                    class="rounded-lg ma-5"
                    outlined
                    icon
                    large
                    color="red"
                    @click="dialog = false"
                  >
                    <v-icon dark> mdi-close </v-icon>
                  </v-btn>
                </div>
              </v-card-title>

              <v-container>
                <v-row>
                  <v-col cols="12" sm="4">
                    <!-- <select v-model="colName" class="select_rang">
                   <v-icon dark> mdi-menu-down </v-icon>
                  <option
                    v-for="(section, index) in sections"
                    :key="section.id"
                    
                    :title="section.title"
                    :tasks="section.tasks"
                    :sectionIndex="section.index"
                    @change="movingTasks"
                    selected
                  >
                 {{index}}   {{ section.title }}
                  </option>
                </select> -->

                    <!-- <v-select
                      :items="arr"
                      item-text="item.title"
                      item-value="item.id"
                      v-model="item.index"
                      rounded="pill"
                      outlined
                    > -->
                      <!-- <v-select 
                :options="options" 
                label="country" 
                :reduce=" title => section.title" /> -->

                      <!-- v-for="(section) in sections"
                :key="section.id" 
                {{ section.title}} -->
                    <!-- </v-select> -->
                  </v-col>
                  <v-col cols="12" sm="4">
                    <v-select
                      :items="tags"
                      color="info"
                      v-model="rangTask"
                      rounded
                      small
                      outlined
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="4"></v-col>
                  <v-spacer></v-spacer>
                  <div class="d-flex mb-6">
                    <v-card-title>
                      <span class="text-h4 font-weight-bold">Task:</span>
                    </v-card-title>

                    <v-text-field
                      class="text-h4 font-weight-bold"
                      v-model="content"
                      type="text"
                      v-on:focusin="startEditing"
                      v-on:focusout="finishEditing"
                      required
                    ></v-text-field>
                  </div>

                  <v-card-title>
                    <span class="text-h5 font-weight-bold">Members</span>
                  </v-card-title>

                  <v-col cols="12">
                    <v-btn outlined large class="mx-4" icon color="info">
                      <v-icon dark> mdi-plus </v-icon>
                    </v-btn>

                    <v-avatar size="40">
                      <img
                        src="https://cdn.vuetifyjs.com/images/lists/4.jpg"
                        alt="John"
                      />
                    </v-avatar>

                    <v-avatar class="ml-n3" size="40">
                      <img
                        src="https://cdn.vuetifyjs.com/images/lists/3.jpg"
                        alt="John"
                      />
                    </v-avatar>
                    <v-avatar class="ml-n3" color="#80D8FF" size="40">
                      <span class="blue--text text-h12 font-weight-medium"
                        >MK</span
                      >
                    </v-avatar>
                  </v-col>

                  <v-col cols="4">
                    <v-card-title>
                      <span class="text-h5 font-weight-bold">Description</span>
                    </v-card-title>
                  </v-col>

                  <v-col cols="12">
                    <v-textarea
                      rounded
                      filled
                      name="input-7-4"
                      placeholder="Please create few alternatives to the profile screen. All the
           screen should on our design system . if you have any
            suggestions you can write a comment on this task."
                    ></v-textarea>
                  </v-col>

                  <v-col cols="4">
                    <v-card-title>
                      <span class="text-h5 font-weight-bold">Comments</span>
                    </v-card-title>
                  </v-col>

                  <v-col cols="12">
                    <v-textarea
                      prepend-inner-icon="mdi-format-italic "
                      rounded
                      filled
                      required
                      ><v-icon>mdi-format-italic</v-icon></v-textarea
                    >
                  </v-col>
                </v-row>
              </v-container>
              <small>*indicates required field</small>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue-darken-1" text @click="dialog = false">
                  Close
                </v-btn>
                <v-btn
                  color="blue-darken-1"
                  text
                  v-if="isActive || contentExists"
                  type="submit"
                  class="add-button"
                >
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </form>
        </v-dialog>

        <v-badge dot color="success" avatar bordered overlap>
          <v-btn class="rounded-lg" outlined icon color="info">
            <v-icon dark> mdi-bell-outline </v-icon>
          </v-btn>
        </v-badge>
        <v-btn outlined class="mx-3 rounded-lg" icon color="info">
          <v-icon dark> mdi-help-circle-outline </v-icon>
        </v-btn>

        <v-btn class="rounded-lg" outlined icon color="info">
          <v-icon dark> mdi-cog-outline </v-icon>
        </v-btn>

        <v-avatar class="ml-6" size="50">
          <img src="https://cdn.vuetifyjs.com/images/lists/4.jpg" alt="John" />
        </v-avatar>
      </div>
    </v-app-bar>
  </div>
</template>

<script>
// import CardCreate from "./cardCreate.vue";
import { mapActions, mapState } from "vuex";
export default {

  data () {


    return {

      content: "",
      rangTask: [],
      colName: [],
      isActive: false,
      dialog: false,
       title: "",
       arr:[
        //  {
      //    title:"Backlog",
      //    id:0,
      //    index:0
      //  },
      //  {
      //    title:"In Progress",
      //    id:1,
      //    index:1
      //  },
      //  {
      //    title:"In Review",
      //    id:2,
      //    index:2
      //  },
      //  {
      //    title:"Done",
      //    id:3,
      //    index:3
      //  }
       
       
       
       ],
      //  sectionIndex:3,

      tags: [
        {
          color: "red",
          text: "High",
        },
        {
          color: "orang",
          text: "Medium",
        },
        {
          color: "yellow",
          text: "Low",
        },
      ],
      };



  },
  // components: { CardCreate },


   computed: {
    ...mapState(["sections"]),

    classList() {
      let classList = [];
      if (this.isActive) {
        classList.push("active");
      }
      return classList;
    },
    contentExists() {
      return this.content.length > 0;
    },

      //  arr : this.sections.map(e => {id:e.id; value:e.title})
  },
  methods: {
    ...mapActions(["createTaskInSection"]),
    createTaskInSectionI() {
      console.log(this.content);
      console.log(this.colName);
      console.log(this.rangTask);
      console.log(this.sectionIndex);

      this.createTaskInSection({
        content: this.content,
        colName: this.colName,
        rangTask: this.rangTask,
        sections: this.sections,
        sectionIndex: this.sectionIndex,
      });
      // this.sectionIndex = 0;
      this.content = "";
      this.colName = "";
      this.rangTask = "";
    },
    startEditing() {
      this.isActive = true;
    },
    finishEditing() {
      this.isActive = false;
    },
    movingTasks: function () {
      this.$store.dispatch("saveSections", { sections: this.sections });
    },
  },
};
</script>

<style></style>
