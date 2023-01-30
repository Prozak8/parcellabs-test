<template>
  <div>
    <TrackAndTrace :options="trackAndTraceOptions" />
    <p>Times rendered: {{ timesRendered }}</p>
  </div>
</template>

<script>
import TrackAndTrace from '@parcellab/js-plugin-utils/vue';

export default {
  name: 'App',
  components: {
    TrackAndTrace,
  },
  data() {
    return {
      timesRendered: 0,
    }
  },
  computed: {
    trackAndTraceOptions() {
      return {
        customTranslations: {
          sv: {
            more: 'VISA MER',
            // '' or null results in original text rendering.
            containsOf: ' ',
            deliveries: 'paket',
          },
        },
        onRendered: () => {
          this.timesRendered++;
          import('./parcellabs.styl');

          if (document.querySelector('.pl-icon-hr-container')) {
            this.replaceOrderStatusIconWithLoadingBar();
          }
        },
      };
    },
  },
  methods: {
    replaceOrderStatusIconWithLoadingBar() {
      const orderStatusContainer = document.querySelector('.pl-icon-hr-container');

      const orderStatusIcons = Array.from(orderStatusContainer.children).filter(el => el.childNodes.length);
      orderStatusContainer.id = 'order-status-container';

      const orderStatusIndex = orderStatusIcons.map(el => {
        if (el.children.length) {
          return el.children[0].classList.contains('pl-state-icon-active');
        }
      }).indexOf(true);

      const loadingBar = document.createElement('div');
      loadingBar.classList.add('loading-bar');

      const loadingBarPercentage = [0, 50, 100];

      const loadingBarInner = document.createElement('div');
      loadingBarInner.classList.add('loading-bar-inner');
      loadingBarInner.style.width = `${loadingBarPercentage[orderStatusIndex]}%`;

      loadingBar.appendChild(loadingBarInner);

      const header = document.createElement('div');
      header.classList.add('pl-box-heading', 'order-status-header');
      header.textContent = 'Spåra ditt paket';

      const labels = ['Lagerhanteras', 'Levererad'];

      const labelContainer = document.createElement('div');
      labelContainer.classList.add('order-status-labels');

      labels.forEach(label => {
        const span = document.createElement('span');
        span.textContent = label;
        labelContainer.appendChild(span);
      });

      orderStatusContainer.textContent = '';
      orderStatusContainer.appendChild(header);
      orderStatusContainer.appendChild(loadingBar);
      orderStatusContainer.appendChild(labelContainer);
    },
  },
}
</script>

<style>
</style>
