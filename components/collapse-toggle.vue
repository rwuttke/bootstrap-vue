<template>
  <span @click.stop.prevent="toggle($event)" :class="{ active: isActive }">
    <slot></slot>
  </span>
</template>

<script>
  // export component object
  export default {
    replace: true,
    computed: {},
    data () {
      return {
        isActive: false
      }
    },
    props: {
      target: {
        type: String,
        default: ''
      },
      targetGroup: {
        type: String,
        default: ''
      }
    },
    methods: {
      toggle() {
        // if both attributes missing, ignore
        if (!this.target && !this.targetGroup) return;
        
        // switch active status
        this.isActive = !this.isActive;

        // broadcast accordion toggle if both id and group are set otherwise broadcast collapse
        // we also use dispatch to tell other components about the change
        if (this.target && this.targetGroup) {
          this.$root.$emit('toggled::accordion', {id: this.target, group: this.targetGroup})
        } else {
          this.$root.$emit('toggled::collapse', {id: this.target, group: this.targetGroup})
        }
      }
    },
  }

</script>
