<template>
  <CreditHeader />
  <div class="container mb-3">
    <!-- Header -->
    <div class="row gap-3">
      <div v-if="showImage" class="col-3 col-sm-8 mx-auto col-md-3 col-lg-2 text-center">
        <img :src="placeholder" alt="Student Image" class="profile_image" />
      </div>
      <div class="col-auto col-sm-12 col-md m-auto w-100">
        <div class="row">
          <div class="col-12 text-center">
            <div class="mb-3 no-print" style="font-size: 15px;">
              [ <a href="javascript: window.print()">Print</a> ]
              [
              <!-- toggle language -->
              <span v-if="currentLang === 'en'">
                <a href="javascript: void(0)" @click="currentLang = 'th'">เปลี่ยนเป็น Thai</a>
              </span>
              <span v-else>
                <a href="javascript: void(0)" @click="currentLang = 'en'">เปลี่ยนเป็น English</a>
              </span>
              ]
              [ <a @click="showImage = !showImage" href="javascript: void(0)">Toggle image</a> ]
            </div>
            <h1 class="mb-0">{{ headerData.name }}</h1>
            <h5 class="text-secondary">({{ headerData.nickname }})</h5>
          </div>
          <div class="col-6">
            <div class="mb-3">
              <div v-for="(item, index) in headerData.left_col" :key="index">
                <template v-if="item.link">
                  <a class="mb-0 sidebar-text" :href="item.link" target="_blank">{{
                    item.title
                  }}</a>
                </template>
                <template v-else>
                  <p class="mb-0 sidebar-text">{{ item.title }}</p>
                </template>
              </div>
            </div>
          </div>
          <div class="col-6 text-end">
            <div v-for="(item, index) in headerData.right_col" :key="index">
              <template v-if="item.link">
                <a class="sidebar-text" :href="item.link" target="_blank">{{ item.title }}</a>
              </template>
              <template v-else>
                <p class="mb-0 sidebar-text">{{ item.title }}</p>
              </template>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Body -->
  <div class="container content-body">
    <div class="row">
      <div
        v-for="(section, sectionName) in bodyData"
        :key="sectionName"
        class="col-12 col-md-6 section-bottom"
      >
        <h4 class="mb-0">{{ section.title }}</h4>
        <hr />
        <!-- check if section.content is string -->
        <template v-if="typeof section.content === 'string'">
          <div v-html="section.content"></div>
        </template>
        <div v-else v-for="(content, index) in section.content" :key="index">
          <template v-if="typeof content === 'string'">
            <div v-html="content"></div>
          </template>
          <template v-else-if="typeof content === 'object'">
            <template v-if="content.link">
              <a :href="content.link" target="_blank">{{ content.title }}</a>
            </template>
            <template v-else>
              <h5 class="mb-1 sub-heading">{{ content.title }}</h5>
            </template>
            <!-- Recursively render nested content -->
            <template v-if="content.content">
              <template v-for="(item, itemIndex) in content.content" :key="itemIndex">
                <div class="mb-1" v-if="typeof item === 'string'">
                  <li class="mb-0">
                    <v-html v-html="item"></v-html>
                  </li>
                </div>
                <div class="mb-1" v-else-if="typeof item === 'object'">
                  <template v-if="item.link">
                    <a :href="item.link" target="_blank">{{ item.title }}</a>
                  </template>
                  <template v-else>
                    <li class="mb-0"><div v-html="item"></div></li>
                  </template>
                  <!-- Add recursion for deeper nesting if needed -->
                </div>
              </template>
            </template>
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CreditHeader from "./CreditHeader.vue";
import placeholder from "@/assets/placeholder.jpg";
import EN_lang from "@/translate/en.json";
import TH_lang from "@/translate/th.json";

export default {
  name: "App",
  components: {
    CreditHeader
  },
  data() {
    return {
      showImage: true,
      placeholder,
      currentLang: "th",
      headerData: {},
      bodyData: {}
    };
  },
  methods: {
    selectLang(language) {
      // Fetch language data dynamically based on the currentLang
      // Update headerData and bodyData with the fetched data
      switch (language) {
        case "en":
          this.headerData = EN_lang.header;
          this.bodyData = EN_lang.body;
          break;
        case "th":
          this.headerData = TH_lang.header;
          this.bodyData = TH_lang.body;
          break;
        // Add more cases for other languages as needed
      }
    }
  },
  watch: {
    currentLang: "selectLang"
  },
  beforeMount() {
    this.selectLang(this.currentLang);
  }
};
</script>

<style></style>
