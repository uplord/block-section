<template>
  <div :id="id" class="block alignwide"
    :class="{ 'animate js_section': setAnimate || animate == 'true', 'visible': visible }">
    <div class="sections">
      <div v-for="(section, key) in allSections" :key="key" ref="section" class="section"
        :class="[
          section.class, {
            'visible': section.visible,
            'loaded': section.loaded,
            'section-icons': section.icons
          }
        ]">

        <div class="image-wrap" v-if="section.image">
          <img :src="section.image" :alt="section.title" loading="lazy" :width="section.imageWidth" />
        </div>

        <div class="text-wrap inner-container">
          <h4 class="primary">{{ section.subtitle }}</h4>
          <h3 class="h2">{{ section.title }}</h3>
          <div class="inner-container" v-if="section.content" v-html="section.content"></div>
          <Buttons :buttons="section.buttons" />
        </div>

        <!--
        <div class="image-wrap" v-if="section.icons">
          <BlockIcons :visible="section.icons_visible" :icons="section.icons" stacked="true" hide_animation="true" />
        </div>

        <div class="image-wrap" v-if="section.contact_form">
          <FormContact />
        </div>
        -->

      </div>
    </div>
  </div>

</template>

<script>
export default {
  props: ['animate', 'visible', 'sections', 'id'],
  data() {
    return {
      setAnimate: false,
      allSections: [...this.sections],
    }
  },
  mounted() {

  },
  watch: {
    visible(value) {
      const vm = this
      this.setAnimate = true

      if (value == true) {
        const observer = new IntersectionObserver((entries, observer) => {
          entries.forEach((entry, i) => {
            if (entry.isIntersecting) {
              const section = vm.all_sections[entry.target.index];
              section.visible = true;

              if (section.icons) {
                section.icons_visible = true;
              }

              setTimeout(() => {
                section.loaded = true;
              }, 300);
            }
          });
        }, {
          threshold: 0,
          rootMargin: "0px 0px -100px 0px",
        });

        this.$refs.section.forEach( function (el, index) {
          el.index = index
          observer.observe(el);
        })
      }
    }
  },
}
</script>

<style lang="less">
  @import 'style';
</style>