<script>
  import animateMixin from '../mixins/animate';
  import eventHub from '../event_hub';
  import tooltip from '../../vue_shared/directives/tooltip';
  import { spriteIcon } from '../../lib/utils/common_utils';

  export default {
    directives: {
      tooltip,
    },
    mixins: [animateMixin],
    props: {
      issuableRef: {
        type: String,
        required: true,
      },
      canUpdate: {
        required: false,
        type: Boolean,
        default: false,
      },
      titleHtml: {
        type: String,
        required: true,
      },
      titleText: {
        type: String,
        required: true,
      },
      showInlineEditButton: {
        type: Boolean,
        required: false,
        default: false,
      },
    },
    data() {
      return {
        preAnimation: false,
        pulseAnimation: false,
        titleEl: document.querySelector('title'),
      };
    },
    computed: {
      pencilIcon() {
        return spriteIcon('pencil', 'link-highlight');
      },
    },
    watch: {
      titleHtml() {
        this.setPageTitle();
        this.animateChange();
      },
    },
    methods: {
      setPageTitle() {
        const currentPageTitleScope = this.titleEl.innerText.split('·');
        currentPageTitleScope[0] = `${this.titleText} (${this.issuableRef}) `;
        this.titleEl.textContent = currentPageTitleScope.join('·');
      },
      edit() {
        eventHub.$emit('open.form');
      },
    },
  };
</script>

<template>
  <div class="title-container">
    <h2
      class="title"
      :class="{
        'issue-realtime-pre-pulse': preAnimation,
        'issue-realtime-trigger-pulse': pulseAnimation
      }"
      v-html="titleHtml"
    >
    </h2>
  </div>
</template>
